# Introduction

Some internal modules use timers to implement their timeout logic. For instance, `lib/net.js` registers a timer every time some activity happens on a socket. If a socket doesn't have any activity after some time, its timeout callback is triggered.

Because there can be a very large number of sockets created by a Node.js process, a large number of timers can be registered in a very short amount of time. For this reason, internal modules do not use the same timer facilities as user-land modules, which would be too costly in terms of resources.

Instead, they use an internal function named `_unrefActive`.

## The problem

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

