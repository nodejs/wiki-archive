When editing this page please be as detailed as possible. Examples are encouraged!

## Changed

* [Streams Interface](https://github.com/joyent/node/blob/master/doc/api/stream.markdown)
  * Readable, Writable, Duplex, and Transform base classes added.
  * Readable streams use a `read()` method instead of emitting `'data'` events right away.
  * Adding a `'data'` event handler, or calling `pause()` or `resume()` will switch into "old mode".
    * This means that `data` event handlers won't ever miss the first chunk if they're not added right away, and `pause()` is no longer merely advisory.
  * If you don't consume the data, then streams will sit in a paused state forever, and the `end` event will never happen.
* The `uv_after_work_cb` signature has changed to take a second integer argument indicating status.  For backwards compatibility, explicitly cast the 4th argument to `uv_queue_work`.  [Example](https://github.com/rbranson/node-ffi/commit/fdeff41ae8b1cca31d4707d7b61253c45181b8fa)
* `process.nextTick` happens at the end of the current tick, immediately after the current stack unwinds.  If you are currently using recursive nextTick calls, use `setImmediate` instead.
* `-p --print` command line switch implies `-e --eval`
* net: sockets created with net.createServer() no longer emit a `connect` event.  The socket is already connected, by the time the `connectionListener` callback is called, so this was considered [superfluous](https://github.com/joyent/node/commit/c11612026fa28f7aedc60c577120f87d86fc15bf#lib/net.js)
* url: Parsed objects always have all properties, but unused ones are set to `null`.  Example:

    ``` js
    // v0.8
    > url.parse('http://foo')
    { protocol: 'http:',
      slashes: true,
      host: 'foo',
      hostname: 'foo',
      href: 'http://foo/',
      pathname: '/',
      path: '/' }
    
    // 0.10
    > url.parse('http://foo')
    { protocol: 'http:',
      slashes: true,
      auth: null,
      host: 'foo',
      port: null,
      hostname: 'foo',
      hash: null,
      search: null,
      query: null,
      pathname: '/',
      path: '/',
      href: 'http://foo/' }
    ```
* Domain-added properties on error objects are `camelCase` instead of `snake_case`
* `path.resolve` and `path.join` throw a TypeError on non-string input
* `dgram.Socket#bind()` is always asynchronous now. If you have code that looks like this:             

    ```javascript                                                                             
    var s = dgram.createSocket('udp4');
    s.bind(1234);
    s.addMembership('224.0.0.114');
    ```
                                                                                        
    You have to change it to this:                                                      
    
    ```javascript                                                                             
    var s = dgram.createSocket('udp4');
    s.bind(1234, function() {
      s.addMembership('224.0.0.114');
    });
    ```
* The `EventEmitter` constructor initializes various properties now.  It still works fine as an OOP inheritance parent, but you have to do inheritance properly.  The Broken-Style JS inheritance pattern will not work when extending the EventEmitter class.  This inheritance style was never supported, but prior to 0.10, it did not actually break.

    ```javascript
    // Broken-Style Inheritance, which has never been safe or wise
    // but is shown on many broken tutorials around the web.
    function Child() {}
    Child.prototype = new Parent(); // <-- NEVER EVER DO THIS!!
    // If you see anyone doing this in any javascript library ever,
    // post a bug telling them that it is a huge terrible mistake!
    // Inheriting from a class should not call that class's ctor
    // on the prototype shared with EVERY instance of the child class!

    // Correct-Style Inheritance
    function Child() {
      Parent.call(this);
    }
    Child.prototype = Object.create(Parent.prototype, {
      constructor: {
        value: Child,
        enumerable: false,
        writable: true,
        configurable: true
      }
    });
    // "Gee that's a lot of lines! I wish there was a helper method!"
    // There is.  Do this:
    util.inherits(Child, Parent);
    ```
* [https](http://nodejs.org/docs/latest/api/https.html) now does peer verification by default. This means that if you try to access an SSL endpoint which has a Certificate Authority that is not in the default CA list, you will get an error where you did not before. You can set `rejectUnauthorized` to `false` to get the old behavior.
* `os.tmpDir()` has been renamed to `os.tmpdir()` as the recommended default. `os.tmpDir()` still exists as an alias.
* Native [addons](http://nodejs.org/docs/latest/api/addons.html) can now receive the complete `module` object, allowing them to override `exports` with a custom object or function (i.e. the ["substack pattern"](https://twitter.com/n8agrin/status/261151288685907968)). Simply adding a second argument to your `Init()` function gives you the `module` object: `void Init(Handle<Object> exports, Handle<Object> module);`. The [addons examples](https://github.com/rvagg/node-addon-examples) now mix both classic and `exports`-overriding patterns (additionally the examples are now available in a [GitHub repo](https://github.com/rvagg/node-addon-examples)).

    **Classic**

    ```c++
    Handle<Value> Booya(const Arguments& args) {
      HandleScope scope;
      return scope.Close(String::New("hello world"));
    }
    void Init(Handle<Object> exports) {
      exports->Set(String::NewSymbol("booya"), FunctionTemplate::New(Booya)->GetFunction());
    }
    NODE_MODULE(addon, Init)
    ```

    ```js
    console.log(require('./build/Release/addon').booya())
    ```

    **Single-function on `exports`**

    ```c++
    Handle<Value> Booya(const Arguments& args) {
      HandleScope scope;
      return scope.Close(String::New("hello world"));
    }
    void Init(Handle<Object> exports, Handle<Object> module) {
      module->Set(String::NewSymbol("exports"), FunctionTemplate::New(Booya)->GetFunction());
    }
    NODE_MODULE(addon, Init)
    ```

    ```js
    console.log(require('./build/Release/addon')())
    ```
* TCP Server socket objects no longer emit the `'connection'` event.  This was originally there to support Socket object re-use, which has not actually been possible since well before Node v0.4.  The Socket is already connected by the time the Server has it, obviously, and the time of the event emission was thus actually a lie.  The spurious event has been removed.  When you get a socket on the server, it's already connected, so you can just go ahead and use it.


## Added

* [Streams](https://github.com/joyent/node/blob/master/doc/api/stream.markdown) - Added base classes for Readable, Writable, Duplex, and Transform
* Streaming interfaces for Crypto API
* process: add getgroups(), setgroups(), initgroups()
* crypto: getHashes() getCiphers()
* http: add response.headersSent property
* events: 'removeListener' event
* setImmediate() and clearImmediate() functions
* string_decoder: the decoder.end() function was added