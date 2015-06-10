# Breaking Changes Between io.js Versions

## 3.0.0 from 2.x

### `Buffer.concat` changes

Through 2.x, if `Buffer.concat` is invoked with a single element array, then it will simply return the first element from it. But, starting from 3.0.0, irrespective of the number of elements in the array, a new `Buffer` object will be created and returned.

Refs: [#1891](https://github.com/nodejs/io.js/issues/1891), [#1937](https://github.com/nodejs/io.js/pull/1937), [`f4f16bf`](https://github.com/nodejs/io.js/commit/f4f16bf03980df4d4366697d40e80da805a38bf8)

### dgram send() error changes

Through 2.x, the the dgram `socket.send()` method emitted errors when a DNS lookup failed, even when a callback was provided. Starting from 3.0.0, if a callback is provided, no error event will be emitted.

Refs: [#1796](https://github.com/nodejs/io.js/pull/1796)

### http server socket finish event stuff???

Somehow this is marked as semver-major?? https://github.com/nodejs/io.js/commit/6020d2a2fb75de6766c807864fa8f1c0fba88ec9 https://github.com/nodejs/io.js/pull/1411 Someone help.

## 2.0.0 from 1.x

### V8 Upgrade

The upgrade from V8 4.1 to 4.2 will require a recompile of all native add-ons. (The API surface area has not changed significantly, so most add-ons will "just work" after a recompile.)

### Consistent Cross-platform `os.tmpdir()` Behavior

`os.tmpdir()` has been changed to never return a trailing slash regardless of the host platform.

Refs: [`#747`](https://github.com/iojs/io.js/pull/747) / [`bb97b70`](https://github.com/iojs/io.js/commit/bb97b70eb709b0e0470a5164b3722c292859618a)