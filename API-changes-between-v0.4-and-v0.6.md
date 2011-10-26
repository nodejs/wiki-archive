When editing this page please be as detailed as possible. Examples are encouraged!

## Changed:
 * Cygwin build is no longer supported. Use native windows builds instead.
 * `Buffer`
   * The `length` optional third parameter was added to `Buffer.write()`
 * `child_process`
   * The `customFds` option to the `child_process.spawn` method is deprecated. Only the values -1, 0, 1, 2 will work with `customFds` now and only if those are TTY file descriptors. You can however use the `stdinStream`, `stdoutStream`, and `stderrStream` options to achieve similar functionality.
   * You can no longer send a file descriptor through a unix pipe. Instead you can send a handle via `child_process.fork`.
 * `dgram`
   * The `'unix_dgram'` type to the `dgram.createSocket()` is no longer supported. The Unix domain datagram is *not* the Internet domain datagram (UDP) which stays supported.
 * `dns`
   * `dns.lookup` now uses `getaddrinfo` in a thread pool instead of c-ares. The rest of the methods in the DNS module still use c-ares. Using the system resolver for common look ups is useful because it hooks into system MDNS lookups and /etc/host files and nsswitch, etc. Perviously when using `dns.lookup` on invalid domain names like `"****"` the command returned an `EBADNAME` error. `getaddrinfo` does not differentiate between invalid domains and `ENOTFOUND` (AKA `NXDOMAIN`). Therefore `dns.lookup` now returns `ENOTFOUND` when given malformated domain names like `"*****"`.
 * `events`
   * C++ `node::EventEmitter` has been removed. Instead use `node::MakeCallback()`
   * `EventEmitter.removeAllListeners()` allows to remove all listeners at once.
 * `fs`
   * `fs.symlink` takes an optional `mode` argument, which can either be 'dir' or 'file'.  The default is 'file'.  This argument is only needed for Windows (it's ignored on other platforms).
   * `fs.watchFile` is replaced by `fs.watch`
 * `http`
   * `http.request()` and `http.get()` use `Connection: Keep-Alive` by default.
   * `http.Agent.appendMessage` was removed.
   * `http.getAgent()` was removed. Use `http.globalAgent` instead.
   * Not `httpAgent` but `http.ClientRequest` emits `'upgrade'` event.
 * `https`
   * `https.request()` and `https.get()` with default `Agent` ignore `key`, `cert` and `ca` options. Use custom `Agent`.
 * Module system
   * The `require.paths` have been removed (use `NODE_PATH` environment variable instead).
 * `net`
   * `net.Server.listenFD()` was no longer supported.
 * `process`
   * `process.ENV` was removed. Use `process.env` instead.
   * `process.ARGV` was removed. Use `process.argv` instead.
   * `process.binding('stdio')` was removed. This was a private API. You shouldn't have been using it in the 
   first place.
   * `process.binding('net')` was removed.
   * `process.stdin`, `process.stdout` and `process.stderr` are getters now.
     You can override it (if you really want to) like this:

     ```javascript
     process.__defineGetter__('stdout', function() {
       return your_object;
     });
     ```

   * `process.memoryUsage().vsize` was removed. You don't need it.
 * V8 (v3.1 to v3.7)
   * `RegExp` was no longer a `Function`. Use `RegExp.exec()` instead.

## Added:

 * `assert`
   * `assert(val)` as a shorthand for `assert.ok(val)`
 * `Buffer`
   * `'hex'` encoding.
   * `Buffer.readInt8()/readInt16BE()/readInt16LE()/readInt32LE()/readInt32BE()`
   * `Buffer.readUInt8()/readUInt16BE()/readUInt16LE()/rreadUInt32LE()/readUInt32BE()`
   * `Buffer.readFloatBE()/readFloadLE()/readDoubleBE()/readDoubleLE()`
   * `Buffer.writeInt8()/writeInt16BE()/writeInt16LE()/write32BE()/write32LE()`
   * `Buffer.writeUInt8()/writeUInt16BE()/writeUInt16LE()/writeU32BE()/writeU32LE()`
   * `Buffer.writeFloatBE()/writeFloatLE()/writeDoubleBE()/writeDoubleLE()`
   * `Buffer.fill()`
   * Typed Arrays
 * `child_process`
   * `child_process.fork()`
 * `cluster`
   * `node cluster` see https://github.com/joyent/node/blob/d2698d182271c77bc5bca44a9cee625d9372301f/doc/api/cluster.markdown
 * `crypto`
   * `crypto.createDiffieHellman()`, `crypto.pbkdf2()`, `crypto.randomBytes()`
 * `fs`
   * `fs.utimes()/utimesSync()`, `fs.futimes()/futimesSync()`
   * `start` option to `fs.createReadStream()` and `fs.createWriteStream()`.
 * `http`
   * `http.ClientRequest.setTimeout()/setNoDelay()/setSocketKeepAlive()`
 * Module system
   * `.json` module.
   * `module.require()`
 * `net`
   * `net.connect()`
   * `net.Socket.remotePort`, `bytesRead`, `bytesWrite`
 * `os`
   * `os.arch()`, `os.platform()`, `os.uptime()`, `os.getNetworkInterfaces()`
 * `path`
   * `path.relative()`
 * `process`
   * `process.arch`, `process.uptime()`
 * `tls`
   * `tls.CleartextStream.address()`, `remoteAddress`, `remotePort`.
   * `tls.CryptoStream.getSession()` and `session` option to `tls.connect()`.
   * `tls.Server` supports [NPN (Next Protocol Negotitation) and SNI (Server Name Indication)](http://nodejs.org/docs/latest/api/tls.html#nPN_and_SNI).
 * `util`
   * `util.format()`, `util.isArray()`, `util.isRegExp()`, `uitl.isDate()`, `util.isError()`.
 * `zlib` module http://nodejs.org/docs/latest/api/zlib.html