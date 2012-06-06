When editing this page please be as detailed as possible. Examples are encouraged!

## Deprecated:
  * `http.Client()`
  * `path.{exists,existsSync}` was moved to `fs.{exists,existsSync}`
  * `tty.setRawMode(mode)` was moved to `tty.ReadStream#setRawMode()` (i.e. `process.stdin.setRawMode()`) 

## Removed:
  * `waf` build system - `node.js` will be using `gyp` now
    * If you are a native module author, migrate to [`node-gyp`](https://github.com/TooTallNate/node-gyp) ASAP!
  * `sys` throws

## Changed:

 * `process`
   * `process.stdin.on('keypress')` will not be emitted by default, as it's an internal API.
 * `cluster`
   * `cluster.fork()` no longer return a `child_process.fork()` object use `cluster.fork().process` to get the object.
   * the `'death'` event on the `cluster` object is renamed to `'exit'`.
   * the `kill()` method is renamed to `destroy()`.
   * the `CLUSTER_WORKER_ID` env is now called `CLUSTER_UNIQUE_ID`, but you should not have used that anyway.
   * workers do now kill them self when they accidentally losses there connection with the master.

 * `http`
   * `http.Server` emits `'connect'` event instead of `'upgrade'` when the CONNECT method is requested.
   * `http.ServerResponse` sends `Date:` header by default. You can disable it by setting `response.sendDate` to `false`.
   * `http.ClientRequest` emits `'connect'` event instead of `'request'` when the CONNECT method is responded.

 * `child_process`
   * `arguments` and `options` arguments of `child_process.fork()` became an option.
   * the 'exit' event is now emitted right after the child process exits. It no longer waits for all stdio pipes to be closed.
   * the 'close' event was added that has is emitted after the child has exited *and* all the stdio pipes are closed.

 * `readline`
   * `arguments` of `rl.createInterface` became an option as `rl.createInterface(options)` but still took an old style as rl.createInterface(input, output, completer)

 * `url`
   * `url.parse()` now parses IPv6 addresses.

 * `fs`
   * `path.exists()` and `path.existsSync()` has moved to `fs.exists()` and `fs.existsSync()`.

 * `console`
   * `console.timeEnd` now throws when there's no such label

## Added:

 * `buffer`
   * `'utf16le'` encoding.

 * `child_process`
   * `silent` option to `child_process.fork()` - `stdout` and `stderr` won't be shared with parent.
   * `.disconnect()` when using `child_process.fork()` this will allow the child to die graceful.

 * `cluster`
   * `'fork'`, `'online'`, `'listening'`, and `'setup'` events.
   * `Worker` object which is provided from `cluster.workers` (in the master) or `cluster.worker` (in the worker).
   * `env` optional argument to `cluster.fork()`.
   * `cluster.setupMaster()` and `cluster.settings`.
   * `cluster.disconnect()` and `worker.disconnect()`.
   * `worker.uniqueID` what there before was internally known as `workerID`.
   * `worker.suicide` flag set when worker disconnect or die, indicate if this was an accidental death.

 * `crypto`
   * `crypto.getDiffieHellman()`.
   * `cipher.setAutoPadding()` and `decipher.setAutoPadding()`.
   * `ciphers` option to `crypto.createCredentials()`.

 * `domain`
   * see http://nodejs.org/docs/v0.7.9/api/domain.html

 * `fs`
   * `fs.appendFile()` and `fs.appendFileSync()`.
   * `wx`, `wx+`, `ax`, and `ax+` modes to `fs.open()` and `fs.openSync()`.

 * `http`
   * callback optional argument to `server.close()`.
   * `sendDate` property to `http.ServerResponse`.

 * `https`
   * `ciphers`, `rejectUnauthorized` option to `https.request()` and `https.get()`.

 * `net`
   * `net.connect(options, [connectionListener)`.
   * callback optional argument to `server.close()`.

 * `process`
   * `process.abort()`.
   * `process.hrtime()`, hi-res timer with up to nanosecond granularity.

 * `querystring`
   * `querystring.parse(str, [sep], [eq], [options])`.

 * `stream`
   * `'utf16le'` and `'ucs2'` encoding to `setEncoding()`.

 * `tls`
   * `tls.CLIENT_RENEG_LIMIT` and `tls.CLIENT_RENEG_WINDOW` to [mitigate session renegotiation attacks](http://nodejs.org/docs/latest/api/tls.html#client_initiated_renegotiation_attack_mitigation)
   * `tls.connect(options, [secureConnectionListener])`.
   * `ciphers`, `rejectUnauthorized` and `socket` options to `tls.connect()`.
   * `cleartextStream.getCipher()` was added in the api doc and open to the public.

 * `zlib`
   * `dictionary` option.