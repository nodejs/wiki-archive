When editing this page please be as detailed as possible. Examples are encouraged!

## V8

0.12.0 ships with V8 version 3.28.73, which was released in August 2014.

## Resource Management

* On Unix soft `ulimit` values are ignored.

## Buffers

### Module API Changes

* `Buffer` class has been removed and replaced with a namespace. So `using node::Buffer` will no longer work.
* All `node::Buffer::New()` variants now return `Local<Object>` instead of `Buffer*`.
* The return value for `node::Buffer::New()` is an instantiated JS `Buffer` object.
* `node::Buffer::New(Handle<String>)` now accepts an optional second argument of `enum encoding`.
* API addition of `node::Buffer::Use()` which will use the passed `char*` instead of making a copy.
* API addition of `node::Crypto::Certificate` which handles `node::Crypto::Certificate::verifySpkac(Handle<String>)`, `node::Crypto::Certificate::exportPublicKey(Handle<String>)` & `node::Crypto::Certificte::exportChallenge(Handle<String>)` for working naively with SPKAC (Signed public key & challenges) coming from the HTML5 `keygen` element.

### JS API Changes

* External memory is now allocated using `smalloc`, instead of using `SlowBuffer` as the parent backing.
* `SlowBuffer` has been repurposed to return a `Buffer` instance that has no parent backing.
* `buffer.parent` now points to an object that has been allocated via `smalloc.alloc`, not a `Buffer` instance, and only if the buffer is a slice.
* `buffer.offset` is now a read-only prototype property equal to zero since no instance methods require working on the parent backing.
* API additions `Buffer.alloc()` and `Buffer.dispose()` have been added.
* `Buffer#fill()`  has been extended to fill with the entire passed value.
* `(new Buffer('text\0!', 'ascii')).toString()` outputs `'text !'` in 0.10 and `'text\u0000!'` in 0.12.
* Writable stream `_write()` gets called with 'buffer' encoding when chunk is a Buffer (https://github.com/joyent/node/issues/6119[#6119]).
* Writable stream emits 'finish' on next tick if there was a `write()` (https://github.com/joyent/node/issues/6118[#6118]).

## Process

* `process.maxTickDepth` has been removed, allowing `process.nextTick` to starve I/O indefinitely. This is due to adding `setImmediate` in 0.10.

## Child Process

* `customFds` option to `child_process.spawn` is deprecated. Instead of `customFds` you can use `stdio` like https://github.com/mochajs/mocha/pull/1364/files

## https

* `SNICallback` required returning a `secureContext` synchronously as `function (hostname) {}`. The function signature is now `function (hostname, cb) { cb(null, secureContext); }`. You can feature detect with `'function' === typeof cb`. 


## Signal handling

In node 0.10.x, exit from a fatal signal was accomplished by a signal handler in
node which called `exit(128 + signo)`.  So for `SIGINT` (signal 2), a node process
observing the exit of a spawned node process would observe that process exiting 130,
but no signal would be noted (see [`waitid(2)`](http://pubs.opengroup.org/onlinepubs/9699919799/functions/waitid.html) for more information on how a process
waiting for another determines if the waitee exited due to a signal).  In node
0.12.x, a node process observing the exit of a spawned child node process will
see a null `code`, and a `'SIGINT'` as the signal.

Here is a pair of test programs which illustrates the difference.

    $ cat sleeper.js
    setTimeout(function () {}, 300000)

    $ cat test.js
    cp = require("child_process");
    p = cp.spawn("node", ["sleeper.js"]);
    report = function (code, signal) { console.log("code=" + code + ", signal=" + signal); }
    p.on('exit', report);
    setTimeout(function() { p.kill('SIGINT') }, 2000);


On node 0.10 this produces:

    $ node test.js
    code=130, signal=null

On node 0.12 this produces:

    $ node test.js
    code=null, signal=SIGINT

This can be a subtle porting issue for multi-process node environments which care
about signals (such as test harnesses).  This change was introduced by
[`main: Handle SIGINT properly`](https://github.com/joyent/node/commit/c61b0e9cbc748c5e90fc5e25e4fb490b4104cae3).