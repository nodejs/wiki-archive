# Breaking Changes Between io.js Versions

## 2.0.0 from 1.x

### V8 Upgrade

The upgrade from V8 4.1 to 4.2 will require a recompile of all native add-ons. (The API surface area has not changed significantly, so most add-ons will "just work" after a recompile.)

### Consistent Cross-platform `os.tmpdir()` Behavior

`os.tmpdir()` has been changed to never return a trailing slash regardless of the host platform.

Refs: [#747](https://github.com/iojs/io.js/pull/747) / [bb97b70](https://github.com/iojs/io.js/commit/bb97b70eb709b0e0470a5164b3722c292859618a)