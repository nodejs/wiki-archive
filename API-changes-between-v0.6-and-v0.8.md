When editing this page please be as detailed as possible. Examples are encouraged!

## Deprecated:
  * `http.Client()`
  * `path.{exists,existsSync}` was moved to `fs.{exists,existsSync}`

## Removed:
  * `waf` build system - `node.js` will be using `gyp` now
  * `sys` throws

## Changed:

 * `cluster`
   * `env` optional argument to `cluster.fork()`.

 * `http`
   * `http.Server` emits `'connect'` event instead of `'upgrade'` when the CONNECT method is requested.
   * `http.ClientRequest` emits `'connect'` event instead of `'request'` when the CONNECT method is responded.

## Added:

 * `cluster`
   * `'fork'`, `'online'`, `'listening'` events.
   * `Worker` object which is provided from `cluster.workers` (in the master) or `cluster.worker` (in the worker).

 * `fs`
   * `fs.appendFile()`, `fs.appendFileSync()`.

 * `https`
   * `rejectUnauthorized` option to `https.request()` and `https.get()`.

 * `net`
   * `net.connect(options, [connectionListener)`.

 * `process`
   * `process.abort()`

 * `querystring`
   * `querystring.parse(str, [sep], [eq], [options])`

 * `tls`
   * `tls.connect(options, [secureConnectionListener])`
   * `rejectUnauthorized` and `socket` options to `tls.connect()`.

 * `zlib`
   * `dictionary` option.