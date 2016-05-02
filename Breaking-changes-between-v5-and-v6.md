# Breaking changes between v5 and v6

When editing this page please be as detailed as possible.

For older breaking changes, please see our [v4 to v5](https://github.com/nodejs/node/wiki/Breaking-changes-between-v4-and-v5) page.

_89 commits were tagged `semver-major`._

**Note to readers**: `#` is synonymous with `.prototype.`, and indicates the property is available on instances of that class.
Example: `Object#toString()` is equivalent to `Object.prototype.toString()`.


## By Subsystem

### buffer

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/buffer.html)]

- Deprecated [`new Buffer()`](https://nodejs.org/dist/latest-v6.x/docs/api/buffer.html#buffer_new_buffer_array) in favor of a few [newly added Buffer APIs: `Buffer.from()`, `Buffer.alloc()` and `Buffer.allocUnsafe()`](https://nodejs.org/dist/latest-v6.x/docs/api/buffer.html#buffer_buffer_from_buffer_alloc_and_buffer_allocunsafe).
  - Refs: [`85ab4a5f12`](https://github.com/nodejs/node/commit/85ab4a5f12), [#4682](https://github.com/nodejs/node/pull/4682)
- Removed the previously deprecated `Buffer#write(string, encoding, offset, length)`.
  - [`Buffer#write()`](https://nodejs.org/dist/latest-v6.x/docs/api/buffer.html#buffer_buf_write_string_offset_length_encoding) remains with all other call signatures, such as `Buffer#write(string[, encoding])`.
  - Refs: [`2c55cc2d2c`](https://github.com/nodejs/node/commit/2c55cc2d2c),  [#5048](https://github.com/nodejs/node/pull/5048)
- Removed the previously deprecated `Buffer#{get|set}` methods.
  - `Buffer#get(index)` is superseded by `buffer[index]`.
  - `Buffer#set(index, value)` is superseded by `buffer[index] = value`.
  - Refs: [`101bca988c`](https://github.com/nodejs/node/commit/101bca988c), [#4594](https://github.com/nodejs/node/pull/4594)
- `new Buffer(length, encoding)` now throws.
  - The length argument had no effect if a number was passed, and this usage indicates a possible security problem.
  - Refs: [`3b27dd5ce1`](https://github.com/nodejs/node/commit/3b27dd5ce1), [#4514](https://github.com/nodejs/node/pull/4514)
- [`SlowBuffer`](https://nodejs.org/dist/latest-v6.x/docs/api/buffer.html#buffer_class_slowbuffer) has been given a docs deprecation notice in favor of the new `Buffer.allocUnsafeSlow()`.
  - Refs: [`3fe204c700`](https://github.com/nodejs/node/commit/3fe204c700), [`627524973a`](https://github.com/nodejs/node/commit/627524973a), [#5833](https://github.com/nodejs/node/pull/5833)

### cluster

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/cluster.html)]

- The `Worker#suicide` property has been deprecated in favor of the more descriptive [`Worker#exitedAfterDisconnect`](https://nodejs.org/dist/latest-v6.x/docs/api/cluster.html#cluster_worker_exitedafterdisconnect).
  - The functionality remains the same.
  - Refs: [`4f619bde4c`](https://github.com/nodejs/node/commit/4f619bde4c), [#3743](https://github.com/nodejs/node/pull/3743)
- The [`'message'`](https://nodejs.org/dist/latest-v6.x/docs/api/cluster.html#cluster_event_message_1) event from cluster now calls with 3 arguments, with worker being the first argument.
  - Previously the callback signature was `(message, handle)`, now it is `(worker, message, handle)`.
  - Refs: [`66f4586dd0`](https://github.com/nodejs/node/commit/66f4586dd0), [#5361](https://github.com/nodejs/node/pull/5361)

### console

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/console.html)]

- [`Console#timeEnd(label)`](https://nodejs.org/dist/latest-v6.x/docs/api/console.html#console_console_timeend_label) now internally cleans up after itself.
  - Previously it left the timer label in the `_times` map.
  - Refs: [`a5cce79ec3`](https://github.com/nodejs/node/commit/a5cce79ec3), [#3562](https://github.com/nodejs/node/pull/3562)
- `Console#timeEnd(label)` now only emits a warning if the label does not exist.
  - Previously, this would throw an error.
  - Refs: [`1c84579031`](https://github.com/nodejs/node/commit/1c84579031), [#5901](https://github.com/nodejs/node/pull/5901)

### crypto

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/crypto.html)]

- Error messages from the C++ code are now better formatted.
  - Refs: [`41feaa89e0`](https://github.com/nodejs/node/commit/41feaa89e0), [#3100](https://github.com/nodejs/node/pull/3100), [`1d9451bb5a`](https://github.com/nodejs/node/commit/1d9451bb5a), [#6042](https://github.com/nodejs/node/pull/6042)
- `require('crypto')` now throws if node has been built without crypto support.
  - Also happens for `require('tls')`, and `require('https')`.
  - Refs: [`f429fe1b88`](https://github.com/nodejs/node/commit/f429fe1b88), [#5611](https://github.com/nodejs/node/pull/5611)
- [`crypto.Certificate`](https://nodejs.org/dist/latest-v6.x/docs/api/crypto.html#crypto_class_certificate) no longer has `_handle` property.
  - Class methods that previously needed this now call to the c++ binding directly.
  - Refs: [`a37401e061`](https://github.com/nodejs/node/commit/a37401e061), [#5382](https://github.com/nodejs/node/pull/5382)
- The `digest` parameter for [`crypto.pbkdf2()`](https://nodejs.org/dist/latest-v6.x/docs/api/crypto.html#crypto_crypto_pbkdf2_password_salt_iterations_keylen_digest_callback) is now required.
  - Not using the digest parameter currently prints a deprecation warning
- The default encoding for all crypto methods is now `utf8`.
  - Previously, the encoding was `binary` (node's version of `latin1`).
  - Refs: [`b010c87164`](https://github.com/nodejs/node/commit/b010c87164), [#5522](https://github.com/nodejs/node/pull/5522)
- FIPS-compliant mode is now off by default even if node is built with FIPS-compliance.
  - Note: Regular node releases are not built with FIPS enabled.
  - Refs: [`7c48cb5601`](https://github.com/nodejs/node/commit/7c48cb5601), [#5181](https://github.com/nodejs/node/pull/5181)

### dgram

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/dgram.html)]

- If there is no error when calling [`Socket#send()`](https://nodejs.org/dist/latest-v6.x/docs/api/dgram.html#dgram_socket_send_msg_offset_length_port_address_callback), the callback's `error` parameter will now once again be `null`, rather than `0`.
  - This is how it was prior to [`c9fd9e2`](https://github.com/nodejs/node/commit/c9fd9e2162) in io.js 1.0.0.
  - Refs: [`4bc1cccb22`](https://github.com/nodejs/node/commit/4bc1cccb22), [#5929](https://github.com/nodejs/node/pull/5929)

### dns

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/dns.html)]

- [`dns.resolve()`](https://nodejs.org/dist/latest-v6.x/docs/api/dns.html#dns_dns_resolve_hostname_rrtype_callback) now supports resolving plain DNS PTR records.
  - Previously, calling `dns.resolve(hostname, 'PTR', cb)` would call `dns.reverse()` on the hostname. That is no longer the case.
  - The hostname must now be passed as a reverse *IN-ADDR* domain.
  - Refs: [`dbdbdd4998`](https://github.com/nodejs/node/commit/dbdbdd4998), [#4921](https://github.com/nodejs/node/pull/4921)

Before:

```js
dns.resolve('8.8.4.4', 'PTR', (err, result) => {
  if (err) {
    // handle error
  }
  // result => ['google-public-dns-b.google.com']
});
```

After:

```js
dns.resolve('4.4.8.8-in-addr.arpa', 'PTR', (err, result) => {
  if (err) {
    // handle error
  }
  // result => ['google-public-dns-b.google.com']
});

// one could also simply do
dns.reverse('8.8.4.4', (err, result) => {
  if (err) {
    // handle error
  }
  // result => ['google-public-dns-b.google.com']
});
```

- [`dns.lookupService()`](https://nodejs.org/dist/latest-v6.x/docs/api/dns.html#dns_dns_lookupservice_address_port_callback) now coerces the port parameter to a number.
  - Previously, if `port` was not a number, a `TypeError` would be thrown.
  - Now, if the port is outside the range of 0-65535, a `TypeError` will be thrown.
  - Refs: [`f3be421c1c`](https://github.com/nodejs/node/commit/f3be421c1c), [#4883](https://github.com/nodejs/node/pull/4883)

### domains

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/domains.html)]

- Domains no longer assign their context to other error handling code when there is no domain `'error'` event handler.
  - Previously, this was only the case if the `'error'` event from the domain was handled.
  - Refs: [`90204cc468`](https://github.com/nodejs/node/commit/90204cc468), [#4659](https://github.com/nodejs/node/pull/4659)

### events

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/events.html)]

- The internal event handler storage object `EventEmitter#_events` now inherits from `Object.create(null)` rather than `Object.prototype`.
  - This prevents issues with events using otherwise reserved property names such as `__proto__`.
  - This also means that any properties that modules intentionally add to `Object.prototype` will not be available on `_events`.
  - Refs: [`e38bade828`](https://github.com/nodejs/node/commit/e38bade828), [#6092](https://github.com/nodejs/node/pull/6092)

### freelist

- The deprecated `freelist` module has been removed.
  - This module was intended to be internal only and we had no intention of maintaining it beyond our own use.
  - Use-cases for this would be better suited using a user-land module.
  - Refs: [`b70dc67828`](https://github.com/nodejs/node/commit/b70dc67828), [#3738](https://github.com/nodejs/node/pull/3738)

### fs

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/fs.html)]

- [`fs.readdir{Sync}()`](https://nodejs.org/dist/latest-v6.x/docs/api/fs.html#fs_fs_readdir_path_options_callback) now returns filenames in utf8 by default.
  - The encoding of filenames is now configurable via an options object.
  - Example: `fs.readdir(path, { encoding: 'hex' }, callback)`
  - Refs: [`060e5f0c00`](https://github.com/nodejs/node/commit/060e5f0c00), [#5616](https://github.com/nodejs/node/pull/5616)
- Deprecated re-evaluating the `fs` source code from user code.
  - Refs: [`1d79787e2e`](https://github.com/nodejs/node/commit/1d79787e2e), [#5102](https://github.com/nodejs/node/pull/5102)
- Deprecated [`fs.read()`](https://nodejs.org/dist/latest-v6.x/docs/api/fs.html#fs_fs_read_fd_buffer_offset_length_position_callback)'s legacy `(fd, length, position, encoding, callback)` call signature.
  - Refs: [`1124de2d76`](https://github.com/nodejs/node/commit/1124de2d76), [#4525](https://github.com/nodejs/node/pull/4525)
- `fs.read()` with a read length of 0 no longer throws.
  - Refs: [`2b15e68bbe`](https://github.com/nodejs/node/commit/2b15e68bbe), [#4518](https://github.com/nodejs/node/pull/4518)
- [`fs.link{Sync}()`](https://nodejs.org/dist/latest-v6.x/docs/api/fs.html#fs_fs_link_srcpath_dstpath_callback) now does its call parameter checks in the correct order.
  - Refs: [`8b97249893`](https://github.com/nodejs/node/commit/8b97249893), [#3917](https://github.com/nodejs/node/pull/3917)
- [`fs.realpath{Sync}()`](https://nodejs.org/dist/latest-v6.x/docs/api/fs.html#fs_fs_realpath_path_options_callback) now uses `uv_fs_realpath()` under the hood, rather than using a JavaScript implementation.
  - The `cache` parameter no longer accepts an object cache, and instead has been superseded by an options parameter.
  - Refs: [`b488b19eaf`](https://github.com/nodejs/node/commit/b488b19eaf), [#3594](https://github.com/nodejs/node/pull/3594)

### globals

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/globals.html)]

- Deprecated the `root` and `GLOBAL` aliases to [`global`](https://nodejs.org/dist/latest-v6.x/docs/api/globals.html#globals_global).
  - Refs: [`4e46931406`](https://github.com/nodejs/node/commit/4e46931406), [#1838](https://github.com/nodejs/node/pull/1838)

### module

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/modules.html)]

- The current directory is now prioritized for relative lookups.
  - Previously `node_module` directories would be prioritized if present.
  - For example, `require('./example')` would previously require `node_modules/example` if it existed, rather than `./example.js`.
  - Refs: [`d38503ab01`](https://github.com/nodejs/node/commit/d38503ab01), [#5689](https://github.com/nodejs/node/pull/5689)
- Symlinks are now preserved when using [`require()`](https://nodejs.org/dist/latest-v6.x/docs/api/globals.html#globals_require)
  - Refs: [`de1dc0ae2e`](https://github.com/nodejs/node/commit/de1dc0ae2e), [#5950](https://github.com/nodejs/node/pull/5950).
- Syntax errors in `require()`'d files now print with more information.
  - Refs: [`18490d3d5a`](https://github.com/nodejs/node/commit/18490d3d5a), [#4287](https://github.com/nodejs/node/pull/4287)

### net

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/net.html)]

- Valid port checking is now stricter.
  - Now ensures possible values such as `true` and `[1]` are not seen as valid ports.
  - Refs: [`d0edabecbf`](https://github.com/nodejs/node/commit/d0edabecbf), [#5733](https://github.com/nodejs/node/pull/5733), [`02ac302b6d`](https://github.com/nodejs/node/commit/02ac302b6d), [#5732](https://github.com/nodejs/node/pull/5732)
- [`net.createServer()`](https://nodejs.org/dist/latest-v6.x/docs/api/net.html#net_net_createserver_options_connectionlistener) now throws if a supplied `options` argument is not an object.
  - It is still possible to only supply a connectionListener callback.
  - Refs: [`a78b3344f8`](https://github.com/nodejs/node/commit/a78b3344f8), [#2904](https://github.com/nodejs/node/pull/2904)
- The `V4MAPPED` DNS hint is no longer set by default. However, the `ADDRCONFIG` is still set.
  - If your platform needs to set hints, you can now use the new `hints` bitwise flag option for [`Socket#connect()`](https://nodejs.org/dist/latest-v6.x/docs/api/net.html#net_socket_connect_options_connectlistener).
  - Refs: [`b85a50b6da`](https://github.com/nodejs/node/commit/b85a50b6da), [`54dd7c38e5`](https://github.com/nodejs/node/commit/54dd7c38e5), [#6021](https://github.com/nodejs/node/pull/6021)

### path

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/path.html)]

- All of the path module's utilities now throw if the provided input is not a string.
  - Refs: [`08085c49b6`](https://github.com/nodejs/node/commit/08085c49b6), [#5348](https://github.com/nodejs/node/pull/5348)
- [`path.format()`](https://nodejs.org/dist/latest-v6.x/docs/api/path.html#path_path_format_pathobject) is now more consistent & functional across platforms.
  - Refs: [`d1000b4137`](https://github.com/nodejs/node/commit/d1000b4137), [#2408](https://github.com/nodejs/node/pull/2408)

### process

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/process.html)]

- Accessing `process.EventEmitter` now prints a deprecation warning.
  - It was long since deprecated in the source code.
  - Refs: [`25751bedfe`](https://github.com/nodejs/node/commit/25751bedfe), [#5049](https://github.com/nodejs/node/pull/5049)
- All previously printed node warnings are now more consistent and only emitted via the default handler for a new process [`'warning'`](https://nodejs.org/dist/latest-v6.x/docs/api/process.html#process_event_warning) event.
  - This include deprecations, which are now classified as `DeprecationWarning`s.
  - Refs: [`c6656db352`](https://github.com/nodejs/node/commit/c6656db352), [#4782](https://github.com/nodejs/node/pull/4782)
- [`process.nextTick()`](https://nodejs.org/dist/latest-v6.x/docs/api/process.html#process_process_nexttick_callback_arg) now throws if the argument is not a function.
  - Refs: [`72e3dd9f43`](https://github.com/nodejs/node/commit/72e3dd9f43), [#3860](https://github.com/nodejs/node/pull/3860)

### querystring

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/querystring.html)]

- The parsed object returned by [`querystring.parse()`](https://nodejs.org/dist/latest-v6.x/docs/api/querystring.html#querystring_querystring_parse_str_sep_eq_options) now inherits from `Object.create(null)` rather than `Object.prototype`.
  - This prevents issues with querystring properties using otherwise reserved property names such as `__proto__`.
  - This also means that any properties that modules intentionally add to `Object.prototype` will not be available on the returned object..
  - Refs: [`dba245f796`](https://github.com/nodejs/node/commit/dba245f796), [#6055](https://github.com/nodejs/node/pull/6055)
- [`querystring.escape()`](https://nodejs.org/dist/latest-v6.x/docs/api/querystring.html#querystring_querystring_escape) now uses `Object#toString()` for objects rather than `Object#valueOf()`.
  - This brings it more in-line with the functionality of `encodeURIComponent()`.
  - [`5dafb435d8`](https://github.com/nodejs/node/commit/5dafb435d8), [#5341](https://github.com/nodejs/node/pull/5341)

### readline

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/readline.html)]

- Readline history can now be disabled by setting the [`createInterface()`](https://nodejs.org/dist/latest-v6.x/docs/api/readline.html#readline_readline_createinterface_options) option `historySize` to `0`.
  - Previously, setting to `0` would just use the default of `30` lines.
  - Refs: [`0303a2552e`](https://github.com/nodejs/node/commit/0303a2552e), [#6352](https://github.com/nodejs/node/pull/6352)
- Deprecated the following undocumented readline functions, which are only intended for internal use:
  - `isFullWidthCodePoint()`, `stripVTControlCharacters()`, `getStringWidth()`, `emitKeys()`
  - Refs: [`ca2e8b292f`](https://github.com/nodejs/node/commit/ca2e8b292f), [#3862](https://github.com/nodejs/node/pull/3862)
- [`Readline#emitKeypressEvents(stream)`](https://nodejs.org/dist/latest-v6.x/docs/api/readline.html#readline_readline_emitkeypressevents_stream) now always provides the key info parameter in `'keypress'` events to the provided stream.
  - Refs: [`0a62f929da`](https://github.com/nodejs/node/commit/0a62f929da), [#6024](https://github.com/nodejs/node/pull/6024)

### repl

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/repl.html)]

- It is now possible to assign a variable to `_`, which usually holds the result of the last expression within the REPL.
  - Doing this will print a warning and disable the behavior of holding the last expression.
  - Refs: [`ad8257fa5b`](https://github.com/nodejs/node/commit/ad8257fa5b), [5535](https://github.com/nodejs/node/pull/5535)
- Improvements have been made that decrease the number of errors when REPL completion fails.
  - Refs: [`3ee68f794f`](https://github.com/nodejs/node/commit/3ee68f794f), [6328](https://github.com/nodejs/node/pull/6328)

### stream

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/stream.html)]

- [Writing](https://nodejs.org/dist/latest-v6.x/docs/api/stream.html#stream_writable_write_chunk_encoding_callback) a `null` chunk while in [object mode](https://nodejs.org/dist/latest-v6.x/docs/api/stream.html#stream_object_mode) is now invalid and will throw a TypeError.
  - Refs: [`e7c077c610`](https://github.com/nodejs/node/commit/e7c077c610), [#6170](https://github.com/nodejs/node/pull/6170)

### timers

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/timers.html)]

- `set{`[`Timeout`](https://nodejs.org/dist/latest-v6.x/docs/api/timers.html#timers_settimeout_callback_delay_arg)`|`[`Interval`](https://nodejs.org/dist/latest-v6.x/docs/api/timers.html#timers_setinterval_callback_delay_arg)`|`[`Immediate`](https://nodejs.org/dist/latest-v6.x/docs/api/timers.html#timers_setimmediate_callback_arg)`}()` now throw immediately if they are not provided functions.
  - These methods already threw for non-functions, just when it would timeout.
  - Refs: [`ac153bd2a6`](https://github.com/nodejs/node/commit/ac153bd2a6), [#4362](https://github.com/nodejs/node/pull/4362)

### tls

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/tls.html)]

- [`tls.Server`](https://nodejs.org/dist/latest-v6.x/docs/api/tls.html#tls_class_tls_server)'s `'clientError'` is now [`'tlsClientError'`](https://nodejs.org/dist/latest-v6.x/docs/api/tls.html#tls_event_tlsclienterror).
  - This was done because `http` now has [`'clientError'`](https://nodejs.org/dist/latest-v6.x/docs/api/http.html#http_event_clienterror).
  - Refs: [`1ab6b21360`](https://github.com/nodejs/node/commit/1ab6b21360), [#4557](https://github.com/nodejs/node/pull/4557)
- [`tls.createServer()`](https://nodejs.org/dist/latest-v6.x/docs/api/tls.html#tls_tls_createserver_options_secureconnectionlistener)'s `sessionIdContext` now uses sha1 instead of md5 to make the default hash.
  - This still only applies if `sessionIdContext` is not set manually, and `requestCert` is set to `true`.
  - Refs: [`df268f97bc`](https://github.com/nodejs/node/commit/df268f97bc), [#3866](https://github.com/nodejs/node/pull/3866)
- [`tls.createSecurePair()`](https://nodejs.org/dist/latest-v6.x/docs/api/tls.html#tls_tls_createsecurepair_context_isserver_requestcert_rejectunauthorized_options) has been deprecated in the docs in favor of [`TLSSocket`](https://nodejs.org/dist/latest-v6.x/docs/api/tls.html#tls_class_tls_tlssocket).
  - Refs: [`31de5cc436`](https://github.com/nodejs/node/commit/31de5cc436), [#6063](https://github.com/nodejs/node/pull/6063)

### tty

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/tty.html)]

- The previously deprecated, global `tty.setRawMode()` method has been removed.
  - Please use the [tty instance method](https://nodejs.org/dist/latest-v6.x/docs/api/tty.html#tty_rs_setrawmode_mode) instead.
  - Refs: [`a2c0aa84e0`](https://github.com/nodejs/node/commit/a2c0aa84e0), [#2528](https://github.com/nodejs/node/pull/2528)

### url

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/url.html)]

- [`url.resolve()`](https://nodejs.org/dist/latest-v6.x/docs/api/url.html#url_url_resolve_from_to) now drops auth information if the host changes.
  - This is a security measure to help ensure authentication credentials are not leaked.
  - Refs: [`eb4201f07a`](https://github.com/nodejs/node/commit/eb4201f07a), [#1480](https://github.com/nodejs/node/pull/1480)

### util

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/util.html)]

- Subclassed errors now format as `[MyError: message]` rather than just `MyError {}`.
  - Refs: [`e2f47f5698`](https://github.com/nodejs/node/commit/e2f47f5698), [#4582](https://github.com/nodejs/node/pull/4582)
- `Date`'s now always use `Date#toISOString()` for formatting.
  - Refs: [`93d6b5fb68`](https://github.com/nodejs/node/commit/93d6b5fb68), [#4318](https://github.com/nodejs/node/pull/4318)
- [`util.inspect()`](https://nodejs.org/dist/latest-v6.x/docs/api/util.html#util_util_inspect_object_options) now uses c++ bindings to detect JavaScript builtins & primitives.
  - Refs: [`24012a879d`](https://github.com/nodejs/node/commit/24012a879d), [#4098](https://github.com/nodejs/node/pull/4098)
- Removed the previously deprecated `util.pump()`. Please use [`ReadableStream#pipe()`](https://nodejs.org/dist/latest-v6.x/docs/api/stream.html#stream_readable_pipe_destination_options) instead.
  - Refs: [`007cfea308`](https://github.com/nodejs/node/commit/007cfea308), [#2531](https://github.com/nodejs/node/pull/2531)
- Removed the previously deprecated `util.exec()`. Please use [`child_process.exec()`](https://nodejs.org/dist/latest-v6.x/docs/api/child_process.html#child_process_child_process_exec_command_options_callback) instead.
  - Refs: [`4cf19ad1bb`](https://github.com/nodejs/node/commit/4cf19ad1bb), [#2530](https://github.com/nodejs/node/pull/2530)
- TypedArrays now format like regular arrays.
  - Also applies to ArrayBuffer and DataView.
  - Refs: [`34a35919e1`](https://github.com/nodejs/node/commit/34a35919e1), [#3793](https://github.com/nodejs/node/pull/3793)
- [`util._extend()`](https://nodejs.org/dist/latest-v6.x/docs/api/util.html#util_util_extend_obj) has been deprecated in the docs in favor of [`Object.assign()`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign).
  - Refs: [`d8290286b3`](https://github.com/nodejs/node/commit/d8290286b3), [#4903](https://github.com/nodejs/node/pull/4903)
- [`util.log()`](https://nodejs.org/dist/latest-v6.x/docs/api/util.html#util_util_log_string) has been given a docs deprecation notice.
  - Refs: [`236b7e8dd1`](https://github.com/nodejs/node/commit/236b7e8dd1), [#6161](https://github.com/nodejs/node/pull/6161)

### vm

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/vm.html)]

- The [`vm.Script`](https://nodejs.org/dist/latest-v6.x/docs/api/vm.html#vm_class_script) option `displayErrors` now attaches the line of code that caused the error to the stack trace.
  - Refs: [`57003520f8`](https://github.com/nodejs/node/commit/57003520f8), [#4874](https://github.com/nodejs/node/pull/4874)

### zlib

[[Docs](https://nodejs.org/dist/latest-v6.x/docs/api/zlib.html)]

- The `close` event from zlib instances no longer emits on synchronous calls.
  - This only effects all `*Sync()` methods.
  - Refs: [`8b43d3f52d`](https://github.com/nodejs/node/commit/8b43d3f52d), [#5707](https://github.com/nodejs/node/pull/5707)
- Gzip trailing garbage after a gzip stream is no longer discarded and now throws an error instead.
  - Note: Null byte padding is not affected, since it has been pointed
  out at various occasions that such padding is normal and
  discarded by `gzip(1)`, too.
  - Refs: [`54a5287e3e`](https://github.com/nodejs/node/commit/54a5287e3e), [#5883](https://github.com/nodejs/node/pull/5883)

## Native Modules (Addons)

- The module ABI has changed due to a minor addition to module initialization.
  - This only means that native addons will need to be recompiled.
  - Refs: [`71470a8e45`](https://github.com/nodejs/node/commit/71470a8e45), [#4771](https://github.com/nodejs/node/pull/4771)
- The `NODE_MODULE_VERSION` is now `48`.
- Some previously deprecated internal functions have been removed.
  - Refs: [`757fbac64b`](https://github.com/nodejs/node/commit/757fbac64b), [#6053](https://github.com/nodejs/node/pull/6053)

## General Node

- Internal tooling no longer ships in node tarballs, reducing their size by about 10%.
  - Refs: [`90a5fc20be`](https://github.com/nodejs/node/commit/90a5fc20be), [#5695](https://github.com/nodejs/node/pull/5695)

--

- All printed warnings are now prefixed with `(node:pid)`.
  - Refs: [`d01eb6882f`](https://github.com/nodejs/node/commit/d01eb6882f), [#3878](https://github.com/nodejs/node/pull/3878), [`94b9948d63`](https://github.com/nodejs/node/commit/94b9948d63), [#3833](https://github.com/nodejs/node/pull/3833)

--

- Error messages are now more consistent across all modules.
  - All now start with a capital letter, contain no other regular words with capitals, and do not contain ending periods.
  - Additionally, argument names and other code is now always between double quotations (`"`).
  - In some cases, errors are also now more informative.
  - Refs: [`20285ad177`](https://github.com/nodejs/node/commit/20285ad177), [#3374](https://github.com/nodejs/node/pull/3374), [`53a95a5b12`](https://github.com/nodejs/node/commit/53a95a5b12), [#5616](https://github.com/nodejs/node/pull/5616), [`8bb60e3c8d`](https://github.com/nodejs/node/commit/8bb60e3c8d), [#5590](https://github.com/nodejs/node/pull/5590), [`ec49fc8229`](https://github.com/nodejs/node/commit/ec49fc8229), [#5981](https://github.com/nodejs/node/pull/5981)

--

- Node.js no longer supports Windows Vista or previous versions, and will refuse to run on those versions of windows.
  - Additionally, the installer will not install on those windows versions.
  - The minimum supported versions of windows are now Windows 7 and Windows Server 2008 R2.
  - Refs: [`1cf26c036d`](https://github.com/nodejs/node/commit/1cf26c036d), [`55db19074d`](https://github.com/nodejs/node/commit/55db19074d), [#5167](https://github.com/nodejs/node/pull/5167)

--

- Node.js no longer supports building on OS X versions older than 10.7.
  - Refs: [`204f3a8a0b`](https://github.com/nodejs/node/commit/204f3a8a0b), [#6402](https://github.com/nodejs/node/pull/6402)

--

- Installing via `Makefile` (`tools/install.py`) no longer attempts to change the target location of node in npm's shebang to the locally built node.
  - Instead, it is kept as `#!/usr/bin/env node`, which looks for node globally.
  - Refs: [`8ffa20c495`](https://github.com/nodejs/node/commit/8ffa20c495), [#6098](https://github.com/nodejs/node/pull/6098)

## Dependencies

- Shared c-ares builds are now supported once again.
  - Refs: [`2253be95d0`](https://github.com/nodejs/node/commit/2253be95d0), [#5775](https://github.com/nodejs/node/pull/5775)
- V8 has been upgraded to 5.0.71.32 + floating patches.
  - Refs: [deps/v8](https://github.com/nodejs/node/commits/master/deps/v8)
