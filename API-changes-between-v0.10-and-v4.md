### When editing this page please be as detailed as possible. Examples are encouraged!

## By Subsystem

### assert

[[Docs](https://iojs.org/api/assert.html)]

- [`assert.deepEqual()`](https://iojs.org/api/assert.html#assert_assert_deepequal_actual_expected_message) no longer checks for `prototype` equality.
  - [`assert.deepStrictEqual()`](https://iojs.org/api/assert.html#assert_assert_deepstrictequal_actual_expected_message) was introduced to deal with some former expectations about `deepEqual()` which were not true.
  - Refs: [`e7573f9`](https://github.com/nodejs/node/commit/e7573f9111f6b85c599ec225714d76e08ec8a4dc), [`3f473ef`](https://github.com/nodejs/node/commit/3f473ef141fdc7059928ebc4542b00e2f126ab07)

### buffer

[[Docs](https://iojs.org/api/buffer.html)]

- External memory is now allocated using [TypedArrays](https://developer.mozilla.org/en/docs/Web/JavaScript/Typed_arrays), instead of using `SlowBuffer` or `smalloc` as the parent backing.
  - Refs: [`63da0df`](https://github.com/nodejs/node/commit/63da0dfd3a4460e117240e84b57af2137469497e)

---

- [`Buffer.concat()`](https://iojs.org/api/buffer.html#buffer_class_method_buffer_concat_list_totallength) now always creates a new buffer, even if only called with one element. Previously when called with one element it would just return the element, even if not a Buffer.
  - Refs: [`f4f16bf`](https://github.com/nodejs/io.js/commit/f4f16bf03980df4d4366697d40e80da805a38bf8), [#1891](https://github.com/nodejs/io.js/issues/1891), [#1937](https://github.com/nodejs/io.js/pull/1937)
- [`SlowBuffer`](https://iojs.org/api/buffer.html#buffer_class_slowbuffer) has been repurposed to return a `Buffer` instance who's parent backing is not pooled.
  - Refs: [`456942a`](https://github.com/nodejs/node/commit/456942a920fe313ebe0b0da366d26ef400ec177e)
- [`Buffer.prototype.toJSON()`](https://iojs.org/api/buffer.html#buffer_buf_tojson)'s output is no longer the same as an array. Instead it is an object specifically tagged as a buffer, which can be recovered by passing it to (a new overload of) the `Buffer` constructor.
  - Refs: [`840a29f`](https://github.com/nodejs/node/commit/840a29fc0fd256a63b3f2f5e7528de5107a608a3)
- `Buffer.prototype.parent` is now a getter that points to `buffer.buffer` if the buffer's size is greater than zero.
  - Refs: [`63da0df`](https://github.com/nodejs/node/commit/63da0dfd3a4460e117240e84b57af2137469497e)
- `Buffer.prototype.offset` is now a read-only getter that returns `buffer.byteOffset`.
  - Refs: [`63da0df`](https://github.com/nodejs/node/commit/63da0dfd3a4460e117240e84b57af2137469497e)
- [`Buffer.prototype.fill()`](https://iojs.org/api/buffer.html#buffer_buf_fill_value_offset_end) now accepts multi-character strings and will fill with the entire passed value. Additionally, `fill()` now returns its `buffer` and can be chained.
  - Refs: [`57ed3da`](https://github.com/nodejs/node/commit/57ed3daebfe4700b14cd551f50240f1a634dbd64)
- `Buffer.prototype._charsWritten` no longer is written to, nor exists.
  - Refs: [`ccda6bb`](https://github.com/nodejs/node/commit/ccda6bb3ac99ee46508860385128f47a3d5547e5)

#### Buffer changes from V8

Implementation changes in V8 have caused subtle impacts how buffers work with encodings in certain cases.

- `(new Buffer('text\0!', 'ascii')).toString()` outputs `'text !'` in 0.10 and `'text\u0000!'` in 0.12.
- Invalid unicode characters are replaced to no longer become invalid.
 - Refs: [#2344](https://github.com/nodejs/node/issues/2344)

### child_process

[[Docs](https://iojs.org/api/child_process.html)]

- [`ChildProcess.prototype.send()`](https://iojs.org/api/child_process.html#child_process_child_send_message_sendhandle) is now always asynchronous, where previously it was blocking on Unix.
  - Pull request [#2620](https://github.com/nodejs/node/pull/2620), which should land before 4.0.0, will make `send()` accept a callback as the last argument, which will be called with one argument: `null` if it succeeded, or an `Error` if it failed.
  - Refs: [#760](https://github.com/nodejs/node/issues/760), [#2620](https://github.com/nodejs/node/pull/2620)
- The `customFds` option for [`ChildProcess.prototype.spawn()`](https://iojs.org/api/child_process.html#child_process_child_process_spawn_command_args_options) is deprecated. Instead of `customFds` you can use the [`stdio`](https://iojs.org/api/child_process.html#child_process_options_stdio) option like [this example](https://github.com/mochajs/mocha/pull/1364/files).
  - Refs: [`2454695`](https://github.com/nodejs/node/commit/245469587c7a6326d1b55bdf6c6d6650d72bfa22)
- The [`'disconnect'`](https://iojs.org/api/child_process.html#child_process_event_disconnect) event on a `ChildProcess` is now fired asynchronously.
  - Refs: [`b255f4c`](https://github.com/nodejs/node/commit/b255f4c10a80343f9ce1cee56d0288361429e214)

### cluster

[[Docs](https://iojs.org/api/cluster.html)]

- Cluster now uses round-robin load balancing by default on non-Windows platforms. The scheduling policy is configurable by setting [`cluster.schedulingPolicy`](https://iojs.org/api/cluster.html#cluster_cluster_schedulingpolicy).
    - It is advised that you do not use round-robin balancing on Windows as it is very inefficient.
  - Refs: [`e72cd41`](https://github.com/nodejs/node/commit/e72cd415adf2ca12daddc001cc3fe953cdb4b507)
- Cluster has been made `--debug`-aware.
  - Refs: [`43ec1b1`](https://github.com/nodejs/node/commit/43ec1b1c2e77d21c7571acd39860b9783aaf5175), [`3821863`](https://github.com/nodejs/node/commit/3821863109be9e56f41f1ea6da0cb6e822037fc3), [`11c1bae`](https://github.com/nodejs/node/commit/11c1bae734dae3a017f2c4f3f71b5e679a9ddfa6), [`423d894`](https://github.com/nodejs/node/commit/423d8944ce58bc8c3f90d2827339a4dea10ab96e)
- Cluster's [`'setup'`](https://iojs.org/api/cluster.html#cluster_event_setup) event now fires asynchronously.
  - Refs: [`876d3bd`](https://github.com/nodejs/node/commit/876d3bd85aabe7fce71a025a89c6b3f6ddbf2053)
- Open connections (and other handles) in the master process are now closed when the last worker that holds a reference to them quits. Previously, they were only closed at cluster shutdown.
  - A side-effect of this is that the cluster master will refuse connections if there are no workers.
  - Refs: [`41b75ca`](https://github.com/nodejs/node/commit/41b75ca9263f368db790fbdcc3963bb1a8c5cb7e), [#2606](https://github.com/nodejs/node/pull/2606),  [#1239](https://github.com/nodejs/node/issues/1239)

### crypto

[[Docs](https://iojs.org/api/crypto.html)]

- [`crypto.randomBytes()`](https://iojs.org/api/crypto.html#crypto_crypto_randombytes_size_callback) now blocks if there is insufficient entropy.
  - Although it blocks, it should normally never take longer than a few milliseconds.
  - `crypto.pseudoRandomBytes()` is now an alias to `crypto.randomBytes()`.
  - Refs: [`e5e5980`](https://github.com/nodejs/node/commit/e5e598060eb43faf2142184d523a04f0ca2d95c3)
- [`crypto.Sign.prototype.sign()`](https://iojs.org/api/crypto.html#crypto_sign_sign_private_key_output_format) will no longer swallow internal OpenSSL errors.
  - Refs: [`00bffa6`](https://github.com/nodejs/node/commit/00bffa6c758038dca039fd9114912c0430114c08)

### dgram

[[Docs](https://iojs.org/api/dgram.html)]

- [`dgram.Socket.prototype.send()`](https://iojs.org/api/dgram.html#dgram_socket_send_buf_offset_length_port_address_callback) error semantics have changed.
  - Through 2.x, the the dgram `socket.send()` method emitted errors when a DNS lookup failed, even when a callback was provided. Starting from 3.0.0, if a callback is provided, no error event will be emitted.
  - When `send()` is supplied with a callback, it will no longer emit the `'error'` event when an error occurs and instead pass the error to the callback.
  - Previously the behavior was to do _both_.
  - Refs: [#1796](https://github.com/nodejs/io.js/pull/1796)

### domain

[[Docs](https://iojs.org/api/domain.html)]

The domain module [has been scheduled for deprecation](https://iojs.org/api/domain.html#domain_domain), awaiting an alternative for those who absolutely **need** domains.

It is suggested you avoid using domains if possible and rather rely on regular error checking.

- [`domain.dispose()`](https://iojs.org/api/domain.html#domain_domain_dispose) has been deprecated.
  - Please recover from failed IO actions explicitly via error event handlers set on the domain.
  - Refs: [`d86814a`](https://github.com/nodejs/node/commit/d86814aeca64d8985402dc073eff1fc8ac93c231)

### events

[[Docs](https://iojs.org/api/events.html)]

- [`EventEmitter.listenerCount()`](https://iojs.org/api/events.html#events_class_method_eventemitter_listenercount_emitter_event) is now deprecated in favor of `EventEmitter.prototype.listenerCount()`.
  - Refs: [`8f58fb9`](https://github.com/nodejs/node/commit/8f58fb92fff904a6ca58fd0df9ee5a1816e5b84e)

### freelist

- The `freelist` module was only ever intended for internal use and is now deprecated.
  - Refs: [`fef87fe`](https://github.com/nodejs/node/commit/fef87fee1de60c3d5db2652ee2004a6e78d112ff), [#569](https://github.com/nodejs/node/issues/569)

### fs

[[Docs](https://iojs.org/api/fs.html)]

- [`fs.createReadStream()`](https://iojs.org/api/fs.html#fs_fs_createreadstream_path_options) and [`fs.createWriteStream()`](https://iojs.org/api/fs.html#fs_fs_createwritestream_path_options) now have stricter type-checking for the `options` argument.
  - Refs: [`353e26e`](https://github.com/nodejs/node/commit/353e26e3c7)
- [`fs.exists()`](https://iojs.org/api/fs.html#fs_fs_exists_path_callback) is now deprecated. It is suggested to use either [`fs.access()`](https://iojs.org/api/fs.html#fs_fs_access_path_mode_callback) or [`fs.stat()`](https://iojs.org/api/fs.html#fs_fs_stat_path_callback). Please read the documentation carefully.
- Added more informative errors and method call site details when the `NODE_DEBUG` environment is set to ease debugging.
  - The same thing applies to the synchronous versions of these APIs.
  - Refs: [`1f76a2e`](https://github.com/nodejs/node/commit/1f76a2eddc3561ff2c434d491e0d2b893c374cfd)
- Fixed missing callbacks errors just being printed instead of thrown.
  - Refs: Probably [`1f40b2a`](https://github.com/nodejs/node/commit/1f40b2a63616efe0e4c0744a1f630161526e4236)
- On Unix soft `ulimit` values are ignored.
  - Refs: [`6820054`](https://github.com/nodejs/node/commit/6820054d2d42ff9274ea0755bea59cfc4f26f353), [nodejs/node-v0.x-archive#8704](https://github.com/nodejs/node-v0.x-archive/issues/8704)
- Errors for async `fs` operations are now thrown if a callback is not present.
  - Refs: [`a804347`](https://github.com/nodejs/node/commit/a80434736bce22a9ac00376bb5786806752ef3dd), [`6bd8b7e`](https://github.com/nodejs/node/commit/6bd8b7e5405e1cdc9f56214f5f6b741806c32e5f), [`5e140b3`](https://github.com/nodejs/node/commit/5e140b33e58bd0ac6779fb0cb635dd51e1a27289)
- The error messages of most `fs` errors have been improved to be more descriptive and useful.
  - Refs: [`bc2c85c`](https://github.com/nodejs/node/commit/bc2c85ceef7ac034830e4a4357d0aef69cd6e386)

### http

[[Docs](https://iojs.org/api/http.html)]

-  http server timing change (Use `'finish'` instead of `'prefinish'` when ending a response)
  - When doing HTTP pipelining of requests, the server creates new request and response objects for each incoming HTTP request on the socket. Starting from 3.0.0, these objects are now created a couple of ticks later, when we are certainly done processing the previous request. This could change the observable timing with certain HTTP server coding patterns.
  - The switch to `'finish'` is intended to prevent the socket being detached from the response until after the data is actually sent to the other side.
  - Refs: [`6020d2`](https://github.com/nodejs/io.js/commit/6020d2a2fb75de6766c807864fa8f1c0fba88ec9), [#1411](https://github.com/nodejs/io.js/pull/1411)
- Removed default chunked encoding on `DELETE` and `OPTIONS` requests.
  - Refs: [`aef0960`](https://github.com/nodejs/node/commit/aef09601b4320648f4b6fac0baba3e5c54b0406a), [`1df32af`](https://github.com/nodejs/node/commit/1df32af74a450d456e264afbbaf6201a388d7572)
- [`http.STATUS_CODES`](https://iojs.org/api/http.html#http_http_status_codes) now maps to the regulated [IANA standard](http://www.iana.org/assignments/http-status-codes/http-status-codes.xhtml) set of codes.
  - Updating the code mappings to conform to the IANA standard was a backwards incompatible change to consumers who depend on the text value of a header. The most significant of these changes was the text for `302`, which was previously `Moved Temporarily` but is now `Found`.
  - Refs: [`235036e`](https://github.com/nodejs/io.js/commit/235036e7faa537469c3600850bdd533c095c392a), [#1470](https://github.com/nodejs/io.js/pull/1470)
- [`http.Agent.prototype.getName()`](https://iojs.org/api/http.html#http_agent_getname_options) no longer returns an extra trailing colon.
  - Refs: [`06cc919`](https://github.com/nodejs/io.js/commit/06cc919a0cd41380a83e4ea699f1a9ea30881266), [#1617](https://github.com/nodejs/io.js/pull/1617)
- `http.OutgoingMessage.prototype.flush()` has been deprecated in favor of the new [`flushHeaders()`](https://iojs.org/api/http.html#http_request_flushheaders) method. The behavior remains the same.
  - Note: these methods reside on **both** the common http `request` and `response` objects.
  - Refs: [`b2e00e3`](https://github.com/nodejs/node/commit/b2e00e38dcac23cf564636e8fad880829489286c), [`nodejs/node@89f3c90`](https://github.com/nodejs/node/commit/89f3c9037faf19eb32c464b2e02a0a9191156c36)
- [`http.OutgoingMessage.prototype.write()`](https://iojs.org/api/http.html#http_response_write_chunk_encoding_callback) will now emit an error on the `OutgoingMessage` (`response`) object if it has been called after [`end()`](https://iojs.org/api/http.html#http_response_end_data_encoding_callback).
  - Refs: [`64d6de9`](https://github.com/nodejs/node/commit/64d6de9f34abe63bf7602ab0b55ff268cf480e45)
- [`http.request()`](https://iojs.org/api/http.html#http_http_request_options_callback) and [`http.get()`](https://iojs.org/api/http.html#http_http_get_options_callback) wrongly decode multi-byte string characters as `'binary'`.
  - Notes: This bug still exists in 4.0.0.
  - Refs: [#2114](https://github.com/nodejs/node/issues/2114), [nodejs/node-v0.x-archive#25634 (comment)](https://github.com/nodejs/node-v0.x-archive/issues/25634#issuecomment-118970793)
- `http.request()` and `http.get()` now throw a TypeError if the requested path contains illegal characters. (Currently only `' '`).
  - Refs: [`7124387`](https://github.com/nodejs/node/commit/7124387b3414c41533078f14a84446e2e0a6ff95)
- `http.request()` and `http.get()` no longer accept non-ascii characters in the header fields or path.
  - This conforms to the spec and is a security fix.
  - Refs: [`ce3d184`](https://github.com/nodejs/node/commit/ce3d18412c9cbd8259f1dac84f83a039436adf91), [#2629](https://github.com/nodejs/node/pull/2629), [#1693](https://github.com/nodejs/node/issues/1693)

### net

[[Docs](https://iojs.org/api/net.html)]

- [`net.Server.prototype.address()`]() now also returns a `{ address: '/path/to/socket' }` object, like it does for TCP and UDP sockets.
  - Previously it returned `socket._pipeName` directly for unix sockets.
  - Refs: [`f337595`](https://github.com/nodejs/node/commit/f337595441641ad36f6ab8ae770e56c1673ef692)
- [`net.Server.prototype.listen()`](https://iojs.org/api/net.html#net_server_listen_port_hostname_backlog_callback) will now try to bind to `::` (IPv6) if the bind address is omitted, and use `0.0.0.0` (IPv4) as a fallback.
  - Refs: [`2272052`](https://github.com/nodejs/node/commit/2272052461445dfcae729cc6420a3d3229362158)

### os

[[Docs](https://iojs.org/api/os.html)]

- [`os.tmpdir()`](https://iojs.org/api/os.html#os_os_tmpdir) now never returns a trailing slash regardless of the host platform.
  - Refs: [`bb97b70`](https://github.com/iojs/io.js/commit/bb97b70eb709b0e0470a5164b3722c292859618a), [#747](https://github.com/iojs/io.js/pull/747)
- `os.tmpdir()` on Windows now uses the `%SystemRoot%` or `%WINDIR%` environment variables instead of the hard-coded value of `c:\windows` when determining the temporary directory location.
  - Refs:  [`120e5a2`](https://github.com/nodejs/node/commit/120e5a24df76deb5019abec9744ace94f0f3746a)

### path

[[Docs](https://iojs.org/api/path.html)]

- [`path.isAbsolute()`](https://iojs.org/api/path.html#path_path_isabsolute_path) will now always return a boolean, even on windows.
  - Refs: [`20229d6`](https://github.com/nodejs/node/commit/20229d6896ce4b802a0789b1d2643dcac55bebb9)
- Many `path` functions now assert that any paths provided are strings.
  - Refs: [`eb995d6`](https://github.com/nodejs/node/commit/eb995d682201018b2a47c44e921848cfa31486a2), [`8de78e4`](https://github.com/nodejs/node/commit/8de78e470d2e291454e2184d7f206c70d4cb8c97)

### process

[[Docs](https://iojs.org/api/process.html)]

- Added the concept of `beforeExit` time.
  - Before the process emits [`'exit'`](https://iojs.org/api/process.html#process_event_exit) and begins shutting down, it will emit a [`'beforeExit'`](https://iojs.org/api/process.html#process_event_beforeexit) event. Code that is run in the `'beforeExit'` event can schedule async operations that will hold the event loop open, unlike `'exit'` where it is too late to async operations.
  - Refs: [`a2eeb43`](https://github.com/nodejs/node/commit/a2eeb43deda58e7bbb8fcf24b934157992b937c0)
- Chunked writes to sdtout/stderr will now be lost if the process is terminated early.
  - Chunked writes happen when the string to be written is beyond a certain, fairly large, size.
  - Refs: [#784](https://github.com/nodejs/node/issues/784), [#1771](https://github.com/nodejs/node/pull/1771)
- [`process.kill()`](https://iojs.org/api/process.html#process_process_kill_pid_signal) now throws errors on non-numeric input.
  - Strings that can be coerced to numbers still work. e.g. `process.kill('0')`.
  - Refs: [`832ec1c`](https://github.com/nodejs/node/commit/832ec1cd507ed344badd2ed97d3da92975650a95), [`743a009`](https://github.com/nodejs/node/commit/743a009bad64c4302a724f70c42d73601a16aed4)
- `process.maxTickDepth` has been removed, allowing [`process.nextTick()`](https://iojs.org/api/process.html#process_process_nexttick_callback_arg) to starve I/O indefinitely.
  - This is due to adding [`setImmediate()`](https://iojs.org/api/timers.html#timers_setimmediate_callback_arg) in 0.10.
  - It is suggested you use `setImmediate()` over `process.nextTick()`. `setImmediate()` likely does what you are hoping for (a more efficient `setTimeout(..., 0)`), and runs after this tick's I/O. `process.nextTick()` does not actually run in the "next" tick anymore and will block I/O as if it were a synchronous operation.
  - Refs: [`0761c90`](https://github.com/nodejs/node/commit/0761c90204d7a0134c657e20f91bd83bfa6e677a), [`9a6c085`](https://github.com/nodejs/node/commit/9a6c0853bc164ad2d76f51cdcb0771e881cd0a5f)
- [`process.send()`](https://github.com/nodejs/node/pull/1978) is now always asynchronous, where previously it was blocking on Unix.
    - Pull request [#2620](https://github.com/nodejs/node/pull/2620), which should land before 4.0.0, will make `send()` accept a callback as the last argument, with will be called with one argument: `null` if it succeeded, or an `Error` if it failed.
    - Refs: [#760](https://github.com/nodejs/node/issues/760), [#2620](https://github.com/nodejs/node/pull/2620)

#### Signal handling

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
    var cp = require("child_process")
    var p = cp.spawn("node", ["sleeper.js"])
    p.on('exit', function (code, signal) {
      console.log("code=" + code + ", signal=" + signal)
    })
    setTimeout(function () { p.kill('SIGINT') }, 2000)


On node 0.10 this produces:

    $ node test.js
    code=130, signal=null

On node 0.12+ this produces:

    $ node test.js
    code=null, signal=SIGINT

This can be a subtle porting issue for multi-process node environments which care
about signals (such as test harnesses).  This change was introduced by
[`c61b0e9`—`main: Handle SIGINT properly`](https://github.com/nodejs/node/commit/c61b0e9cbc748c5e90fc5e25e4fb490b4104cae3).

### querystring

[[Docs](https://iojs.org/api/querystring.html)]

- querystring's `stringifyPrimitive()` now stringifies numbers correctly.
  - Refs: [`c9aec2b`](https://github.com/nodejs/node/commit/c9aec2b7167a08dc88141fbe3be1c498f8c5b061)
- [`querystring.stringify()`](https://iojs.org/api/querystring.html#querystring_querystring_stringify_obj_sep_eq_options) no longer accepts a 4th, undocumented, `name` parameter.
  - Refs: [`8d3bc88`](https://github.com/nodejs/node/commit/8d3bc88bbe3c0690f433a97c1df33fca099f18be)

### smalloc

`smalloc` was a core module in 0.12 and io.js <3.0.0 for doing (external) raw memory allocation/deallocation/copying in JavaScript.

`smalloc` was removed in io.js 3.0.0, as the required v8 APIs were removed. `smalloc` used to be used in the buffer implementation, however buffers are now built on top of TypedArrays (Specifically Uint8Array).

### stream

[[Docs](https://iojs.org/api/stream.html)]

The changes to streams are not as drastic as the transition from streams1 to streams2: they are a
refinement of existing ideas, and should make the API slightly less surprising for humans and faster
for computers. As a whole the changes are referred to as "streams3", but the changes should largely go
unnoticed by the majority of stream consumers and implementers.

#### Readable Streams

The distinction between "flowing" and "non-flowing" modes has been refined. Entering "flowing" mode is
no longer an irreversible operation -- it is possible to return to "non-flowing" mode from "flowing" mode.
Additionally, the two modes now flow through the same machinery instead of replacing methods. Any time
data is returned as a result of a [`.read()`](https://iojs.org/api/stream.html#stream_readable_read_size) call that data will *also* be emitted on the [`'data'`](https://iojs.org/api/stream.html#stream_event_data) event.

As before, adding a listener for the [`'readable'`](https://iojs.org/api/stream.html#stream_event_readable) or `'data'` event will start flowing the stream; as
will piping to another stream.

#### Writable Streams

The ability to "bulk write" to underlying resources has been added to `Writable` streams. For stream
implementers, one can signal that a stream is bulk-writable by specifying a [_writev](https://iojs.org/api/stream.html#stream_writable_writev_chunks_callback) method.
Bulk writes will occur in two situations:

1. When a bulk-writable stream is clearing its backlog of buffered write requests,
2. or if an end user has made use of the new [`.cork()`](https://iojs.org/api/stream.html#stream_writable_cork) and [`.uncork()`](https://iojs.org/api/stream.html#stream_writable_uncork) API methods.

`.cork` and `.uncork` allow the end user to control the buffering behavior of writable streams separate
from exerting backpressure. `.cork()` indicates that the stream should accept new writes (up to `.highWaterMark`),
while `.uncork()` resets that behavior and attempts to bulk-write all buffered writes to the underlying resource.

The only core stream API that **currently** implements `_writev()` is [`net.Socket`](https://iojs.org/api/net.html#net_class_net_socket).

In addition to the bulk-write changes, the performance of repeated small writes to non-bulk-writable streams
(such as `fs.WriteStream`) has been drastically improved. Users piping high volume log streams to disk should
see an improvement.

For a detailed overview of how streams3 interact, [see this diagram](https://cloud.githubusercontent.com/assets/37303/5728694/f9a3e300-9b20-11e4-9e14-a6938b3327f0.png).

- [`WritableState.prototype.buffer`](https://iojs.org/api/stream.html#stream_buffering) has been deprecated in favor of `_writableState.getBuffer()`, which builds an array from an internal object single-way linked list.
  - Mutating the array returned will have no effect as `getBuffer()` constructs it from the linked list. However modifying one of the array's element's `.next` property will effect the list.
  - Refs: [`9158666`](https://github.com/nodejs/node/commit/91586661c983f45d650644451df73c8649a8d459)
- `WritableStream.prototype._write()` now gets called with `'buffer'` encoding when chunk is a `Buffer`.
  - Refs: [nodejs/node-v0.x-archive#6119](https://github.com/nodejs/node-v0.x-archive/issues/6119)
- Writable streams now emit `'finish'` on the next tick if there was a `write()` when finishing.  
  - Refs: [nodejs/node-v0.x-archive#6118](https://github.com/nodejs/node-v0.x-archive/issues/6118)

#### Transform Streams

- When in `objectMode`, `Transform.prototype._read()` now processes false-y (but not `null`) values, such as `''`, `0`, etc.
  - Refs: [`26ca7d7`](https://github.com/nodejs/node/commit/26ca7d73ca9c45112f33579aa5a1293059010779)

### sys

As of 1.0.0 the `sys` module is deprecated. It is advised to use the [`util`](https://iojs.org/api/util.html) module instead.

### timers

[[Docs](https://iojs.org/api/timers.html)]

- Updated [`setImmediate()`](https://iojs.org/api/timers.html#timers_setimmediate_callback_arg) to process the full queue each turn of the event loop, instead of one per queue.
  - It is suggested you use `setImmediate()` over `process.nextTick()`. `setImmediate()` likely does what you are hoping for (a more efficient `setTimeout(..., 0)`), and runs after this tick's I/O. `process.nextTick()` does not actually run in the "next" tick anymore and will block I/O as if it were a synchronous operation.
  - Refs: [`fa46483`](https://github.com/nodejs/node/commit/fa46483fe203f56dccd6e122573857cc2c322220)
- `setImmediate()`'s timing was adjusted slightly, but is still after `process.nextTick()` and I/O, but before regular timeouts and intervals.
  - Refs: [`cd37251`](https://github.com/nodejs/node/commit/cd372510bb504b6d3414b01cc8c9ee457b2e16c4)
- Internal timeouts now run in a separate queue with slightly different semantics, and will never keep the process alive on their own.
  - This may effect your performance profile.
  - It is strongly advised you do not attempt to use `_unrefActive()` as it will probably be hidden in the future.
  - Refs: [`f46ad01`](https://github.com/nodejs/node/commit/f46ad012bc5a40194242ea1e9669c9cc25bd7047)
- Timer globals (e.g. `setTimeout()`) are no longer lazy-loaded.
  - Refs: [`2903410`](https://github.com/nodejs/node/commit/2903410aa8)

### tls

[[Doc](https://iojs.org/api/tls.html)]

- The [tls server option `SNICallback`](https://iojs.org/api/tls.html#tls_tls_createserver_options_secureconnectionlistener) required returning a `secureContext` synchronously as `function (hostname) { return secureContext; }`. The function signature is now asynchronous as `function (hostname, cb) { cb(null, secureContext); }`. You can feature detect with `'function' === typeof cb`.
  - Refs: [`048e0e7`](https://github.com/nodejs/node/commit/048e0e77e0c341407ecea364cbe26c8f77be48b8)
- The [tls server option `dhparam`](https://iojs.org/api/tls.html#tls_tls_createserver_options_secureconnectionlistener) must now have a key length greater than 1024 bits, or else it will throw an error.
  - This is effectively a security fix. Please see https://weakdh.org/ for more information.
  - Refs: [`9b35be5`](https://github.com/nodejs/node/commit/9b35be5810)
- RC4 is now disabled on the cipher list.
  - RC4 is now considered insecure and has been removed from the list of default ciphers for TLS servers. Use the `ciphers` option when starting a new TLS server to supply an alternative list.
  - Refs: [#826](https://github.com/nodejs/io.js/pull/826)
  - Potentially mitigated if [archive#39](https://github.com/nodejs/node-convergence-archive/issues/39) is merged.
- Implemented TLS streams in C++, boosting their performance.
  - Refs: [`1738c77`](https://github.com/nodejs/node/commit/1738c7783526868d86cb213414cb4d40c5a89662)
- Moved & renamed `crypto.createCredentials()` to [`tls.createSecureContext()`](https://iojs.org/api/tls.html#tls_tls_createsecurecontext_details).
  - Refs: [`5d2aef1`](https://github.com/nodejs/node/commit/5d2aef17ee56fbbf415ca1e3034cdb02cd97117c)
- Removed SSLv2 and SSLv3 support.
  - Both SSLv2 and SSLv3 are now considered too insecure for general use and have been disabled at compile-time.
  - Refs: [#290](https://github.com/nodejs/node/pull/290), [#315](https://github.com/nodejs/node/pull/315), [archive#20](https://github.com/nodejs/node-convergence-archive/issues/20)

### url

[[Docs](https://iojs.org/api/url.html)]

- [`url.parse()`](https://iojs.org/api/url.html#url_url_parse_urlstr_parsequerystring_slashesdenotehost) will now always return a query object and search string, even if they are empty, when `parseQueryString` (the second argument) is set to `true`.
  - e.g. a `url.parse(..., true)` return value will now include `{ search: '', query: {} }` where previously both properties would not exist.
  - Refs: [`b705b73`](https://github.com/nodejs/node/commit/b705b73e46193c7691be40b732330a49affacedb)
- `url.parse()` now escapes the following characters and spaces (`' '`) in the parsed object's properties:
  ```
  < > " ` \r \n \t { } | \ ^ '
  ```
  - e.g.
  ```
  url.parse("http://example.com/{z}/{x}/{y}.png#foo{}").href === 'http://example.com/%7Bz%7D/%7Bx%7D/%7By%7D.png#foo%7B%7D'
  ```
  - Refs: [#2605](https://github.com/nodejs/node/pull/2605), [#2113](https://github.com/nodejs/node/issues/2113)
- [`url.resolve()`](https://iojs.org/api/url.html#url_url_resolve_from_to) now resolves properly to `'.'` and `'..'`.
  - This means that `resolve('/a/b/', '.')` will return `'/a/b/'`, and `resolve('/a/b', '.')` will return `'/a/'`.
  - `resolve('/a/b/', '..')` returns `'/a/'`.
  - This change, while technically a potentially breaking change, also landed in node 0.10.37.
  - Refs: [`faa687b`](https://github.com/nodejs/node/commit/faa687b4be2cea71c545cc1bec631c164b608acd), [#278](https://github.com/nodejs/io.js/pull/278)

### util

[[Docs](https://iojs.org/api/util.html)]

- `util.is*()` (`isArray()` ... `isUndefined()`) type-checking functions were added in 0.12 but are scheduled for deprecation. Please use user-land solutions instead.
  - The type checking these use will be come brittle with the eventual addition of `Symbol.toStringTag`.
  - Refs: [#2447](https://github.com/nodejs/node/pull/2447)
- Updated [`util.format()`](https://iojs.org/api/util.html#util_util_format_format) to receive several changes:
  - `-0` is now displayed as such, instead of as `0`.
    - Refs: [`b3e4fc6`](https://github.com/nodejs/node/commit/b3e4fc6a48b97b52bd19de43c76b7082dcab4988)
  - Anything that is `instanceof Error` is now formatted as an error.
    - Refs: [`684dd28`](https://github.com/nodejs/node/commit/684dd28a6c684532336777348875ac11305727b9)
  - Circular references in JavaScript objects are now handled for the `%j` specifier.
    - Refs:  [`2cd7adc`](https://github.com/nodejs/node/commit/2cd7adc7f44e4dfe440162a31a168e6aa9a8cea1)
  - Custom `inspect` functions now receive any arguments passed to `util.inspect`.
    - Refs: [`07774e6`](https://github.com/nodejs/node/commit/07774e6b9570f90166a54fa87af74b8a7cf9926a)
  - Displays the constructor name if available.
    - Refs: [`7d14dd9`](https://github.com/nodejs/node/commit/7d14dd9b5e78faabb95d454a79faa513d0bbc2a5)
- The following utilities were deprecated in [`896b2aa`](https://github.com/nodejs/node/commit/896b2aa7074fc886efd7dd0a397d694763cac7ce):
  - `util.p()`, `util.debug()`, `util.error()` — Use [`console.error()`](https://iojs.org/api/console.html#console_console_error_data) instead.
  - `util.print()`, `util.puts()` — Use [`console.log()`](https://iojs.org/api/console.html#console_console_log_data) instead.
  - `util.exec()` — Now found at [`child_process.exec()`](https://iojs.org/api/child_process.html#child_process_child_process_exec_command_options_callback).
  - `util.pump()` — Use [`ReadableStream.prototype.pipe()`](https://iojs.org/api/stream.html#stream_readable_pipe_destination_options) instead.

### vm

[[Docs](https://iojs.org/api/vm.html)]

- The `vm` module has been rewritten to work better, based on the excellent [Contextify](https://github.com/brianmcd/contextify) native module. All of the functionality of Contextify is now in core, with improvements!
  - Refs: [`7afdba6`](https://github.com/nodejs/node/commit/7afdba6e0bc3b69c2bf5fdbd59f938ac8f7a64c5)
- Updated [`vm.createContext(sandbox)`](https://iojs.org/api/vm.html#vm_vm_createcontext_sandbox) to "contextify" the sandbox, making it suitable for use as a global for `vm` scripts, and then return it. It no longer creates a separate context object.
  - Refs: [`7afdba6`](https://github.com/nodejs/node/commit/7afdba6e0bc3b69c2bf5fdbd59f938ac8f7a64c5)
- Updated most `vm` and `vm.Script` methods to accept an `options` object, allowing you to configure a timeout for the script, the error display behavior, and sometimes the filename (for stack traces).
  - Refs: [`fd36576`](https://github.com/nodejs/node/commit/fd3657610e49005dfc778c3f060dbba0a34f286a)
- Updated the supplied sandbox object to be used directly as the global, remove error-prone copying of properties back and forth between the supplied sandbox object and the global that appears inside the scripts run by the `vm` module.
  - Refs: [`7afdba6`](https://github.com/nodejs/node/commit/7afdba6e0bc3b69c2bf5fdbd59f938ac8f7a64c5)

For more information, see the `vm` documentation linked above.


## JavaScript: `array.values()` has been removed since 0.12

- `array.values()` came from an unofficial release of V8 and has since been removed pending further work on V8's conformance with the spec. Instead of continuing to float a patch on top of V8, io.js and the converged repository are currently without support for it.
  - Convergence discussion: [archive#52](https://github.com/nodejs/node-convergence-archive/issues/52)

## General Node

- Unknown command-line options are now errors.
  - Refs: [`185c515`](https://github.com/nodejs/node/commit/185c515c9febf2229ed2ac76bfdd0c767ea7fd43)

---

- The built-in mdb_v8 bindings have been removed due to a lack of maintenance.
  - They may be re-added later in 4.x. Please see [#2517](https://github.com/nodejs/node/issues/2517) for more info.
  - Refs: [`37bb1df`](https://github.com/nodejs/node/commit/37bb1df7c4)

---

- Building node now has the following compiler version minimum requirements due to the newer v8 versions:
  - g++ 4.8 & gcc 4.2 OR clang++ 3.4 & clang 3.2
  - Refs: [`48774ec`](https://github.com/nodejs/node/commit/48774ec027a28cca17656659d316bb7ed8d6f33c)

## Dependencies

### c-ares
[[Repo](https://github.com/bagder/c-ares)]

[`1.9.0-DEV --> `](https://github.com/nodejs/node/commits/master/deps/cares)[`bagder/c-ares#bba4dc5`](https://github.com/bagder/c-ares/commit/bba4dc5) + [`7e1c0e7...08d0866`](https://github.com/nodejs/node/compare/7e1c0e7...08d0866)

This is roughly c-ares version 1.10.1. At this point the Node.js team effectively "maintains" c-ares.

### gtest
[[Repo](https://code.google.com/p/googletest/source/list)]

New in [`0080788`](https://github.com/nodejs/node/commit/008078862ed0731f2d1e74fb4182b67a29b31589), added to test c++ code. Only used in node's tests. Not exposed.

### http_parser
[[Repo](https://github.com/joyent/http-parser)]

[`1.0 --> 2.5.0`](https://github.com/nodejs/node/commits/master/deps/http_parser)

### libuv
[[Repo](https://github.com/libuv/libuv)]

[`0.10.36 --> ^1.6.1`](https://github.com/nodejs/node/commits/master/deps/uv)

### npm
[[Repo](https://github.com/npm/npm)]

[`1.4.28 --> ^2.13.3`](https://github.com/nodejs/node/commits/master/deps/npm)

Note that npm minor and patch versions are pulled into io.js and node at an almost-weekly rate.

Also, the version of npm that ships with node.js v4.0.0+ has had it's `node-gyp` dependency patched in a couple ways. `node-gyp` is used to compile native add-ons.
- Enabled the windows delay-load hook by default. [`3bda6cb`](https://github.com/nodejs/node/commit/3bda6cbfa4a9bb073790d53bc14e85b6e575bbe5)
  - A rare few native modules will not work with this on by default unless they are updated to fix the issues.
  - More info about the delay-load hook can be found at [`efadffe`](https://github.com/nodejs/node/commit/efadffe8616b895cd67fd270eadbd241d1069a47).
- Downloads the new headers-only tarballs introduced in io.js. [`e52f963`](https://github.com/nodejs/node/commit/e52f9636329dc5ecf3b6d79f8307e7817fff8e1e)
  - Instead of downloading full tarballs of node.js, we now download tarballs of only the headers, which are much smaller. Full tarballs are still available for nodejs.org.
- Nightly and pre-release versions of node.js are able to download tarballs. [`902c9ca`](https://github.com/nodejs/node/commit/902c9ca51d1e14a1ebce35177548260079a31b8c) & [`9f727f5`](https://github.com/nodejs/node/commit/9f727f5e03bbe6f798f0a084bf318f78f064013d)

### OpenSSL
[[Repo](https://github.com/openssl/openssl)]

[`1.0.1p --> 1.0.2d`](https://github.com/nodejs/node/commits/master/deps/openssl)

### punycode
[[Repo](https://github.com/bestiejs/punycode.js)]

[`v1.2.0 --> v1.3.2`](https://github.com/nodejs/node/commits/master/lib/punycode)

### zlib
[[Repo](https://github.com/madler/zlib)]

[`1.2.3 --> 1.2.8`](https://github.com/nodejs/node/commits/master/deps/zlib) + [`59ad4b0...a80b977`](https://github.com/nodejs/node/compare/59ad4b0...a80b977)

### V8
[`v3.14.5.9 --> v4.5.x`](https://github.com/nodejs/node/commits/master/deps/v8) + floating patches

The version bump is contains about 21 stable (n.X) V8 releases.

## c++

Virtually all native addons must be updated to work with 4.0.0 from 0.10 or 0.12. Please see [NAN's API docs](https://github.com/nodejs/nan#api) for help with migrating your native addons.