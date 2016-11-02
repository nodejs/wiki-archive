# Breaking changes between v6 and v7


When editing this page please be as detailed as possible.


For older breaking changes, please see our [v5 to v6](https://github.com/nodejs/node/wiki/Breaking-changes-between-v5-and-v6) page.


_62 commits were tagged `semver-major`._


**Note to readers**: `#` is synonymous with `.prototype.`, and indicates the property is available on instances of that class.
Example: `Object#toString()` is equivalent to `Object.prototype.toString()`.




## By Subsystem




### buffer


[[Docs](https://nodejs.org/dist/latest-v7.x/docs/api/buffer.html)]


- [`Buffer.byteLength()`](https://nodejs.org/dist/latest-v7.x/docs/api/buffer.html#buffer_class_method_buffer_bytelength_string_encoding) now throws if the input is not a String, Buffer, or ArrayBuffer.
  - Refs: [[`96b501d338`](https://github.com/nodejs/node/commit/96b501d338)], [#8946](https://github.com/nodejs/node/pull/8946)
- Constructing Buffers via `Buffer()` without using `new` is now deprecated. 
  - More info is avaliable on the [new Buffer construction APIs](https://nodejs.org/dist/latest-v7.x/docs/api/buffer.html#buffer_buffer_from_buffer_alloc_and_buffer_allocunsafe).
  - Refs: [[`f2fe5583c4`](https://github.com/nodejs/node/commit/f2fe5583c4)], [#8169](https://github.com/nodejs/node/pull/8169)
- `Buffer#toLocaleString()` is now an alias to `Buffer#toString()`.
  - Refs: [[`9cee8b1b62`](https://github.com/nodejs/node/commit/9cee8b1b62)], [#8148](https://github.com/nodejs/node/pull/8148)
- [`Buffer.allocUnsafe()`](https://nodejs.org/dist/latest-v7.x/docs/api/buffer.html#buffer_class_method_buffer_allocunsafe_size) now throws on negative input.
  - Refs: [[`8f90dcc1b8`](https://github.com/nodejs/node/commit/8f90dcc1b8)], [#7079](https://github.com/nodejs/node/pull/7079)




### child\_process


[[Docs](https://nodejs.org/dist/latest-v7.x/docs/api/child_process.html)]


- [`child_process.fork()`](https://nodejs.org/dist/latest-v7.x/docs/api/child_process.html#child_process_child_process_fork_modulepath_args_options) and `child_process.exec()` now do stricter argument validation. 
  - Refs: [[`0548e5d12a`](https://github.com/nodejs/node/commit/0548e5d12a)], [#7399](https://github.com/nodejs/node/pull/7399)
- [`child_process.spawn()`](https://nodejs.org/dist/latest-v6.x/docs/api/child_process.html#child_process_child_process_exec_command_options_callback) now spawns processes with the `/d` flag on Windows.
  - This turns off Windows `AutoRun` functionality by default for spawned processes.
  - Refs: [[`b90f3da9de`](https://github.com/nodejs/node/commit/b90f3da9de)], [#8063](https://github.com/nodejs/node/pull/8063)




### cluster


[[Docs](https://nodejs.org/dist/latest-v7.x/docs/api/cluster.html)]


- The unfortunately named `worker.suicide` property has been deprecated in favor of [`worker.exitedAfterDisconnect`](https://nodejs.org/dist/latest-v7.x/docs/api/cluster.html#cluster_worker_exitedafterdisconnect).
  - Refs: [[`f44b18f010`](https://github.com/nodejs/node/commit/f44b18f010)], [#3747](https://github.com/nodejs/node/pull/3747)




### crypto


[[Docs](https://nodejs.org/dist/latest-v7.x/docs/api/crypto.html)]


- The [`ECDH`](https://nodejs.org/dist/latest-v7.x/docs/api/crypto.html#crypto_class_ecdh) `’hybrid’` format option has been undocumented.
  - (Hybrid keys are illegal in X.509 certificates.)
  - Refs: [[`f4aa2c2c93`](https://github.com/nodejs/node/commit/f4aa2c2c93)], [#4956](https://github.com/nodejs/node/pull/4956)


### debugger


[[Docs](https://nodejs.org/dist/latest-v7.x/docs/api/debugger.html)]


- The debugger now once again listens on `127.0.0.1` by default.
  - Faulty behaviour causing the debugger to listen on `0.0.0.0` had been introduced in 0.11.13.
  - Refs: [[`8e7cbe2546`](https://github.com/nodejs/node/commit/8e7cbe2546)], [#8106](https://github.com/nodejs/node/pull/8106)




### dgram


[[Docs](https://nodejs.org/dist/latest-v7.x/docs/api/dgram.html)]


- All internal checks in `dgram` now use strict equality (type validation).
  - Most of these were checks for constants of some form.
  - Refs: [[`e9b6fbbf17`](https://github.com/nodejs/node/commit/e9b6fbbf17)], [#8011](https://github.com/nodejs/node/pull/8011)
- The remaining one-line trace of `unix_dgram` has been removed.
  - Unix datagram support was removed 5 years ago.
  - Refs: [[`6a3dbdacd6`](https://github.com/nodejs/node/commit/6a3dbdacd6)], [#8088](https://github.com/nodejs/node/pull/8088)




### domain


[[Docs](https://nodejs.org/dist/latest-v7.x/docs/api/domain.html)]

- The already-deprecated [`Domain#dispose()`](https://nodejs.org/dist/latest-v7.x/docs/api/domain.html#domain_domain_dispose) now has a custom deprecation message.
  - Refs:  [[`3b8ec68a3a`](https://github.com/nodejs/node/commit/3b8ec68a3a)], [#7053](https://github.com/nodejs/node/pull/7053)




### events


[[Docs](https://nodejs.org/dist/latest-v7.x/docs/api/events.html)]


- The [potential event listeners memory leak](https://nodejs.org/dist/latest-v7.x/docs/api/events.html#events_eventemitter_defaultmaxlisteners) warning is now named `MaxListenersExceededWarning`.
  - Refs: [[`983775d457`](https://github.com/nodejs/node/commit/983775d457)], [#8341](https://github.com/nodejs/node/pull/8341)
- [`EventEmitter#listeners()`](https://nodejs.org/dist/latest-v7.x/docs/api/events.html#events_emitter_listeners_eventname) not longer returns the `once` function wrappers for listeners registered via [`EventEmitter#once()`](https://nodejs.org/dist/latest-v7.x/docs/api/events.html#events_emitter_once_eventname_listener).
  - Refs: [[`b7a8a691b4`](https://github.com/nodejs/node/commit/b7a8a691b4)], [#6881](https://github.com/nodejs/node/pull/6881)




### fs


[[Docs](https://nodejs.org/dist/latest-v7.x/docs/api/fs.html)]


- Support for re-evaluating the `fs` module has now been dropped entirely.
  - This is a necessary step in improving the maintainability of Node.js core.
  - (The graceful-fs module will need to be updated to v4.x in all dependency trees.)
  - Refs: [[`49ef3ae90a`](https://github.com/nodejs/node/commit/49ef3ae90a)], [#6413](https://github.com/nodejs/node/pull/6413)
- `fs._stringToFlags()` has been removed and moved to `lib/internal`.
  - Refs: [[`7f7d1d385d`](https://github.com/nodejs/node/commit/7f7d1d385d)], [#7162](https://github.com/nodejs/node/pull/7162)
- Options object processing has been refactored for any `fs` methods that use options objects.
  - The error message for when an options argument is not an object has changed slightly.
  - Refs: [[`169f485289`](https://github.com/nodejs/node/commit/169f485289)], [#7165](https://github.com/nodejs/node/pull/7165)

Previously: `"options" argument must be a string or an object`

Now: `"options" must be a string or an object`


- The `’stop’` event emitted from [`fs.FSWatcher`](https://nodejs.org/dist/latest-v7.x/docs/api/fs.html#fs_class_fs_fswatcher) is now asynchronously emitted.
  - This prevents a potential infinite loop if stop is called synchronously after a listener is added.
   - Refs: [[`21124ba23a`](https://github.com/nodejs/node/commit/21124ba23a)], [#8524](https://github.com/nodejs/node/pull/8524)
- Calling async `fs` methods without a callback is now deprecated.
  - Refs: [[`f8f283b8f3`](https://github.com/nodejs/node/commit/f8f283b8f3)], [#7897](https://github.com/nodejs/node/pull/7897), [[`9359de9dd2`](https://github.com/nodejs/node/commit/9359de9dd2)], [#7168](https://github.com/nodejs/node/pull/7168)
- File Descriptors are now validated more strictly.
  - They must be valid unsigned 32-bit integers.
  - Refs: [[`c86c1eeab5`](https://github.com/nodejs/node/commit/c86c1eeab5)], [#2498](https://github.com/nodejs/node/pull/2498)




### http


[[Docs](https://nodejs.org/dist/latest-v7.x/docs/api/http.html)]


- The long-deprecated legacy `http.createClient()` interface has been removed.
  - Refs: [[`2cc7fa5e7d`](https://github.com/nodejs/node/commit/2cc7fa5e7d)], [#8104](https://github.com/nodejs/node/pull/8104)
- The error message for invalid [`trailer`s](https://nodejs.org/dist/latest-v7.x/docs/api/http.html#http_response_addtrailers_headers) has been corrected.
  - Refs: [[`31bef6b704`](https://github.com/nodejs/node/commit/31bef6b704)], [#6308](https://github.com/nodejs/node/pull/6308)

Previously: `The header content contains invalid characters`

Now: `The trailer content contains invalid characters`



### inspector


- `--inspect` now attaches to `process.debugPort` by default.
  - Refs: [[`9f1f7e2a34`](https://github.com/nodejs/node/commit/9f1f7e2a34)], [#8386](https://github.com/nodejs/node/pull/8386)



### intl


- The V8-specific [`intl` object](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Intl) function `v8BreakIterator()` has been deprecated.
  - Refs: [[`9ad3082b1c`](https://github.com/nodejs/node/commit/9ad3082b1c)], [#8908](https://github.com/nodejs/node/pull/8908)



### module


[[Docs](https://nodejs.org/dist/latest-v7.x/docs/api/modules.html)]

- The previously deprecated legacy `module.requireRepl()` function has been removed.
  - Refs:  [[`d582193613`](https://github.com/nodejs/node/commit/d582193613)], [#8575](https://github.com/nodejs/node/pull/8575)




### net


[[Docs](https://nodejs.org/dist/latest-v7.x/docs/api/net.html)]


- [`net.Server#listen()`](https://nodejs.org/dist/latest-v7.x/docs/api/net.html#net_server_listen_handle_backlog_callback) has been refactored and invalid option errors may differ slightly.
  - Refs: [[`fd6af98c2d`](https://github.com/nodejs/node/commit/fd6af98c2d)], [#4039](https://github.com/nodejs/node/pull/4039)


### os


[[Docs](https://nodejs.org/dist/latest-v7.x/docs/api/os.html)]

- `os.tmpDir()` has been deprecated in favor of [`os.tmpdir()`](https://nodejs.org/dist/latest-v7.x/docs/api/os.html#os_os_tmpdir), which was added as a replacement 3 years ago.
  - Refs: [[`5e5ec2cd1e`](https://github.com/nodejs/node/commit/5e5ec2cd1e)], [#6739](https://github.com/nodejs/node/pull/6739)




### process


[[Docs](https://nodejs.org/dist/latest-v7.x/docs/api/process.html)]


- The previously deprecated `process.EventEmitter` has been removed.
  - Was deprecated in v6.0.0.
  - Refs: [[`62b544290a`](https://github.com/nodejs/node/commit/62b544290a)], [#6862](https://github.com/nodejs/node/pull/6862)
- The `unhandledRejection` process event default handler now emits warnings for unhandled [Promise](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Promise) rejections.
  - This can be overridden by listening to the [`’unhandledRejection’`](https://nodejs.org/dist/latest-v7.x/docs/api/process.html#process_event_unhandledrejection) process event.
  - In the future as noted by the deprecation, unhandled Promises will terminate the node process with a non-zero exit code. (Likely on garbage collection.)
  - Refs: [[`ecf474ceba`](https://github.com/nodejs/node/commit/ecf474ceba)], [#8217](https://github.com/nodejs/node/pull/8217),  [[`07dbf7313d`](https://github.com/nodejs/node/commit/07dbf7313d)], [#8217](https://github.com/nodejs/node/pull/8217)


### punycode


[[Docs](https://nodejs.org/dist/latest-v7.x/docs/api/punycode.html)]

- The [punycode](https://nodejs.org/dist/latest-v7.x/docs/api/punycode.html) module is now deprecated in the docs, it’s functionality being replaced internally by ICU.
  - Refs: [[`29e49fc286`](https://github.com/nodejs/node/commit/29e49fc286)], [#7941](https://github.com/nodejs/node/pull/7941)




### readline


[[Docs](https://nodejs.org/dist/latest-v7.x/docs/api/readline.html)]


- Readline completions now autocompletes as much as possible on `TAB`, only showing the list of results when necessary.
  - Refs: [[`1a9e247c79`](https://github.com/nodejs/node/commit/1a9e247c79)], [#7754](https://github.com/nodejs/node/pull/7754)
- The following deprecated readline functions have been removed:
  - `codePointAt()`, `getStringWidth()`, `isFullWidthCodePoint()`, `stripVTControlCharacters()`.
  - These were previously undocumented, and then [deprecated in v6.0.0](https://github.com/nodejs/node/commit/ca2e8b292f0288d90e4e8c546adb459e2ab4eecd).
  - Refs: [[`8a87b29034`](https://github.com/nodejs/node/commit/8a87b29034)], [#6423](https://github.com/nodejs/node/pull/6423)




### repl


[[Docs](https://nodejs.org/dist/latest-v7.x/docs/api/repl.html)]


- The internally unused function `REPL#convertToContext()` has been runtime-deprecated.
  - Refs: [[`488d28d391`](https://github.com/nodejs/node/commit/488d28d391)], [#7829](https://github.com/nodejs/node/pull/7829)



### stream


[[Docs](https://nodejs.org/dist/latest-v7.x/docs/api/stream.html)]


- Error messages for unimplemented stream methods have been improved.
  - Refs: [[`2f05af4c06`](https://github.com/nodejs/node/commit/2f05af4c06)], [#8801](https://github.com/nodejs/node/pull/8801), [[`9983af0347`](https://github.com/nodejs/node/commit/9983af0347)], [#7671](https://github.com/nodejs/node/pull/7671)

Previously: `not implemented`

Now: `_read() is not implemented`, `_transform() is not implemented`, `_write() is not implemented`

- `TransformStream#_flush()` now accepts a second `data` argument: `_flush(err, data)`.
  - This now maintains consistency with other Transform Stream methods as hinted at in the docs.
  - Refs: [[`0cd0118334`](https://github.com/nodejs/node/commit/0cd0118334)], [#3708](https://github.com/nodejs/node/pull/3708)




### url


[[Docs](https://nodejs.org/dist/latest-v7.x/docs/api/url.html)]


- [`url.format()`](https://nodejs.org/dist/latest-v7.x/docs/api/url.html#url_url_format_urlobject) now returns valid `file://` urls.
  - Refs: [[`336b027411`](https://github.com/nodejs/node/commit/336b027411)], [#7234](https://github.com/nodejs/node/pull/7234)




### zlib


[[Docs](https://nodejs.org/dist/latest-v7.x/docs/api/zlib.html)]

- [zlib constants](https://nodejs.org/dist/latest-v7.x/docs/api/zlib.html#zlib_zlib_constants) have been moved to `zlib.constants`.
  - The constants are still available directly off of the zlib object, but are now docs-deprecated.
  - Refs: [[`197a465280`](https://github.com/nodejs/node/commit/197a465280)], [#7203](https://github.com/nodejs/node/pull/7203)


## Native Modules (Addons)


- The Native Module version mismatch error has been updated to be far more clear.
  - Refs: [[`1fda657cac`](https://github.com/nodejs/node/commit/1fda657cac)], [#8391](https://github.com/nodejs/node/pull/8391)

Previously:

```
Module version mismatch. Expected 51, got 48.
```

Now:

```
The module '<module>'
was compiled against a different Node.js version using
NODE_MODULE_VERSION 48. This version of Node.js requires
NODE_MODULE_VERSION 51. Please try re-compiling or Re-installing
the module (for instance, using `npm rebuild` or `npm install`).
```


- The `NODE_MODULE_VERSION` is now `51`.
  - Refs: [[`96933df2ff`](https://github.com/nodejs/node/commit/96933df2ff)], [#8808](https://github.com/nodejs/node/pull/8808)



## General Node


- Benchmarking has been completely overhauled.
  - Anything previously relying on `/benchmarks/` will probably no longer work.
  - Refs: [#7094](https://github.com/nodejs/node/pull/7094)


--


- libc++ is now always necessary for building on macOS.
  - Refs: [[`b032f1cfc3`](https://github.com/nodejs/node/commit/b032f1cfc3)], [#8317](https://github.com/nodejs/node/pull/8317)

--


- The Windows exit code for OS version mismatch is now the appropriate `ERROR_EXE_MACHINE_TYPE_MISMATCH`.
  - Refs: [[`a3c5567eb4`](https://github.com/nodejs/node/commit/a3c5567eb4)], [#8204](https://github.com/nodejs/node/pull/8204)

## Dependencies


- V8 has been upgraded to 5.4.500.36 + floating patches.
  - Refs: [deps/v8](https://github.com/nodejs/node/tree/v7.x/deps/v8), [`90efff6000`](https://github.com/nodejs/node/commit/90efff6000)], [#8317](https://github.com/nodejs/node/pull/8317)
