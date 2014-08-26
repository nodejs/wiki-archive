TL;DR jump to the [conclusion](https://github.com/joyent/node/wiki/Optimizing-_unrefActive#conclusion).

# Introduction

Some internal modules use timers to implement their timeout logic. For instance, `lib/net.js` registers a timer every time some activity happens on a socket. If a socket doesn't have any activity after some time, its timeout callback is triggered.

Because there can be a very large number of sockets created by a Node.js process, a large number of timers can be registered in a very short amount of time. For this reason, internal modules do not use the same timer facilities as user-land modules, which would be too costly in terms of resources.

Instead, they use an internal function named `_unrefActive`.

## The problem

### How to reproduce it

As described in [a recent GitHub issue](https://github.com/joyent/node/issues/8160) `_unrefActive` seems to take a significant part of CPU time when Node.js is under some heavy HTTP load. For instance, when running the following Node.js server:
```
var http = require('http');
var server = http.createServer(function (req, res) {
    res.end();
})

server.listen(4242, function() {
    console.log('Server listening on port 4242...');
});
```
and if we put it under some heavy HTTP load with wrk:
```
wrk -t12 -c400 -d30s http://127.0.0.1:4242/
```
the profiling results show that `_unrefActive` spends a lot of time on CPU:
```
ticks parent  name
  10456   34.4%  LazyCompile: *exports._unrefActive timers.js:517:32
  10275   98.3%    LazyCompile: *onread net.js:492:16

   4283   14.1%  Stub: CompareICStub {1}
   4283  100.0%    LazyCompile: *exports._unrefActive timers.js:517:32
   4215   98.4%      LazyCompile: *onread net.js:492:16

   2020    6.7%  node::Parser::Execute(v8::FunctionCallbackInfo<v8::Value> const&)
   1326   65.6%    LazyCompile: ~socketOnData _http_server.js:339:24
   1323   99.8%      LazyCompile: *emit events.js:68:44
   1322   99.9%        LazyCompile: *readableAddChunk _stream_readable.js:134:26
   1320   99.8%          LazyCompile: *onread net.js:492:16
    688   34.1%    LazyCompile: socketOnData _http_server.js:339:24
    688  100.0%      LazyCompile: *emit events.js:68:44
    688  100.0%        LazyCompile: *readableAddChunk _stream_readable.js:134:26
    688  100.0%          LazyCompile: *onread net.js:492:16

   1563    5.1%  _getpid
```

Because `_unrefActive`'s purpose is to allow internal modules to create a large number of timers efficiently, it is an issue.

### What's happening under the hood

Whenever a timer is added to handle timeout for a socket (or any other object that needs to implement some timeout logic), `_unrefActive` adds a timer to a priority queue. When an internal timer expires, `unrefTimeout` (also implemented in `lib/timers.js`) is called. It processes the priority queue and calls the timeout callback of each timer that has expired.

Using a priority queue allows quick and easy retrieval of expired timers when they expire, because they all are at the beginning of the list. However, the priority queue is implemented as a linked list. This means every time a timer is added, the list has to be traversed to determine its position in the queue. With a very large number of timer, this process can take a lot of CPU time.

In other words, the current implementation has the following characteristics:
* Cost of adding a timer: O(n).
* Cost of a timer timing out: O(1).

It is clear now why, when running a benchmark such as the one mentioned above, this solution shows poor performance. A lot of connections are created (thus a lot of timers are added to the queue) and only a few timeouts happen.

The current implementation is optimized for use cases when there's a lot of timeouts, and not a lot of timers 
added. In practice, this is contradictory because you can't have a lot of timeouts without a lot of timers added. The solution to this problem needs to at least have significant better performance when adding timers.

# Solutions

There are at least two other ways to implement a priority queue that have better performance characteristics than an ordered linked list with regard to adding timers:
* Using an unsorted array.
* Using a heap.

Finally, another popular implementation for timers management is called a timer wheel. It uses a more complex data structure and has not been implemented yet. Depending on the outcome of the comparison described in this document, it will be implemented and its performance will be tested.

## Using an unsorted array

### Pros

An unsorted array has the advantage of providing constant time addition to the priority queue. When running the same HTTP heavy benchmark as above, this shows a significant improvement. Here's the results from v8 profiler when running the same `wrk` benchmark with the same server, but this time with an unordered list as the underlying implementation:
```
ticks parent  name
   8365   27.6%  node::Parser::Execute(v8::FunctionCallbackInfo<v8::Value> const&)
   4225   50.5%    LazyCompile: ~socketOnData _http_server.js:339:24
   4220   99.9%      LazyCompile: *emit events.js:68:44
   4220  100.0%        LazyCompile: *readableAddChunk _stream_readable.js:134:26
   4220  100.0%          LazyCompile: *onread net.js:492:16
   4095   49.0%    LazyCompile: socketOnData _http_server.js:339:24
   4095  100.0%      LazyCompile: *emit events.js:68:44
   4095  100.0%        LazyCompile: *readableAddChunk _stream_readable.js:134:26
   4095  100.0%          LazyCompile: *onread net.js:492:16

   1798    5.9%  _getpid

   1203    4.0%  LazyCompile: *emit events.js:68:44
    867   72.1%    LazyCompile: *readableAddChunk _stream_readable.js:134:26
    867  100.0%      LazyCompile: *onread net.js:492:16
    214   17.8%    LazyCompile: *resume_ _stream_readable.js:717:17
    214  100.0%      LazyCompile: ~<anonymous> _stream_readable.js:711:30
    214  100.0%        LazyCompile: _tickCallback node.js:332:27
     43    3.6%    LazyCompile: *emitReadable_ _stream_readable.js:417:23
     43  100.0%      LazyCompile: ~<anonymous> _stream_readable.js:409:32
     43  100.0%        LazyCompile: _tickCallback node.js:332:27
     30    2.5%    LazyCompile: ~finish _http_outgoing.js:504:18
     30  100.0%      LazyCompile: *afterWrite _stream_writable.js:321:20
     30  100.0%        LazyCompile: ~<anonymous> _stream_writable.js:312:32
     30  100.0%          LazyCompile: _tickCallback node.js:332:27

    801    2.6%  LazyCompile: *exports._unrefActive timers.js:555:32
    778   97.1%    LazyCompile: *onread net.js:492:16

    692    2.3%  LazyCompile: socketOnData _http_server.js:339:24
    692  100.0%    LazyCompile: *emit events.js:68:44
    692  100.0%      LazyCompile: *readableAddChunk _stream_readable.js:134:26
    692  100.0%        LazyCompile: *onread net.js:492:16
```
We can see that `_unrefActive` is still a significant contributor. But this time, only 2.6% of the time spent on the CPU is spent executing `_unrefActive` (down from 34%). This benchmark was run from a Ubuntu VM with only 4 CPUs. The same benchmark ran from my MacOS X laptop shows that `_unrefActive`'s impact on overall performance is even less than 2%. Most of the time, it doesn't show up in the profiling smmary.

The number of requests/s also rises significantly, which indicates that overall performance is affected by this change.

### Cons

However, using an unsorted array also means that when a timer fires, `unrefTimeout` has to traverse the whole priority queue to determine which timer fired. This characteristic is not exposed by the benchmark above, since only a few timeouts happen. However, it is easy to build a very simple micro benchmark that exposes this issue.

We can run the following code and profile it:
```
var timers = require('timers');

var N = 100000;

var i = 0;
while (i < N) {
    var  someObject = { _onTimeout: function () { } };
    timers.enroll(someObject, i);
    timers._unrefActive(someObject);
    ++i;
}

setTimeout(function() {}, N);
```
to see that `unrefTimeout` is very costly:
```
Note: percentage shows a share of a particular caller in the total
  amount of its parent calls.
  Callers occupying less than 2.0% are not shown.

   ticks parent  name
  41831   52.2%  syscall

  19713   24.6%  LazyCompile: unrefTimeout timers.js:470:22

   9679   12.1%  Stub: LoadFieldStub
   9679  100.0%    LazyCompile: unrefTimeout timers.js:470:22

   4590    5.7%  Stub: CompareICStub
   4590  100.0%    LazyCompile: unrefTimeout timers.js:470:22
```
Basically, what this micro  benchmark does is to add a very large number of timers that all fire 1ms after the previous one. This means that every time `unrefTimeout` will be called, a lot of timers will be present in the priority queue.

We can build a macro-benchmark that is very similar to the previous micro-benchmark by writing the following server-code:
```
var http = require('http');
var reqIndex = 0;

var server = http.createServer(function (req, res) {
    reqIndex++;
    req.setTimeout(reqIndex, function() { });
});

server.listen(4242);
```
When stress-testing this server with the following wrk benchmark:
```
wrk -t12 -c5000 -d30s http://127.0.0.1:4242/
```

Note the use of 5000 connections instead of 1000 so that wrk can put a lot of stress on the server despite all requests timing out. Using 1000 simultaneous connections wouldn't highlight the issue. Using more simultaneous connections stresses the issue even more, as more timers are added and removed over the same period of time.

We can see that `unrefTimeout` is one of the top contributors of v8's profile:
```
[Bottom up (heavy) profile]:
  Note: percentage shows a share of a particular caller in the total
  amount of its parent calls.
  Callers occupying less than 2.0% are not shown.

   ticks parent  name
  18597   76.9%  syscall

    991    4.1%  LazyCompile: unrefTimeout timers.js:399

    968    4.0%  node::HandleWrap::Close(v8::Arguments const&)
    956   98.8%    LazyCompile: *Socket._destroy net.js:431
    869   90.9%      LazyCompile: *Socket.destroy net.js:488
    858   98.7%        LazyCompile: ~<anonymous> http.js:1935
    858  100.0%          LazyCompile: *emit events.js:53
    858  100.0%            LazyCompile: *Socket._onTimeout net.js:324
     87    9.1%      LazyCompile: *onSocketFinish net.js:194
     69   79.3%        LazyCompile: *Writable.end _stream_writable.js:327
     69  100.0%          LazyCompile: *Socket.end net.js:395
     69  100.0%            LazyCompile: ~socket.onend http.js:2000
     18   20.7%        LazyCompile: ~emit events.js:53
     18  100.0%          LazyCompile: *Writable.end _stream_writable.js:327
     18  100.0%            LazyCompile: *Socket.end net.js:395

    746    3.1%  Stub: CompareICStub
    746  100.0%    LazyCompile: unrefTimeout timers.js:399
```
Reducing the frequency of timeouts mitigates the impact of `unrefTimeout` on the overall performance, but even with 1 timeout out of 10 requests, it's still a significant contributor.


## Using a heap

A heap theoretically gives us a good balance between the cost of adding and finding a timer:
* Addition is O(log(n)).
* Retrieval of the next timer is also O(log(n)).

# Pros

Unsurprisingly, the micro-benchmark that highlighted the very bad performance of the unordered list implementation when a lot of timeouts happen shows that a heap implementation performs well in this case:
```
ticks parent  name
  71657   93.2%  syscall

   1610    2.1%  LazyCompile: *Heap._swap _heap.js:181:32
   1475   91.6%    LazyCompile: *Heap._down _heap.js:278:32
   1306   88.5%      LazyCompile: *Heap._down _heap.js:278:32
   1124   86.1%        LazyCompile: *Heap._down _heap.js:278:32
    946   84.2%          LazyCompile: *Heap._down _heap.js:278:32
    793   83.8%            LazyCompile: *Heap._down _heap.js:278:32
    153   16.2%            LazyCompile: *Heap.remove _heap.js:88:33
    178   15.8%          LazyCompile: *Heap.remove _heap.js:88:33
    178  100.0%            LazyCompile: *Heap.pop _heap.js:79:30
    182   13.9%        LazyCompile: *Heap.remove _heap.js:88:33
    182  100.0%          LazyCompile: *Heap.pop _heap.js:79:30
    182  100.0%            LazyCompile: ~<anonymous> native v8natives.js:1:1
    169   11.5%      LazyCompile: *Heap.remove _heap.js:88:33
    169  100.0%        LazyCompile: *Heap.pop _heap.js:79:30
    169  100.0%          LazyCompile: ~<anonymous> native v8natives.js:1:1
    134    8.3%    LazyCompile: *Heap.remove _heap.js:88:33
    134  100.0%      LazyCompile: *Heap.pop _heap.js:79:30
    134  100.0%        LazyCompile: ~<anonymous> native v8natives.js:1:1
```
This is due to the improvement from a O(n) to O(log n) process to determine which timer has expired. Results are similar with the macro-benchmark that simulates a large number of frequent timeouts.

# Cons

On the other hand, because adding a timer does not happen in constant time, the heavy HTTP benchmark shows that its performance is slightly worse than when using an unordered list:
```
 456 4.4% LazyCompile: *exports._unrefActive timers.js:534:32
```

Sometimes, the heap implementation itself shows as a significant cost. For instance, here's a sample from a profiling session on MacOS X with 10K concurrent connections:
```
435    3.4%  LazyCompile: *Heap._swap _heap.js:181:32
    397   91.3%    LazyCompile: *Heap._down _heap.js:278:32
    267   67.3%      LazyCompile: *Heap._down _heap.js:278:32
    190   71.2%        LazyCompile: *Heap._down _heap.js:278:32
    120   63.2%          LazyCompile: *Heap._down _heap.js:278:32
     65   54.2%            LazyCompile: *Heap._down _heap.js:278:32
     42   35.0%            LazyCompile: Heap.remove _heap.js:88:33
      5    4.2%            Stub: binaryWrite {5}
      5    4.2%            LazyCompile: *Heap.remove _heap.js:88:33
      3    2.5%            Stub: parent {4}
     53   27.9%          LazyCompile: Heap.remove _heap.js:88:33
     53  100.0%            LazyCompile: *exports._unrefActive timers.js:534:32
      6    3.2%          LazyCompile: *Heap.remove _heap.js:88:33
      6  100.0%            LazyCompile: *exports._unrefActive timers.js:534:32
      5    2.6%          Stub: binaryWrite {5}
      5  100.0%            LazyCompile: *exports._unrefActive timers.js:534:32
      4    2.1%          Stub: parent {4}
      4  100.0%            LazyCompile: *exports._unrefActive timers.js:534:32
     58   21.7%        LazyCompile: Heap.remove _heap.js:88:33
     58  100.0%          LazyCompile: *exports._unrefActive timers.js:534:32
     58  100.0%            LazyCompile: *onread net.js:492:16
      9    3.4%        LazyCompile: *Heap.remove _heap.js:88:33
      8   88.9%          LazyCompile: *exports._unrefActive timers.js:534:32
      8  100.0%            LazyCompile: *onread net.js:492:16
      1   11.1%          LazyCompile: ~exports._unrefActive timers.js:534:32
      1  100.0%            LazyCompile: *onread net.js:492:16
      6    2.2%        Stub: binaryWrite {5}
      6  100.0%          LazyCompile: *exports._unrefActive timers.js:534:32
      6  100.0%            LazyCompile: *onread net.js:492:16
    111   28.0%      LazyCompile: Heap.remove _heap.js:88:33
    111  100.0%        LazyCompile: *exports._unrefActive timers.js:534:32
    111  100.0%          LazyCompile: *onread net.js:492:16
     18    4.1%    LazyCompile: *Heap.remove _heap.js:88:33
     18  100.0%      LazyCompile: *exports._unrefActive timers.js:534:32
     18  100.0%        LazyCompile: *onread net.js:492:16
     11    2.5%    Stub: parent {4}
     11  100.0%      LazyCompile: *exports._unrefActive timers.js:534:32
     11  100.0%        LazyCompile: *onread net.js:492:16

    377    2.9%  LazyCompile: *exports._unrefActive timers.js:534:32
    361   95.8%    LazyCompile: *onread net.js:492:16
```

Still, the combined cost of `_unrefActive` and the heap implementation amounts to only a fraction of the cost of `_unrefActive`'s original implementation.

# Conclusion

The unordered list implementation is the top performer when tested with the HTTP heavy benchmark mentioned at the top of the [GitHub issue](https://github.com/joyent/node/issues/8160). However, it is clear that this implementation suffers from the same issues than the current one when most timers timeout. 

The heap implementation performs much better in the case when a lot of timeouts are triggered, but it's slightly slower than the unordered list implementation when tested under the HTTP heavy benchmark without timeouts.

My recommendation would be to favor the heap implementation over the unordered list one. The reason is that with an unordered list, although we would be sure to have the fastest implementation regarding adding timers, we would only move the problem to another point in time when timeouts fire.

A few questions remain:
* If we decide to go this way: can we use @tjfontaine's binaryheap module as our `lib/_heap.js` implementation (as it's done currently)?
* Do we want to integrate the heap implementation first and then benchmark the timer wheel? Or do we want to benchmark the timer wheel before merging any change?