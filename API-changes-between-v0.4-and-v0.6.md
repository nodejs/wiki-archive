When editing this page please be as detailed as possible. Examples are encouraged!

## Changed:

 * The `customFds` option to the `child_process.spawn` method is deprecated. Only the values -1, 0, 1, 2 will work with `customFds` now and only if those are TTY file descriptors. You can however use the `stdinStream`, `stdoutStream`, and `stderrStream` options to achieve similar functionality.
 * You can no longer send a file descriptor through a unix pipe. Instead you can send a handle via `child_process.fork`.
 * `fs.symlink` takes an optional `mode` argument, which can either be 'dir' or 'file'.  The default is 'file'.  This argument is only needed for Windows (it's ignored on other platforms).
 * `http.Agent.appendMessage` was removed.
 * `net.Server.listenFD()` is no longer supported.
 * The `require.paths` have been removed (use `NODE_PATH` environment variable instead).
 * C++ `node::EventEmitter` has been removed. Instead use `node::MakeCallback()`
 * `process.ENV` was removed. Use `process.env` instead.

## Added:

 * zlib module http://nodejs.org/docs/v0.5.9/api/zlib.html
 * `Buffer` supports `'hex'` encoding.
 * `Buffer.readInt8()/readInt16BE()/readInt16LE()/readInt32LE()/readInt32BE()`
 * `Buffer.readUInt8()/readUInt16BE()/readUInt16LE()/rreadUInt32LE()/readUInt32BE()`
 * `Buffer.readFloatBE()/readFloadLE()/readDoubleBE()/readDoubleLE()`
 * `Buffer.writeInt8()/writeInt16BE()/writeInt16LE()/write32BE()/write32LE()`
 * `Buffer.writeUInt8()/writeUInt16BE()/writeUInt16LE()/writeU32BE()/writeU32LE()`
 * `Buffer.writeFloatBE()/writeFloatLE()/writeDoubleBE()/writeDoubleLE()`
 * `Buffer.fill()`
 * `child_process.fork`
 * `fs.watch`
 * `assert(val)` as a shorthand for `assert.ok(val)`
 * `process.arch`, `os.arch()`
 * `util.format()`