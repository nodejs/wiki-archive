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

## Added

* [Streams](https://github.com/joyent/node/blob/master/doc/api/stream.markdown) - Added base classes for Readable, Writable, Duplex, and Transform
* Streaming interfaces for Crypto API
* process: add getgroups(), setgroups(), initgroups()
* crypto: getHashes() getCiphers()
* http: add response.headersSent property
* events: 'removeListener' event
* setImmediate() and clearImmediate() functions
* string_decoder: the decoder.end() function was added