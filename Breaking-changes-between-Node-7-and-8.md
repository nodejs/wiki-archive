# Breaking changes between Node 7 and 8

**Authors note (Fishrock123): This is highly unfinished copy from shortly after Node 8 became _Current_.**


When editing this page please be as detailed as possible.


For older breaking changes, please see our [Node 6 to 7](https://github.com/nodejs/node/wiki/Breaking-changes-between-v6-and-v7) page.


_183 commits were tagged `semver-major`._


**Note to readers**: `#` is synonymous with `.prototype.`, and indicates the property is available on instances of that class.
Example: `Object#toString()` is equivalent to `Object.prototype.toString()`.




## By Subsystem

### assert

[[Docs](https://nodejs.org/dist/latest-v8.x/docs/api/assert.html)]

* `assert.AssertException` is now a `class`, resulting in an updated error message when invoked without the `new` keyword.
  - Previously, invoking without `new` would result in a undescriptive `TypeError: Cannot assign to read only property 'name' of function ...` error.
  - Refs: [[`e48d58b8b2`](https://github.com/nodejs/node/commit/e48d58b8b2)],  [#12651](https://github.com/nodejs/node/pull/12651)
* When `assert.fail()` is provided a single argument, it now interprets it as the `message` parameter.
  - This behavior is equivalent to `assert.fail(undefined, undefined, <first argument>)`.
  - Refs: [[`758b8b6e5d`](https://github.com/nodejs/node/commit/758b8b6e5d)], [#12293](https://github.com/nodejs/node/pull/12293)
* The `deepEqual` and `deepStrictEqual` functions now check equality on the contents of `Set`s and `Map`s.
  - Previously, these functions did not deeply compare `Set`s or `Map`s and would return true when comparing any two regardless of their contents.
  - Refs: [[`6481c93aef`](https://github.com/nodejs/node/commit/6481c93aef)], [#12142](https://github.com/nodejs/node/pull/12142)
* !!!!!!!!!  [[`efec14a7d1`](https://github.com/nodejs/node/commit/efec14a7d1)] - **(SEMVER-MAJOR)** **assert**: enforce type check in deepStrictEqual (Joyee Cheung) [#10282](https://github.com/nodejs/node/pull/10282)
* !!!!!!!!!  [[`562cf5a81c`](https://github.com/nodejs/node/commit/562cf5a81c)] - **(SEMVER-MAJOR)** **assert**: fix premature deep strict comparison (Joyee Cheung) [#11128](https://github.com/nodejs/node/pull/11128)
* `assert.throws(fn)` has a slightly updated error message when the provided function does not throw.
  - The error message no longer has two periods.
  - Refs: [[`0af41834f1`](https://github.com/nodejs/node/commit/0af41834f1)], [#11254](https://github.com/nodejs/node/pull/11254)


### buffer

[[Docs](https://nodejs.org/dist/latest-v8.x/docs/api/buffer.html)]

* !!!!!! [[`d2d32ea5a2`](https://github.com/nodejs/node/commit/d2d32ea5a2)] - **(SEMVER-MAJOR)** **buffer**: add pending deprecation warning (James M Snell) [#11968](https://github.com/nodejs/node/pull/11968)
* The memory allocated by `new Buffer(Number)` is now zero-filled by default.
  - Refs: [[`7eb1b4658e`](https://github.com/nodejs/node/commit/7eb1b4658e)], [#12141](https://github.com/nodejs/node/pull/12141)
* `Buffer#write()`, `Buffer#fill()` and `Buffer.from()` now ignore invalid `hex` strings.
  - Previously: `Buffer.from('abx', 'hex')` caused: `TypeError: Invalid hex string`.
  - Now: `Buffer.from('abx', 'hex')` results in: `<Buffer ab>`.
  - Refs: [[`682573c11d`](https://github.com/nodejs/node/commit/682573c11d)], [#12012](https://github.com/nodejs/node/pull/12012)
* `Buffer#toString()` now throws an error if an unknown encoding is passed.
  - `buf.toString(0, 1, 2)` now throws: `TypeError: Unknown encoding: 0`.
  - If `undefined` is passed or implied, the behavior is unchanged ad still defaults to `utf8`.
  - Refs: [[`9a0829d728`](https://github.com/nodejs/node/commit/9a0829d728)], [#11120](https://github.com/nodejs/node/pull/11120)
* All `Buffer` methods that would accept being passed a `Buffer` may now also be passed `Uint8Array` instead.
  - Error messages have been updated to reflect this change in behavior.
  - Refs: [[`beca3244e2`](https://github.com/nodejs/node/commit/beca3244e2)], [#10236](https://github.com/nodejs/node/pull/10236)
* The error message for oversized buffers is now more consistent.
  - Now: `RangeError: "size" argument must not be larger than <buffer.kMaxLength>`
  - Refs: [[`3d353c749c`](https://github.com/nodejs/node/commit/3d353c749c)], [#10152](https://github.com/nodejs/node/pull/10152)


### child\_process

[[Docs](https://nodejs.org/dist/latest-v8.x/docs/api/child_process.html)]

* !!!!!!!! [[`97a77288ce`](https://github.com/nodejs/node/commit/97a77288ce)] - **(SEMVER-MAJOR)** **child_process**: improve ChildProcess validation (cjihrig) [#12348](https://github.com/nodejs/node/pull/12348)
* !!!!!!!! [[`d75fdd96aa`](https://github.com/nodejs/node/commit/d75fdd96aa)] - **(SEMVER-MAJOR)** **child_process**: improve killSignal validations (Sakthipriyan Vairamani (thefourtheye)) [#10423](https://github.com/nodejs/node/pull/10423)
* The `stdio` option for `child_process.fork()` now has a consistent error message with `child_process.spawn()`.
  - Now: `TypeError: Incorrect value of stdio option: <String>`.
  - Refs: [[`4cafa60c99`](https://github.com/nodejs/node/commit/4cafa60c99)] - **(SEMVER-MAJOR)** **child_process**: align fork/spawn stdio error msg (Sam Roberts) [#11044](https://github.com/nodejs/node/pull/11044)
* !!!!! [[`3268863ebc`](https://github.com/nodejs/node/commit/3268863ebc)] - **(SEMVER-MAJOR)** **child_process**: add string shortcut for fork stdio (Javis Sullivan) [#10866](https://github.com/nodejs/node/pull/10866)
* The `maxBuffer` option for `child_process` functions can now be set to any positive number, including `Infinity`.
  - Refs: [[`8f3ff98f0e`](https://github.com/nodejs/node/commit/8f3ff98f0e)], [#10769](https://github.com/nodejs/node/pull/10769)
  * All `child_process` methods that would accept being passed a `Buffer` may now also be passed `Uint8Array` instead.
    - Error messages have been updated to reflect this change in behavior.
    - Refs: [[`627ecee9ed`](https://github.com/nodejs/node/commit/627ecee9ed)], [#10653](https://github.com/nodejs/node/pull/10653)
* !!!!!  [[`fc7b0dda85`](https://github.com/nodejs/node/commit/fc7b0dda85)] - **(SEMVER-MAJOR)** **child_process**: improve input validation (cjihrig) [#8312](https://github.com/nodejs/node/pull/8312)
* Errors from `exec{File}Sync()` will no longer sometimes have extra newlines.
  - Refs: [[`49d1c366d8`](https://github.com/nodejs/node/commit/49d1c366d8)], [#9343](https://github.com/nodejs/node/pull/9343)


### console

* The console will no longer throw any stream error events by default.
  - This bring behavior closer in line to browsers.
  - For example, the following now works as expected: `node -e "console.log(1);console.log(2);" | head -1`.
  - Refs: [[`f18e08d820`](https://github.com/nodejs/node/commit/f18e08d820)], [#9744](https://github.com/nodejs/node/pull/9744)



### crypto

[[Docs](https://nodejs.org/dist/latest-v8.x/docs/api/crypto.html)]

* [[`a8f460f12d`](https://github.com/nodejs/node/commit/a8f460f12d)] - **(SEMVER-MAJOR)** **crypto**: support all ArrayBufferView types (Timothy Gu) [#12223](https://github.com/nodejs/node/pull/12223)
* [[`0db49fef41`](https://github.com/nodejs/node/commit/0db49fef41)] - **(SEMVER-MAJOR)** **crypto**: support Uint8Array prime in createDH (Anna Henningsen) [#11983](https://github.com/nodejs/node/pull/11983)
* [[`443691a5ae`](https://github.com/nodejs/node/commit/443691a5ae)] - **(SEMVER-MAJOR)** **crypto**: fix default encoding of LazyTransform (Lukas Möller) [#8611](https://github.com/nodejs/node/pull/8611)
* `crypto.pbkdf2{Sync}()` will now throw an error if the `digest` argument is `undefined`.
  - Previously, this produced a deprecation warning.
  - Refs: [[`9f74184e98`](https://github.com/nodejs/node/commit/9f74184e98)], [#11305](https://github.com/nodejs/node/pull/11305)
* [[`e90f38270c`](https://github.com/nodejs/node/commit/e90f38270c)] - **(SEMVER-MAJOR)** **crypto**: throw error in CipherBase::SetAutoPadding (Kirill Fomichev) [#9405](https://github.com/nodejs/node/pull/9405)
* [[`1ef401ce92`](https://github.com/nodejs/node/commit/1ef401ce92)] - **(SEMVER-MAJOR)** **crypto**: use check macros in CipherBase::SetAuthTag (Kirill Fomichev) [#9395](https://github.com/nodejs/node/pull/9395)


### debugger

[[Docs](https://nodejs.org/dist/latest-v8.x/docs/api/debugger.html)]

* [[`7599b0ef9d`](https://github.com/nodejs/node/commit/7599b0ef9d)] - **(SEMVER-MAJOR)** **debug**: activate inspector with `_debugProcess` (Eugene Ostroukhov) [#11431](https://github.com/nodejs/node/pull/11431)
* The now-obsolete `_debug_agent` module has been removed.
  - Refs: [[`549e81bfa1`](https://github.com/nodejs/node/commit/549e81bfa1)], [#12582](https://github.com/nodejs/node/pull/12582)


### dgram

[[Docs](https://nodejs.org/dist/latest-v8.x/docs/api/dgram.html)]

* [[`e912c67d24`](https://github.com/nodejs/node/commit/e912c67d24)] - **(SEMVER-MAJOR)** **dgram**: convert to using internal/errors (Michael Dawson) [#12926](https://github.com/nodejs/node/pull/12926)
* [[`2dc1053b0a`](https://github.com/nodejs/node/commit/2dc1053b0a)] - **(SEMVER-MAJOR)** **dgram**: support Uint8Array input to send() (Anna Henningsen) [#11985](https://github.com/nodejs/node/pull/11985)
* The `address` argument to `dgram.Socket#send()` is now optional and does additional validation.
  - Now if `address` is not a `String` the following error will occur: `TypeError: Invalid arguments: address must be a nonempty string or falsy`.
  - Refs: [[`32679c73c1`](https://github.com/nodejs/node/commit/32679c73c1)], [#10473](https://github.com/nodejs/node/pull/10473)


### dns


* [[`5587ff1ccd`](https://github.com/nodejs/node/commit/5587ff1ccd)] - **(SEMVER-MAJOR)** **dns**: handle implicit bind DNS errors (cjihrig) [#11036](https://github.com/nodejs/node/pull/11036)



### domain

[[Docs](https://nodejs.org/dist/latest-v8.x/docs/api/domain.html)]



### events

[[Docs](https://nodejs.org/dist/latest-v8.x/docs/api/events.html)]

* The error message for error events with no listeners has been updated.
  - Previously: `Error: Uncaught, unspecified "error" event`.
  - Now: `Error: Unhandled "error" event`.
  - Refs: [[`2141d37452`](https://github.com/nodejs/node/commit/2141d37452)], [#10387](https://github.com/nodejs/node/pull/10387)
* The setter for `events.defaultMaxListeners` now does value validation and must be a positive number (including `Infinity`).
  - The new validation error is as follows: `TypeError: defaultMaxListeners must be a positive number`.
  - Refs: [[`221b03ad20`](https://github.com/nodejs/node/commit/221b03ad20)], [#11938](https://github.com/nodejs/node/pull/11938)


### fs

[[Docs](https://nodejs.org/dist/latest-v8.x/docs/api/fs.html)]

* [[`4cb5f3daa3`](https://github.com/nodejs/node/commit/4cb5f3daa3)] - **(SEMVER-MAJOR)** **fs**: throw on invalid callbacks for async functions (Sakthipriyan Vairamani (thefourtheye)) [#12562](https://github.com/nodejs/node/pull/12562)
* `fs.utimes()` will now throw an error if the `atime` or `mtime` arguments are `NaN` or `Infinity`.
  - Refs: [[`eed87b1637`](https://github.com/nodejs/node/commit/eed87b1637)], [#11919](https://github.com/nodejs/node/pull/11919)
* [[`71097744b2`](https://github.com/nodejs/node/commit/71097744b2)] - **(SEMVER-MAJOR)** **fs**: more realpath*() optimizations (Brian White) [#11665](https://github.com/nodejs/node/pull/11665)
* [[`6a5ab5d550`](https://github.com/nodejs/node/commit/6a5ab5d550)] - **(SEMVER-MAJOR)** **fs**: include more fs.stat*() optimizations (Brian White) [#11665](https://github.com/nodejs/node/pull/11665)
* [[`1c3df96570`](https://github.com/nodejs/node/commit/1c3df96570)] - **(SEMVER-MAJOR)** **fs**: replace regexp with function (Brian White) [#10789](https://github.com/nodejs/node/pull/10789)
* [[`34c9fc2e4e`](https://github.com/nodejs/node/commit/34c9fc2e4e)] - **(SEMVER-MAJOR)** **fs**: avoid multiple conversions to string (Brian White) [#10789](https://github.com/nodejs/node/pull/10789)
* [[`21b2440176`](https://github.com/nodejs/node/commit/21b2440176)] - **(SEMVER-MAJOR)** **fs**: avoid recompilation of closure (Brian White) [#10789](https://github.com/nodejs/node/pull/10789)
* `fs.SyncWriteStream` has been issued a runtime deprecation.
  - The deprecation code for this API is `DEP0061`.
  - Previously, this API was docs-deprecated.
  - Refs: [[`7a55e34ef4`](https://github.com/nodejs/node/commit/7a55e34ef4)], [#10467](https://github.com/nodejs/node/pull/10467)
* [[`b1fc7745f2`](https://github.com/nodejs/node/commit/b1fc7745f2)] - **(SEMVER-MAJOR)** **fs**: avoid emitting error EBADF for double close (Matteo Collina) [#11225](https://github.com/nodejs/node/pull/11225)
* The "string interface" for `fs.read()` has been removed.
  - Previously worked, but was deprecated: `fs.read(fd, length, position, encoding, callback)`
  - Refs: [[`3c2a9361ff`](https://github.com/nodejs/node/commit/3c2a9361ff)], [#9683](https://github.com/nodejs/node/pull/9683)
* `fs.readFile(path, encoding, (err, data) => {})` will no longer return with `data` as a `Buffer` if an `err` was returned from an internal `.toString()` failure.
  - Refs: [[`f3cf8e9808`](https://github.com/nodejs/node/commit/f3cf8e9808)], [#9670](https://github.com/nodejs/node/pull/9670)



### http

[[Docs](https://nodejs.org/dist/latest-v8.x/docs/api/http.html)]


* `http.ClientRequest` (used by `http.request()`) now does additional `options.host{name}` validation.
  - `undefined` and `null` still default to `'localhost'`.
  - Now, other non-string values cause: `TypeError: "options.hostname" must either be a string, undefined or null`.
  - Refs: [[`85a4e25775`](https://github.com/nodejs/node/commit/85a4e25775)], [#12494](https://github.com/nodejs/node/pull/12494)
* !!!!  [[`90403dd1d0`](https://github.com/nodejs/node/commit/90403dd1d0)], [#11567](https://github.com/nodejs/node/pull/11567)
* `http.IncomingMessage#headers.cookie` now concatenates multiple cookie headers with semicolons (`;`) rather than commas (`,`).
  - Previously: `'foo=bar, baz=boo'`.
  - Now: `'foo=bar; baz=boo'`.
  - Refs: [[`6b2cef65c9`](https://github.com/nodejs/node/commit/6b2cef65c9)], [#11259](https://github.com/nodejs/node/pull/11259)
* [[`d3480776c7`](https://github.com/nodejs/node/commit/d3480776c7)] - **(SEMVER-MAJOR)** **http**: concatenate outgoing Cookie headers (Brian White) [#11259](https://github.com/nodejs/node/pull/11259)
* The widely-used but private `http.ServerResponse` properties `_headers`, `_headerNames`, & `_renderHeaders` have been docs-deprecated.
  - The deprecation code for `_headers` and `_headerNames` is `DEP0066`, and the code for `_renderHeaders` is `DEP0067`.
  - More information can be found under the deprecation codes.
  - Refs: [[`8243ca0e0e`](https://github.com/nodejs/node/commit/8243ca0e0e)], [#10941](https://github.com/nodejs/node/pull/10941)
* `http.ServerResponse#writeHeader()` has been docs-deprecated with a code of `DEP0063`.
  - It is an alias to `ServerResponse#writeHead()`.
  - Refs: [[`fb71ba4921`](https://github.com/nodejs/node/commit/fb71ba4921)], [#11355](https://github.com/nodejs/node/pull/11355)
* `http.ServerResponse#writeHead()` now reports a non-number-coerced status code in the error message if the status code is not valid.
  - Refs: [[`a4bb9fdb89`](https://github.com/nodejs/node/commit/a4bb9fdb89)], [#11221](https://github.com/nodejs/node/pull/11221)
* [[`fc7025c9c0`](https://github.com/nodejs/node/commit/fc7025c9c0)] - **(SEMVER-MAJOR)** **http**: throw an error for unexpected agent values (brad-decker) [#10053](https://github.com/nodejs/node/pull/10053)
* [[`176cdc2823`](https://github.com/nodejs/node/commit/176cdc2823)] - **(SEMVER-MAJOR)** **http**: misc optimizations and style fixes (Brian White) [#10558](https://github.com/nodejs/node/pull/10558)
* [[`73d9445782`](https://github.com/nodejs/node/commit/73d9445782)] - **(SEMVER-MAJOR)** **http**: try to avoid lowercasing incoming headers (Brian White) [#10558](https://github.com/nodejs/node/pull/10558)
* [[`c77ed327d9`](https://github.com/nodejs/node/commit/c77ed327d9)] - **(SEMVER-MAJOR)** **http**: avoid using object for removed header status (Brian White) [#10558](https://github.com/nodejs/node/pull/10558)
* [[`c00e4adbb4`](https://github.com/nodejs/node/commit/c00e4adbb4)] - **(SEMVER-MAJOR)** **http**: optimize header storage and matching (Brian White) [#10558](https://github.com/nodejs/node/pull/10558)
* [[`ec8910bcea`](https://github.com/nodejs/node/commit/ec8910bcea)] - **(SEMVER-MAJOR)** **http**: check statusCode early (Brian White) [#10558](https://github.com/nodejs/node/pull/10558)
* [[`a73ab9de0d`](https://github.com/nodejs/node/commit/a73ab9de0d)] - **(SEMVER-MAJOR)** **http**: remove pointless use of arguments (cjihrig) [#10664](https://github.com/nodejs/node/pull/10664)
* [[`df3978421b`](https://github.com/nodejs/node/commit/df3978421b)] - **(SEMVER-MAJOR)** **http**: verify client method is a string (Luca Maraschi) [#10111](https://github.com/nodejs/node/pull/10111)



### inspector



### intl


### linklist

* [[`b40dab553f`](https://github.com/nodejs/node/commit/b40dab553f)] - **(SEMVER-MAJOR)** **linkedlist**: remove unused methods (Brian White) [#11726](https://github.com/nodejs/node/pull/11726)
* [[`84a23391f6`](https://github.com/nodejs/node/commit/84a23391f6)] - **(SEMVER-MAJOR)** **linkedlist**: remove public module (Brian White) [#12113](https://github.com/nodejs/node/pull/12113)


### module

[[Docs](https://nodejs.org/dist/latest-v8.x/docs/api/modules.html)]

* [[`e32425bfcd`](https://github.com/nodejs/node/commit/e32425bfcd)] - **(SEMVER-MAJOR)** **module**: avoid JSON.stringify() for cache key (Brian White) [#10789](https://github.com/nodejs/node/pull/10789)
* [[`403b89e72b`](https://github.com/nodejs/node/commit/403b89e72b)] - **(SEMVER-MAJOR)** **module**: avoid hasOwnProperty() (Brian White) [#10789](https://github.com/nodejs/node/pull/10789)
* [[`298a40e04e`](https://github.com/nodejs/node/commit/298a40e04e)] - **(SEMVER-MAJOR)** **module**: use "clean" objects (Brian White) [#10789](https://github.com/nodejs/node/pull/10789)


### net

[[Docs](https://nodejs.org/dist/latest-v8.x/docs/api/net.html)]

* `net.Server#address()` now propagates internal `getsockname()` errors.
  - This means that `net.Server#address()` could now throw any libuv error that `_handle.getsockname()` might return.
  - Refs: [[`cf980b0311`](https://github.com/nodejs/node/commit/cf980b0311)], [#12871](https://github.com/nodejs/node/pull/12871)



### process

[[Docs](https://nodejs.org/dist/latest-v8.x/docs/api/process.html)]

* [[`03e89b3ff2`](https://github.com/nodejs/node/commit/03e89b3ff2)] - **(SEMVER-MAJOR)** **process**: add --redirect-warnings command line argument (James M Snell) [#10116](https://github.com/nodejs/node/pull/10116)
* [[`5e1f32fd94`](https://github.com/nodejs/node/commit/5e1f32fd94)] - **(SEMVER-MAJOR)** **process**: add optional code to warnings + type checking (James M Snell) [#10116](https://github.com/nodejs/node/pull/10116)
* [[`a647d82f83`](https://github.com/nodejs/node/commit/a647d82f83)] - **(SEMVER-MAJOR)** **process**: improve process.hrtime (Joyee Cheung) [#10764](https://github.com/nodejs/node/pull/10764)


### querystring

* [[`4e259b21a3`](https://github.com/nodejs/node/commit/4e259b21a3)] - **(SEMVER-MAJOR)** **querystring, url**: handle repeated sep in search (Daijiro Wachi) [#10967](https://github.com/nodejs/node/pull/10967)


### repl

[[Docs](https://nodejs.org/dist/latest-v8.x/docs/api/repl.html)]

* [[`39d9afe279`](https://github.com/nodejs/node/commit/39d9afe279)] - **(SEMVER-MAJOR)** **repl**: refactor `LineParser` implementation (Blake Embrey) [#6171](https://github.com/nodejs/node/pull/6171)
* The `replMode` option `repl.REPL_MODE_MAGIC` has been docs-deprecated, with a code of `DEP00XX`.
  - The behavior is now equivalent to `repl.REPL_MODE_SLOPPY`.
  - This also applies for the environment variable: `NODE_REPL_MODE=magic`.
  - Refs: [[`3f27f02da0`](https://github.com/nodejs/node/commit/3f27f02da0)], [#11599](https://github.com/nodejs/node/pull/11599)
* [[`007386ee81`](https://github.com/nodejs/node/commit/007386ee81)] - **(SEMVER-MAJOR)** **repl**: remove workaround for function redefinition (Michaël Zasso) [#9618](https://github.com/nodejs/node/pull/9618)



### stream

[[Docs](https://nodejs.org/dist/latest-v8.x/docs/api/stream.html)]

* [[`f8c617dbe2`](https://github.com/nodejs/node/commit/f8c617dbe2)] - **(SEMVER-MAJOR)** **stream**: improve multiple callback error message (cjihrig) [#12520](https://github.com/nodejs/node/pull/12520)
* [[`330c8d743e`](https://github.com/nodejs/node/commit/330c8d743e)] - **(SEMVER-MAJOR)** **stream**: add `destroy` and `_destroy` methods. (Matteo Collina) [#12925](https://github.com/nodejs/node/pull/12925)
* [[`202b07f414`](https://github.com/nodejs/node/commit/202b07f414)] - **(SEMVER-MAJOR)** **stream**: fix comment for sync flag of ReadableState (Wang Xinyong) [#11139](https://github.com/nodejs/node/pull/11139)
* [[`1004b9b4ad`](https://github.com/nodejs/node/commit/1004b9b4ad)] - **(SEMVER-MAJOR)** **stream**: remove unused `ranOut` from ReadableState (Wang Xinyong) [#11139](https://github.com/nodejs/node/pull/11139)
* [[`03b9f6fe26`](https://github.com/nodejs/node/commit/03b9f6fe26)] - **(SEMVER-MAJOR)** **stream**: avoid instanceof (Brian White) [#10558](https://github.com/nodejs/node/pull/10558)
* [[`a3539ae3be`](https://github.com/nodejs/node/commit/a3539ae3be)] - **(SEMVER-MAJOR)** **stream**: use plain objects for write/corked reqs (Brian White) [#10558](https://github.com/nodejs/node/pull/10558)


### string_decoder

* [[`24ef1e6775`](https://github.com/nodejs/node/commit/24ef1e6775)] - **(SEMVER-MAJOR)** **string_decoder**: align UTF-8 handling with V8 (Brian White) [#9618](https://github.com/nodejs/node/pull/9618)


### tls

* [[`348cc80a3c`](https://github.com/nodejs/node/commit/348cc80a3c)] - **(SEMVER-MAJOR)** **tls**: make rejectUnauthorized default to true (ghaiklor) [#5923](https://github.com/nodejs/node/pull/5923)
* [[`a2ae08999b`](https://github.com/nodejs/node/commit/a2ae08999b)] - **(SEMVER-MAJOR)** **tls**: runtime deprecation for tls.createSecurePair() (James M Snell) [#11349](https://github.com/nodejs/node/pull/11349)
* [[`d523eb9c40`](https://github.com/nodejs/node/commit/d523eb9c40)] - **(SEMVER-MAJOR)** **tls**: use emitWarning() for dhparam \< 2048 bits (James M Snell) [#11447](https://github.com/nodejs/node/pull/11447)


### tty

* The `NODE_TTY_UNSAFE_ASYNC` environment variable detection has been removed.
  - This was previously a stop-gap for a theoretical behavior change.
  - Refs: [[`1b63fa1096`](https://github.com/nodejs/node/commit/1b63fa1096)], [#12057](https://github.com/nodejs/node/pull/12057)


### url

[[Docs](https://nodejs.org/dist/latest-v8.x/docs/api/url.html)]

* `url.format()` now provides a correct error message if it is not called with a `urlObject`.
  - Previously: `TypeError: undefined`, or other broken variants.
  - Now: `TypeError: Parameter "urlObj" must be an object, not <type>`.
  - Refs: [[`78182458e6`](https://github.com/nodejs/node/commit/78182458e6)], [#11162](https://github.com/nodejs/node/pull/11162)
* `url.parse()` no longer truncates long hostnames.
  - Refs: [[`c65d55f087`](https://github.com/nodejs/node/commit/c65d55f087)], [#9292](https://github.com/nodejs/node/pull/9292)


### util

* `util.inspect()` now shows `v8::External` C++ objects as `[External]`, rather than `{}`.
  - Refs: [[`3cc3e099be`](https://github.com/nodejs/node/commit/3cc3e099be)], [#12151](https://github.com/nodejs/node/pull/12151)
* `util.inspect()` now shows deeply nested Arrays as `[Array]`, rather than `[Object]`.
  - Refs: [[`4a5a9445b5`](https://github.com/nodejs/node/commit/4a5a9445b5)], [#12046](https://github.com/nodejs/node/pull/12046)
* `util.inspect()` now shows enumerable Symbol keys by default.
  - This can be configured by setting `util.inspect.defaultOptions.showHidden`.
  - Refs: [[`5bfd13b81e`](https://github.com/nodejs/node/commit/5bfd13b81e)], [#9726](https://github.com/nodejs/node/pull/9726)
* The `%j` (`JSON`) specifier conversion in `util.format()` now propagates non-circular-reference errors.
  - Refs: [[`455e6f1dd8`](https://github.com/nodejs/node/commit/455e6f1dd8)], [#11708](https://github.com/nodejs/node/pull/11708)
* `util.inspect` has
[[`ec2f098156`](https://github.com/nodejs/node/commit/ec2f098156)] - **(SEMVER-MAJOR)** **util**: change sparse arrays inspection format (Alexey Orlenko) [#11576](https://github.com/nodejs/node/pull/11576)
* [[`aab0d202f8`](https://github.com/nodejs/node/commit/aab0d202f8)] - **(SEMVER-MAJOR)** **util**: convert inspect.styles and inspect.colors to prototype-less objects (Nemanja Stojanovic) [#11624](https://github.com/nodejs/node/pull/11624)
* [[`4151ab398b`](https://github.com/nodejs/node/commit/4151ab398b)] - **(SEMVER-MAJOR)** **util**: add createClassWrapper to internal/util (James M Snell) [#11391](https://github.com/nodejs/node/pull/11391)
* `util.inspect()` now has improved display for Async Functions and Generators.
  - Previously, either was formatted as `[Function]`.
  - Now, Async Functions: `[AsyncFunction]`, Generators: `[GeneratorFunction]`.
  - Refs: [[`f65aa08b52`](https://github.com/nodejs/node/commit/f65aa08b52)], [#11210](https://github.com/nodejs/node/pull/11210)


### zlib

[[Docs](https://nodejs.org/dist/latest-v8.x/docs/api/zlib.html)]

* All of the Zlib TransformStream options now do stricter validation.
  - Options no longer conflate `0` with `undefined`.
  - Numeric options no longer accept `NaN` or `Infinity`.
  - Refs: [[`9e4660b518`](https://github.com/nodejs/node/commit/9e4660b518)], [#13098](https://github.com/nodejs/node/pull/13098)
* [[`2ced07ccaf`](https://github.com/nodejs/node/commit/2ced07ccaf)] - **(SEMVER-MAJOR)** **zlib**: support all ArrayBufferView types (Timothy Gu) [#12223](https://github.com/nodejs/node/pull/12223)
* [[`91383e47fd`](https://github.com/nodejs/node/commit/91383e47fd)] - **(SEMVER-MAJOR)** **zlib**: support Uint8Array in convenience methods (Timothy Gu) [#12001](https://github.com/nodejs/node/pull/12001)
* [[`b514bd231e`](https://github.com/nodejs/node/commit/b514bd231e)] - **(SEMVER-MAJOR)** **zlib**: use RangeError/TypeError consistently (James M Snell) [#11391](https://github.com/nodejs/node/pull/11391)
* [[`8e69f7e385`](https://github.com/nodejs/node/commit/8e69f7e385)] - **(SEMVER-MAJOR)** **zlib**: refactor zlib module (James M Snell) [#11391](https://github.com/nodejs/node/pull/11391)
* [[`dd928b04fc`](https://github.com/nodejs/node/commit/dd928b04fc)] - **(SEMVER-MAJOR)** **zlib**: be strict about what strategies are accepted (Rich Trott) [#10934](https://github.com/nodejs/node/pull/10934)


## Native Modules (Addons)



Previously:



- The `NODE_MODULE_VERSION` is now `51`.
  - Refs: [[`96933df2ff`](https://github.com/nodejs/node/commit/96933df2ff)], [#8808](https://github.com/nodejs/node/pull/8808)



## General Node


## Dependencies


- V8 has been upgraded to 5.4.500.36 + floating patches.
  - Refs: [deps/v8](https://github.com/nodejs/node/tree/v7.x/deps/v8), [`90efff6000`](https://github.com/nodejs/node/commit/90efff6000)], [#8317](https://github.com/nodejs/node/pull/8317)






#
#
#
#
#
#
#
#
#
#
#
#


* [[`bf5c309b5e`](https://github.com/nodejs/node/commit/bf5c309b5e)] - **(SEMVER-MAJOR)** **build**: fix V8 build on FreeBSD (Michaël Zasso) [#12784](https://github.com/nodejs/node/pull/12784)
* [[`a1028d5e3e`](https://github.com/nodejs/node/commit/a1028d5e3e)] - **(SEMVER-MAJOR)** **build**: remove cares headers from tarball (Gibson Fahnestock) [#10283](https://github.com/nodejs/node/pull/10283)
* [[`d08836003c`](https://github.com/nodejs/node/commit/d08836003c)] - **(SEMVER-MAJOR)** **build**: build an x64 build by default on Windows (Nikolai Vavilov) [#11537](https://github.com/nodejs/node/pull/11537)
* [[`92ed1ab450`](https://github.com/nodejs/node/commit/92ed1ab450)] - **(SEMVER-MAJOR)** **build**: change nosign flag to sign and flips logic (Joe Doyle) [#10156](https://github.com/nodejs/node/pull/10156)

* [[`c0d858f8bb`](https://github.com/nodejs/node/commit/c0d858f8bb)] - **(SEMVER-MAJOR)** **deps**: upgrade npm beta to 5.0.0-beta.56 (Kat Marchán) [#12936](https://github.com/nodejs/node/pull/12936)
* [[`6690415696`](https://github.com/nodejs/node/commit/6690415696)] - **(SEMVER-MAJOR)** **deps**: cherry-pick a927f81c7 from V8 upstream (Anna Henningsen) [#11752](https://github.com/nodejs/node/pull/11752)
* [[`60d1aac8d2`](https://github.com/nodejs/node/commit/60d1aac8d2)] - **(SEMVER-MAJOR)** **deps**: update V8 to 5.8.283.38 (Michaël Zasso) [#12784](https://github.com/nodejs/node/pull/12784)
* [[`b7608ac707`](https://github.com/nodejs/node/commit/b7608ac707)] - **(SEMVER-MAJOR)** **deps**: cherry-pick node-inspect#43 (Ali Ijaz Sheikh) [#11441](https://github.com/nodejs/node/pull/11441)
* [[`9c9e2d7f4a`](https://github.com/nodejs/node/commit/9c9e2d7f4a)] - **(SEMVER-MAJOR)** **deps**: backport 3297130 from upstream V8 (Michaël Zasso) [#11752](https://github.com/nodejs/node/pull/11752)
* [[`07088e6fc1`](https://github.com/nodejs/node/commit/07088e6fc1)] - **(SEMVER-MAJOR)** **deps**: backport 39642fa from upstream V8 (Michaël Zasso) [#11752](https://github.com/nodejs/node/pull/11752)
* [[`8394b05e20`](https://github.com/nodejs/node/commit/8394b05e20)] - **(SEMVER-MAJOR)** **deps**: cherry-pick c5c570f from upstream V8 (Michaël Zasso) [#11752](https://github.com/nodejs/node/pull/11752)
* [[`fcc58bf0da`](https://github.com/nodejs/node/commit/fcc58bf0da)] - **(SEMVER-MAJOR)** **deps**: cherry-pick a927f81c7 from V8 upstream (Anna Henningsen) [#11752](https://github.com/nodejs/node/pull/11752)
* [[`83bf2975ec`](https://github.com/nodejs/node/commit/83bf2975ec)] - **(SEMVER-MAJOR)** **deps**: cherry-pick V8 ValueSerializer changes (Anna Henningsen) [#11752](https://github.com/nodejs/node/pull/11752)
* [[`c459d8ea5d`](https://github.com/nodejs/node/commit/c459d8ea5d)] - **(SEMVER-MAJOR)** **deps**: update V8 to 5.7.492.69 (Michaël Zasso) [#11752](https://github.com/nodejs/node/pull/11752)
* [[`7c0c7baff3`](https://github.com/nodejs/node/commit/7c0c7baff3)] - **(SEMVER-MAJOR)** **deps**: fix gyp configuration for v8-inspector (Michaël Zasso) [#10992](https://github.com/nodejs/node/pull/10992)
* [[`00a2aa0af5`](https://github.com/nodejs/node/commit/00a2aa0af5)] - **(SEMVER-MAJOR)** **deps**: fix gyp build configuration for Windows (Michaël Zasso) [#10992](https://github.com/nodejs/node/pull/10992)
* [[`b30ec59855`](https://github.com/nodejs/node/commit/b30ec59855)] - **(SEMVER-MAJOR)** **deps**: switch to v8_inspector in V8 (Ali Ijaz Sheikh) [#10992](https://github.com/nodejs/node/pull/10992)
* [[`7a77daf243`](https://github.com/nodejs/node/commit/7a77daf243)] - **(SEMVER-MAJOR)** **deps**: update V8 to 5.6.326.55 (Michaël Zasso) [#10992](https://github.com/nodejs/node/pull/10992)
* [[`c9e5178f3c`](https://github.com/nodejs/node/commit/c9e5178f3c)] - **(SEMVER-MAJOR)** **deps**: hide zlib internal symbols (Sam Roberts) [#11082](https://github.com/nodejs/node/pull/11082)
* [[`2739185b79`](https://github.com/nodejs/node/commit/2739185b79)] - **(SEMVER-MAJOR)** **deps**: update V8 to 5.5.372.40 (Michaël Zasso) [#9618](https://github.com/nodejs/node/pull/9618)

* [[`eb535c5154`](https://github.com/nodejs/node/commit/eb535c5154)] - **(SEMVER-MAJOR)** **doc**: deprecate vm.runInDebugContext (Josh Gavant) [#12243](https://github.com/nodejs/node/pull/12243)
* [[`75c471a026`](https://github.com/nodejs/node/commit/75c471a026)] - **(SEMVER-MAJOR)** **doc**: drop PPC BE from supported platforms (Michael Dawson) [#12309](https://github.com/nodejs/node/pull/12309)
* [[`86996c5838`](https://github.com/nodejs/node/commit/86996c5838)] - **(SEMVER-MAJOR)** **doc**: deprecate private http properties (Brian White) [#10941](https://github.com/nodejs/node/pull/10941)
* [[`3d8379ae60`](https://github.com/nodejs/node/commit/3d8379ae60)] - **(SEMVER-MAJOR)** **doc**: improve assert.md regarding ECMAScript terms (Joyee Cheung) [#11128](https://github.com/nodejs/node/pull/11128)
* [[`d708700c68`](https://github.com/nodejs/node/commit/d708700c68)] - **(SEMVER-MAJOR)** **doc**: deprecate buffer's parent property (Sakthipriyan Vairamani (thefourtheye)) [#8332](https://github.com/nodejs/node/pull/8332)
* [[`03d440e3ce`](https://github.com/nodejs/node/commit/03d440e3ce)] - **(SEMVER-MAJOR)** **doc**: document buffer.buffer property (Sakthipriyan Vairamani (thefourtheye)) [#8332](https://github.com/nodejs/node/pull/8332)

* [[`f0b702555a`](https://github.com/nodejs/node/commit/f0b702555a)] - **(SEMVER-MAJOR)** **errors**: use lazy assert to avoid issues on startup (James M Snell) [#11300](https://github.com/nodejs/node/pull/11300)
* [[`251e5ed8ee`](https://github.com/nodejs/node/commit/251e5ed8ee)] - **(SEMVER-MAJOR)** **errors**: assign error code to bootstrap_node created error (James M Snell) [#11298](https://github.com/nodejs/node/pull/11298)
* [[`e75bc87d22`](https://github.com/nodejs/node/commit/e75bc87d22)] - **(SEMVER-MAJOR)** **errors**: port internal/process errors to internal/errors (James M Snell) [#11294](https://github.com/nodejs/node/pull/11294)
* [[`76327613af`](https://github.com/nodejs/node/commit/76327613af)] - **(SEMVER-MAJOR)** **errors, child_process**: migrate to using internal/errors (James M Snell) [#11300](https://github.com/nodejs/node/pull/11300)
* [[`1c834e78ff`](https://github.com/nodejs/node/commit/1c834e78ff)] - **(SEMVER-MAJOR)** **errors,test**: migrating error to internal/errors (larissayvette) [#11505](https://github.com/nodejs/node/pull/11505)

* [[`90476ac6ee`](https://github.com/nodejs/node/commit/90476ac6ee)] - **(SEMVER-MAJOR)** **lib**: remove `_debugger.js` (Ben Noordhuis) [#12495](https://github.com/nodejs/node/pull/12495)
* [[`3209a8ebf3`](https://github.com/nodejs/node/commit/3209a8ebf3)] - **(SEMVER-MAJOR)** **lib**: ensure --check flag works for piped-in code (Teddy Katz) [#11689](https://github.com/nodejs/node/pull/11689)
* [[`c67207731f`](https://github.com/nodejs/node/commit/c67207731f)] - **(SEMVER-MAJOR)** **lib**: simplify `Module._resolveLookupPaths` (Brian White) [#10789](https://github.com/nodejs/node/pull/10789)
* [[`28dc848e70`](https://github.com/nodejs/node/commit/28dc848e70)] - **(SEMVER-MAJOR)** **lib**: improve method of function calling (Brian White) [#10789](https://github.com/nodejs/node/pull/10789)
* [[`a851b868c0`](https://github.com/nodejs/node/commit/a851b868c0)] - **(SEMVER-MAJOR)** **lib**: remove sources of permanent deopts (Brian White) [#10789](https://github.com/nodejs/node/pull/10789)
* [[`62e96096fa`](https://github.com/nodejs/node/commit/62e96096fa)] - **(SEMVER-MAJOR)** **lib**: more consistent use of module.exports = {} model (James M Snell) [#11406](https://github.com/nodejs/node/pull/11406)
* [[`88c3f57cc3`](https://github.com/nodejs/node/commit/88c3f57cc3)] - **(SEMVER-MAJOR)** **lib**: refactor internal/socket_list (James M Snell) [#11406](https://github.com/nodejs/node/pull/11406)
* [[`f04387e9f2`](https://github.com/nodejs/node/commit/f04387e9f2)] - **(SEMVER-MAJOR)** **lib**: refactor internal/freelist (James M Snell) [#11406](https://github.com/nodejs/node/pull/11406)
* [[`d61a511728`](https://github.com/nodejs/node/commit/d61a511728)] - **(SEMVER-MAJOR)** **lib**: refactor internal/linkedlist (James M Snell) [#11406](https://github.com/nodejs/node/pull/11406)
* [[`2ba4eeadbb`](https://github.com/nodejs/node/commit/2ba4eeadbb)] - **(SEMVER-MAJOR)** **lib**: remove simd support from util.format() (Ben Noordhuis) [#11346](https://github.com/nodejs/node/pull/11346)
* [[`dfdd911e17`](https://github.com/nodejs/node/commit/dfdd911e17)] - **(SEMVER-MAJOR)** **lib**: deprecate node --debug at runtime (Josh Gavant) [#10970](https://github.com/nodejs/node/pull/10970)
* [[`5de3cf099c`](https://github.com/nodejs/node/commit/5de3cf099c)] - **(SEMVER-MAJOR)** **lib**: add static identifier codes for all deprecations (James M Snell) [#10116](https://github.com/nodejs/node/pull/10116)
* [[`4775942957`](https://github.com/nodejs/node/commit/4775942957)] - **(SEMVER-MAJOR)** **lib, test**: fix server.listen error message (Joyee Cheung) [#11693](https://github.com/nodejs/node/pull/11693)
* [[`caf9ae748b`](https://github.com/nodejs/node/commit/caf9ae748b)] - **(SEMVER-MAJOR)** **lib,src**: make constants not inherit from Object (Sakthipriyan Vairamani (thefourtheye)) [#10458](https://github.com/nodejs/node/pull/10458)
* [[`e0b076a949`](https://github.com/nodejs/node/commit/e0b076a949)] - **(SEMVER-MAJOR)** **lib,src,test**: update --debug/debug-brk comments (Ben Noordhuis) [#12495](https://github.com/nodejs/node/pull/12495)

* [[`5b63fabfd8`](https://github.com/nodejs/node/commit/5b63fabfd8)] - **(SEMVER-MAJOR)** **src**: update NODE_MODULE_VERSION to 55 (Michaël Zasso) [#12784](https://github.com/nodejs/node/pull/12784)
* [[`a16b570f8c`](https://github.com/nodejs/node/commit/a16b570f8c)] - **(SEMVER-MAJOR)** **src**: add --pending-deprecation and NODE_PENDING_DEPRECATION (James M Snell) [#11968](https://github.com/nodejs/node/pull/11968)
* [[`faa447b256`](https://github.com/nodejs/node/commit/faa447b256)] - **(SEMVER-MAJOR)** **src**: allow ArrayBufferView as instance of Buffer (Timothy Gu) [#12223](https://github.com/nodejs/node/pull/12223)
* [[`47f8f7462f`](https://github.com/nodejs/node/commit/47f8f7462f)] - **(SEMVER-MAJOR)** **src**: remove support for --debug (Jan Krems) [#12197](https://github.com/nodejs/node/pull/12197)
* [[`a5f91ab230`](https://github.com/nodejs/node/commit/a5f91ab230)] - **(SEMVER-MAJOR)** **src**: throw when -c and -e are used simultaneously (Teddy Katz) [#11689](https://github.com/nodejs/node/pull/11689)
* [[`8a7db9d4b5`](https://github.com/nodejs/node/commit/8a7db9d4b5)] - **(SEMVER-MAJOR)** **src**: add --use-bundled-ca --use-openssl-ca check (Daniel Bevenius) [#12087](https://github.com/nodejs/node/pull/12087)
* [[`ed12ea371c`](https://github.com/nodejs/node/commit/ed12ea371c)] - **(SEMVER-MAJOR)** **src**: update inspector code to match upstream API (Michaël Zasso) [#11752](https://github.com/nodejs/node/pull/11752)
* [[`89d8dc9afd`](https://github.com/nodejs/node/commit/89d8dc9afd)] - **(SEMVER-MAJOR)** **src**: update NODE_MODULE_VERSION to 54 (Michaël Zasso) [#11752](https://github.com/nodejs/node/pull/11752)
* [[`be98f26917`](https://github.com/nodejs/node/commit/be98f26917)] - **(SEMVER-MAJOR)** **src**: exclude node_root_certs when use-def-ca-store (Daniel Bevenius) [#11939](https://github.com/nodejs/node/pull/11939)
* [[`1125c8a814`](https://github.com/nodejs/node/commit/1125c8a814)] - **(SEMVER-MAJOR)** **src**: fix typos in node_lttng_provider.h (Benjamin Fleischer) [#11723](https://github.com/nodejs/node/pull/11723)
* [[`aae8f683b4`](https://github.com/nodejs/node/commit/aae8f683b4)] - **(SEMVER-MAJOR)** **src**: update NODE_MODULE_VERSION to 53 (Michaël Zasso) [#10992](https://github.com/nodejs/node/pull/10992)
* [[`91ab09fe2a`](https://github.com/nodejs/node/commit/91ab09fe2a)] - **(SEMVER-MAJOR)** **src**: update NODE_MODULE_VERSION to 52 (Michaël Zasso) [#9618](https://github.com/nodejs/node/pull/9618)
* [[`334be0feba`](https://github.com/nodejs/node/commit/334be0feba)] - **(SEMVER-MAJOR)** **src**: fix space for module version mismatch error (Yann Pringault) [#10606](https://github.com/nodejs/node/pull/10606)
* [[`45c9ca7fd4`](https://github.com/nodejs/node/commit/45c9ca7fd4)] - **(SEMVER-MAJOR)** **src**: remove redundant spawn/spawnSync type checks (cjihrig) [#8312](https://github.com/nodejs/node/pull/8312)
* [[`b374ee8c3d`](https://github.com/nodejs/node/commit/b374ee8c3d)] - **(SEMVER-MAJOR)** **src**: add handle check to spawn_sync (cjihrig) [#8312](https://github.com/nodejs/node/pull/8312)
* [[`3295a7feba`](https://github.com/nodejs/node/commit/3295a7feba)] - **(SEMVER-MAJOR)** **src**: allow getting Symbols on process.env (Anna Henningsen) [#9631](https://github.com/nodejs/node/pull/9631)
* [[`1aa595e5bd`](https://github.com/nodejs/node/commit/1aa595e5bd)] - **(SEMVER-MAJOR)** **src**: throw on process.env symbol usage (cjihrig) [#9446](https://github.com/nodejs/node/pull/9446)
* [[`a235ccd168`](https://github.com/nodejs/node/commit/a235ccd168)] - **(SEMVER-MAJOR)** **src,test**: debug is now an alias for inspect (Ali Ijaz Sheikh) [#11441](https://github.com/nodejs/node/pull/11441)

* [[`23fc082409`](https://github.com/nodejs/node/commit/23fc082409)] - **(SEMVER-MAJOR)** **test**: remove extra console output from test-os.js (James M Snell) [#12654](https://github.com/nodejs/node/pull/12654)
* [[`59c6230861`](https://github.com/nodejs/node/commit/59c6230861)] - **(SEMVER-MAJOR)** **test**: cleanup test-child-process-constructor.js (cjihrig) [#12348](https://github.com/nodejs/node/pull/12348)
* [[`06c29a66d4`](https://github.com/nodejs/node/commit/06c29a66d4)] - **(SEMVER-MAJOR)** **test**: remove common.fail() (Rich Trott) [#12293](https://github.com/nodejs/node/pull/12293)
* [[`0c539faac3`](https://github.com/nodejs/node/commit/0c539faac3)] - **(SEMVER-MAJOR)** **test**: add common.getArrayBufferViews(buf) (Timothy Gu) [#12223](https://github.com/nodejs/node/pull/12223)
* [[`c5d1851ac4`](https://github.com/nodejs/node/commit/c5d1851ac4)] - **(SEMVER-MAJOR)** **test**: drop v5.x-specific code path from simd test (Ben Noordhuis) [#11346](https://github.com/nodejs/node/pull/11346)
* [[`c2c6ae52ea`](https://github.com/nodejs/node/commit/c2c6ae52ea)] - **(SEMVER-MAJOR)** **test**: move test-vm-function-redefinition to parallel (Franziska Hinkelmann) [#9618](https://github.com/nodejs/node/pull/9618)
* [[`5b30c4f24d`](https://github.com/nodejs/node/commit/5b30c4f24d)] - **(SEMVER-MAJOR)** **test**: refactor test-child-process-spawnsync-maxbuf (cjihrig) [#10769](https://github.com/nodejs/node/pull/10769)

* [[`994617370e`](https://github.com/nodejs/node/commit/994617370e)] - **(SEMVER-MAJOR)** **tools**: update test-npm.sh paths (Kat Marchán) [#12936](https://github.com/nodejs/node/pull/12936)
* [[`6f202ef857`](https://github.com/nodejs/node/commit/6f202ef857)] - **(SEMVER-MAJOR)** **tools**: remove assert.fail() lint rule (Rich Trott) [#12293](https://github.com/nodejs/node/pull/12293)
* [[`615789b723`](https://github.com/nodejs/node/commit/615789b723)] - **(SEMVER-MAJOR)** **tools**: enable ES2017 syntax support in ESLint (Michaël Zasso) [#11210](https://github.com/nodejs/node/pull/11210)
