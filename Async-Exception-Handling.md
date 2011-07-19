This page describes a proposed new node.js core feature for handling exceptions that are thrown by asynchronous callbacks.

See also:

* [Working implementation][implementation] of this proposal

* [Examples][examples] from this document as a separate project.

Please direct feedback to [Ben Williamson](mailto:benw@pobox.com).

## Problem

Code has bugs. Sometimes bugs cause unanticipated exceptions to be thrown. We like to constrain the impact of bugs by catching exceptions. For example, if an exception occurs while handling an http request, we aim for the following:

* The request that threw should return a 500 Internal Server Error to the client

* Other concurrent connections and requests should continue undisturbed

* The process should stay up

Achieving this with node.js is currently challenging, because of the asynchronous programming style. This example illustrates the current state of affairs:

    var http = require('http');

    http.createServer(function (req, res) {
      try {
        // Application code that might throw.
        handle(req, res);
      } catch (e) {
        res.statusCode = 500;
        res.setHeader("Content-Type", "text/plain");
        res.end('500 Internal Server Error\n\n' + e.stack + '\n');
      }
    }).listen(1337, "127.0.0.1");

This works fine if `handle()` throws an exception - it is caught, and an error is returned to the client.

But what if `handle()` performs some async I/O before responding to the request?

    function handle(req, res) {
      // Return the mtime of the requested file.
      fs.stat(req.url, function (err, stats) {
        res.setHeader("Content-Type", "text/plain");
        // Bug: If the file does not exist,
        // stats is undefined and stats.mtime will throw.
        res.end(req.url + ' modified ' + stats.mtime + '\n');
      });
    }

The try/catch mechanism is designed to allow an exception to bubble up the call stack to the point where it can best be handled. But in this async case `handle()` has returned and exited the try/catch block before the exception occurs. The bug is in the callback that was passed to `fs.stat()`, which is called from the event loop. The exception is uncaught, with these results:

* The process prints the exception to stderr and exits

* The client sees a closed connection with an empty response

* All other client connections are dropped

Not the result we were hoping for.

Node does provide the `uncaughtException` event, but it doesn't provide sufficiently specific context to achieve our goals. It is a single global "catch everything" handler that can prevent the process from exiting, but it doesn't help us to perform the appropriate cleanup such as returning an error response to the affected client, closing a database connection etc.

We want to catch the exception at the point where it can best be dealt with - and that means putting some kind of exception handler right where we currently have our try/catch block. In this context we have the relevant client connection: `req` and `res`.

## Proposed Solution

In this proposal:

1. Uncaught exceptions are passed to a global handler called `process.exceptionCatcher`

2. Anyone can assign a function to `process.exceptionCatcher` at any time

3. Every API function that accepts an async callback saves the current `process.exceptionCatcher` value and restores it right before the callback is invoked.

This example shows it in use:

    var http = require('http');

    http.createServer(function (req, res) {
      // Install an exception catcher for this request.
      process.exceptionCatcher = function (e) {
        res.statusCode = 500;
        res.setHeader("Content-Type", "text/plain");
        res.end('500 Internal Server Error\n\n' + e.stack + '\n');
      };
      // Application code that might throw.
      handle(req, res);
      // Subsequent exceptions should not
      // interfere with this request.
      delete process.exceptionCatcher;
    }).listen(1337, "127.0.0.1");

This works. According to point 3 above, the `fs.stat` API function saves our exception handler function and restores it right before calling the buggy callback. So `process.exceptionCatcher` is set to our handler when `stats.mtime` throws. The exception is caught, an error response is returned to the correct client, and all is well.

## Implementation

A [candidate implementation][implementation] is under development. Let's take a look:

    fs.stat = function(path, callback) {
      callback = wrapCallback(callback);
      binding.stat(path, wrapCallback(callback || noop));
    };

    function wrapCallback(callback) {
      var catcher = process.exceptionCatcher;
      if (!catcher || 'function' !== typeof callback) {
        return callback;
      }
      return function () {
        process.exceptionCatcher = catcher;
        callback.apply(this, arguments);
        delete process.exceptionCatcher;
      };
    }

In this implementation, every API function (`fs.stat()` in this example) calls `wrapCallback()` to wrap its callback in a closure that stores the current `process.exceptionCatcher` and restores it when the callback is called.

This does incur the overhead of an extra closure on every callback. You only pay for it if you use it - `wrapCallback()` is a noop if there is no `process.exceptionCatcher` installed.

The bulk of the implementation is to apply this pattern to every async API method. The `wrapCallback` function is exposed as `require('events').wrapCallback`.

The C++ change in `src/node.cc` to pass uncaught exceptions to `process.exceptionCatcher` is quite small, and is compatible with the existing `uncaughtException` event. If `process.exceptionCatcher` throws an exception then it is handled by emitting an `uncaughtException` event or exiting the process.

## Nesting Handlers

A feature of try/catch blocks is that they can be nested. We can achieve the same thing with `process.exceptionCatcher` by chaining. If a handler cannot handle the exception, it can explicitly call the previously installed `process.exceptionCatcher` if any. This is analogous to a catch block that rethrows the caught exception. 

    var oldCatcher = process.exceptionCatcher;
    process.exceptionCatcher = function (e) {
      if (e instanceof ExceptionWeCanHandle) {
        // take care of it
      } else if ('function' === typeof oldCatcher) {
        oldCatcher(e);
      } 
    };

## Incompatibilities

This proposal does introduce some minor incompatibilities.

Modules outside the node.js core should ensure that callbacks are invoked with the `process.exceptionCatcher` that was in effect when the callback was passed in. For code that passes these callbacks through to core API methods, no change will be needed. Code that stores and dispatches callbacks directly will need to apply the `wrapCallback()` pattern or provide equivalent behaviour. Modules that call callbacks without setting `process.exceptionCatcher` will be no worse off than they were prior to this proposal, but users should expect that asynchronous exception handling will work consistently everywhere.

The `EventEmitter` interface contains a `listeners()` method which exposes a mutable array of listener callbacks:

> #### emitter.listeners(event)
> 
> Returns an array of listeners for the specified event. This array can be
> manipulated, e.g. to remove listeners.

This proposal introduces a necessary change to that API. Because an exception handler function must be stored with each listener callback, either:

1. `listeners()` will return an array of objects (perhaps closures) which wrap exception handlers along with the listener callbacks that were passed in, in which case comparisons with the original callbacks will fail; or

2. `listeners()` will return a temporary array of the original listener callbacks, but changes to the array will have no effect.

The candidate implementation chooses option 2, which seems the least incompatible. While the present API documentation states that the array can be manipulated, there is no unit test for that feature and it seems unlikely to be used widely.

It would be straight forward to provide both options, with one going by a new method name.

[implementation]: https://github.com/benw/node
[examples]: https://github.com/benw/async-catch-examples