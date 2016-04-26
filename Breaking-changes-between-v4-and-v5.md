# API changes between v4 and v5

### When editing this page please be as detailed as possible.

For older breaking changes, please see our [v0.10 to v4](https://github.com/nodejs/node/wiki/API-changes-between-v0.10-and-v4) page.

## By Subsystem

### console

[[Docs](https://nodejs.org/dist/latest-v5.x/docs/api/buffer.html)]

- [`console.time()`](https://nodejs.org/dist/latest-v5.x/docs/api/console.html#console_console_time_label) now logs with sub-millisecond accuracy.
  - Refs: [`419f7d4`](https://github.com/nodejs/node/commit/419f7d472687ba65715b593a2660f12b84ed0ec8), [#3166](https://github.com/nodejs/node/pull/3166)

### repl

[[Docs](https://nodejs.org/dist/latest-v5.x/docs/api/buffer.html)]

- The repl's [`'close'`](https://nodejs.org/dist/latest-v5.x/docs/api/readline.html#readline_event_close) event now waits for the `'flushHistory'` event if the repl is set to write history to disk while closing.
  - For consistency, the `'close'` event is now always asynchronous in the repl only.
  - Refs: [`ecab7a6`](https://github.com/nodejs/node/commit/ecab7a6bee6857a07855bcbf749c75c68502ab49), [#3435](https://github.com/nodejs/node/pull/3435)

### tls

[[Docs](https://nodejs.org/dist/latest-v5.x/docs/api/buffer.html)]

- The minimum key length for [`tls.connect()`](https://nodejs.org/dist/latest-v5.x/docs/api/tls.html#tls_tls_connect_options_callback)'s `dhparam` option is now 1024 bits.
  - This a security consideration to prevent logjam attacks.
  - Refs: [`0140e1b`](https://github.com/nodejs/node/commit/0140e1b5e39342f87133f7f42e9b49a702f69b39), [#1831](https://github.com/nodejs/node/pull/1831)

### util

[[Docs](https://nodejs.org/dist/latest-v5.x/docs/api/buffer.html)]

- `util.p()` was deprecated for years, and has been removed.
  - It had been deprecated since v0.1.96: [`022c083`](https://github.com/nodejs/node/commit/022c0838480ddec334e85dd8a8ca7d376eb26d95).
  - Refs: [`8b4adb2`](https://github.com/nodejs/node/commit/8b4adb267b9320c78e1508b1c973efd256e15b21), [#3432](https://github.com/nodejs/node/pull/3432)

### zlib

[[Docs](https://nodejs.org/dist/latest-v5.x/docs/api/buffer.html)]

- Decompression now throws on truncated input, such as if there is an unexpected end-of-file.
  - Refs: [`0fc0902`](https://github.com/nodejs/node/commit/0fc0902c21c9c9ed180e575f34b9b39366d57685), [#2595](https://github.com/nodejs/node/pull/2595)

## Native Modules (Addons)

- The `NODE_MODULE_VERSION` is now `47`.

## Dependencies

### npm
[[Repo](https://github.com/npm/npm)]

[`2.14.7 --> 3.3.6`](https://github.com/nodejs/node/commits/v5.x/deps/npm)

This is a major version bump for npm and it has seen a significant amount of change. Please see the [npm v3.0.0 release notes](https://github.com/npm/npm/blob/master/CHANGELOG.md#v300-2015-06-25).

Note that npm minor and patch versions are pulled into io.js and node at an almost-weekly rate.

### v8
[`v4.5.103.35 --> v4.6.85.x`](https://github.com/nodejs/node/commits/v5.x/deps/v8) + floating patches
