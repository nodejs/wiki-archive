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

There are at least two other ways to implement a priority queue that have better performance characteristics than an ordered linked list with regards to adding timers:
* Using an unsorted array.
* Using a heap.

Finally, there is another popular implementation for timers management that uses a more complex data structure called a timer wheel.

## Using an unsorted array

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
We can see that `_unrefActive` is not a significant contributor. The number of requests/s also rises significantly, which indicates that overall performance is affected by this change.

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

It is not clear yet if that behavior represents a significant use case in production, but it is at least worth considering.

## Using a heap