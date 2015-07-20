# Breaking Changes Between io.js Versions

## 3.0.0 from 2.x

### `Buffer.concat` changes

Through 2.x, if `Buffer.concat` is invoked with a single element array, then it will simply return the first element from it. But, starting from 3.0.0, irrespective of the number of elements in the array, a new `Buffer` object will be created and returned.

Refs: [#1891](https://github.com/nodejs/io.js/issues/1891), [#1937](https://github.com/nodejs/io.js/pull/1937), [`f4f16bf`](https://github.com/nodejs/io.js/commit/f4f16bf03980df4d4366697d40e80da805a38bf8)

### dgram send() error changes

Through 2.x, the the dgram `socket.send()` method emitted errors when a DNS lookup failed, even when a callback was provided. Starting from 3.0.0, if a callback is provided, no error event will be emitted.

Refs: [#1796](https://github.com/nodejs/io.js/pull/1796)

### http server timing change

When doing HTTP pipelining of requests, the server creates new request and response objects for each incoming HTTP request on the socket. Starting from 3.0.0, these objects are now created a couple of ticks later, when we are certainly done processing the previous request. This could change the observable timing with certain HTTP server coding patterns.

Refs: [#1411](https://github.com/nodejs/io.js/pull/1411) [`6020d2`](https://github.com/nodejs/io.js/commit/6020d2a2fb75de6766c807864fa8f1c0fba88ec9)

### V8 upgrade

The upgrade from V8 4.2 to 4.3 will require a recompile of all native add-ons. The API surface area [has not changed significantly](https://docs.google.com/document/d/1g8JFi8T_oAE_7uAri7Njtig7fKaPDfotU6huOa1alds/edit), so most add-ons will "just work" after a recompile.

_However_, 4.3 introduces the new `Maybe<>` and `MaybeLocal<>` types, to fix systemic use-after-failure bugs where consumers could accidentally use empty handles. Many new APIs were introduced that return these types and take a Context argument. Add-on authors are encouraged to transition to these new maybe-style APIs as soon as possible, as V8 has deprecated the old ones and will eventually remove them.

### HTTP status codes

HTTP status codes in core (`http.STATUS_CODES`) were previously incorrect in mapping to the regulated codes. Updating the code mappings to conform to the [IANA standard](http://www.iana.org/assignments/http-status-codes/http-status-codes.xhtml) was a backwards incompatible change to consumers who depend on the text value of a header. The most significant of these changes was the text for `302`, which was previously `Moved Temporarily` but is now `Found`.

Refs: [#1470](https://github.com/nodejs/io.js/pull/1470) [`235036e`](https://github.com/nodejs/io.js/commit/235036e7faa537469c3600850bdd533c095c392a)

## 2.0.0 from 1.x

### Consistent cross-platform `os.tmpdir()` behavior

`os.tmpdir()` has been changed to never return a trailing slash regardless of the host platform.

Refs: [#747](https://github.com/iojs/io.js/pull/747) / [`bb97b70`](https://github.com/iojs/io.js/commit/bb97b70eb709b0e0470a5164b3722c292859618a)

### V8 upgrade

The upgrade from V8 4.1 to 4.2 will require a recompile of all native add-ons. (The API surface area [has not changed significantly](https://docs.google.com/document/d/1g8JFi8T_oAE_7uAri7Njtig7fKaPDfotU6huOa1alds/edit), so most add-ons will "just work" after a recompile.)
