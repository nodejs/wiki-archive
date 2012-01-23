When editing this page please be as detailed as possible. Examples are encouraged!

## Deprecated:
  * `http.Client()`
  * `path.{exists,existsSync}` was moved to `fs.{exists,existsSync}`

## Removed:
  * `waf` build system - `node.js` will be using `gyp` now
  * `sys` throws

## Changed:

 * `cluster`
   * `cluster.fork()` no longer return a `child_process.fork()` object use `cluster.fork().process` to get the object.
   * the `exit` event is renamed to `death`.
   * the `kill` method is renamed to `destroy`.
   * the `CLUSTER_WORKER_ID` env is now called `CLUSTER_UNIQUE_ID`, but you should not have used that any way.

 * `http`
   * `http.Server` emits `'connect'` event instead of `'upgrade'` when the CONNECT method is requested.
   * `http.ClientRequest` emits `'connect'` event instead of `'request'` when the CONNECT method is responded.

## Added:

 * `cluster`
   * `'fork'`, `'online'`, `'listening'` events.
   * `Worker` object which is provided from `cluster.workers` (in the master) or `cluster.worker` (in the worker).
   * `env` optional argument to `cluster.fork()`.

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