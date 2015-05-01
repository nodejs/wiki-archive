# Breaking Changes Between io.js Versions

## 2.0.0 from 1.x

### V8 Upgrade

The upgrade from V8 4.1 to 4.2 will require a recompile of all native add-ons. (The API surface area has not changed significantly, so most add-ons will "just work" after a recompile.)

### Parsed URL Representation Change

Parsed URL objects returned by the `url` module no longer have own data properties, but instead accessor properties on their prototype. This means that e.g. `parsedUrl.hasOwnProperty("href")` will now return false, and common object-copying methods (like Underscore/Lo-Dash's `_.extend` or ES2015's `Object.assign`) will not copy over the URL properties.

Additionally, assignments to various properties will now ensure that the others still stay consistent. For example, setting `parsedUrl.hostname` will update `parsedUrl.href`.

To get back an object with the appropriate own data properties, use `parsedUrl.toJSON()`.

### Consistent Cross-platform `os.tmpdir()` Behavior

`os.tmpdir()` has been changed to never return a trailing slash regardless of the host platform.

Refs: [#747](https://github.com/iojs/io.js/pull/747) / [bb97b70](https://github.com/iojs/io.js/commit/bb97b70eb709b0e0470a5164b3722c292859618a)