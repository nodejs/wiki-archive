When editing this page please be as detailed as possible. Examples are encouraged!

## Changed:

 * The `customFds` option to the `child_process.spawn` method is deprecated. Only the values -1, 0, 1, 2 will work with `customFds` now and only if those are TTY file descriptors. You can however use the `stdinStream`, `stdoutStream`, and `stderrStream` options to achieve similar functionality.
 * You can no longer send a file descriptor through a unix pipe. Instead you can send a handle via `child_process.fork`.
 * The `'unix_dgram'` type to the `dgram.createSocket()` is no longer supported.
 * `fs.symlink` takes an optional `mode` argument, which can either be 'dir' or 'file'.  The default is 'file'.  This argument is only needed for Windows (it's ignored on other platforms).
 * `http.Agent.appendMessage` was removed.
 * `https.request()` and `https.get()` with default `Agent` cannot accept `key`, `cert` and `ca` options. Use custom `Agent`.
 * `net.Server.listenFD()` was no longer supported.
 * The `require.paths` have been removed (use `NODE_PATH` environment variable instead).
 * C++ `node::EventEmitter` has been removed. Instead use `node::MakeCallback()`
 * `EventEmitter.removeAllListeners()` allows to remove all listeners at once.
 * `process.ENV` was removed. Use `process.env` instead.
 * `process.ARGV` was removed. Use `process.argv` instead.
 * `process.binding('stdio')` was removed. This was a private API. You shouldn't have been using it in the first place.
 * [V8] `RegExp` was no longer a `Function`.

## Added:

 * `assert(val)` as a shorthand for `assert.ok(val)`
 * `Buffer` supports `'hex'` encoding.
 * `Buffer.readInt8()/readInt16BE()/readInt16LE()/readInt32LE()/readInt32BE()`
 * `Buffer.readUInt8()/readUInt16BE()/readUInt16LE()/rreadUInt32LE()/readUInt32BE()`
 * `Buffer.readFloatBE()/readFloadLE()/readDoubleBE()/readDoubleLE()`
 * `Buffer.writeInt8()/writeInt16BE()/writeInt16LE()/write32BE()/write32LE()`
 * `Buffer.writeUInt8()/writeUInt16BE()/writeUInt16LE()/writeU32BE()/writeU32LE()`
 * `Buffer.writeFloatBE()/writeFloatLE()/writeDoubleBE()/writeDoubleLE()`
 * Typed Arrays
 * `Buffer.fill()`
 * `child_process.fork()`
 * `crypto.createDiffieHellman()`, `crypto.pbkdf2()`, `crypto.randomBytes()`
 * `fs.utimes()/utimesSync()`, `fs.futimes()/futimesSync()`
 * `fs.watch`
 * Module system supports `.json` module.
 * `net.connect()`
 * `net.Socket.remotePort`, `bytesRead`, `bytesWrite`
 * `os.arch()`, `os.uptime()`, `os.getNetworkInterfaces()`
 * `path.relative()`
 * `process.arch`, `process.uptime()`
 * `tls.CryptoStream.getSession()` and `session` option to `tls.connect()`.
 * `tls.Server` supports NPN (Next Protocol Negotitation) and SNI (Server Name Indication).
 * `util.format()`
 * `zlib` module http://nodejs.org/docs/v0.5.9/api/zlib.html
