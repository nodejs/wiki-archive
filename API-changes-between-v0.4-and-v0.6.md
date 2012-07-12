When editing this page please be as detailed as possible. Examples are encouraged!!

## Changed:
 * Cygwin build is no longer supported. Use native windows builds instead.
 * `Buffer`
   * The `length` optional third parameter was added to `Buffer.write()`
 * `child_process`
   * The `customFds` option to the `child_process.spawn()` method is deprecated. Only the values -1, 0, 1, 2 will work with `customFds` now and only if those are TTY file descriptors. You can however use the `stdinStream`, `stdoutStream`, and `stderrStream` options to achieve similar functionality.
   * You can no longer send a file descriptor through a unix pipe. Instead you can send a handle via `child_process.fork()`.
  * The `.pid` property is not set to null when the child process exits. Listen for the `'exit'` event instead.
 * `dgram`
   * The `'unix_dgram'` type to the `dgram.createSocket()` is no longer supported. Use [bnoordhuis/node-unix-dgram](https://github.com/bnoordhuis/node-unix-dgram) instead. Note: The Unix domain datagram is *not* the Internet domain datagram (UDP) which stays supported. 
 * `dns`
   * `dns.lookup()` now uses `getaddrinfo()` in a thread pool instead of c-ares. The rest of the methods in the DNS module still use c-ares. Using the system resolver for common look ups is useful because it hooks into system MDNS lookups and /etc/host files and nsswitch, etc. Previously when using `dns.lookup` on invalid domain names like `"****"` the command returned an `EBADNAME` error. `getaddrinfo()` does not differentiate between invalid domains and `ENOTFOUND` (AKA `NXDOMAIN`). Therefore `dns.lookup` now returns `ENOTFOUND` when given malformated domain names like `"*****"`.
 * `events`
   * C++ `node::EventEmitter` has been removed. Instead use `node::MakeCallback()`
   * `EventEmitter.removeAllListeners()` allows to remove all listeners at once.
 * `fs`
   * `mode` argument of `fs.mkdir()` and `fs.mkdirSync()` became an option (defaults to `0777`).
   * `fs.symlink()` takes an optional `mode` argument, which can either be `'dir'` or `'file'`.  The default is `'file'`.  This argument is only needed for Windows (it's ignored on other platforms).
 * `http`
   * `http.request()` and `http.get()` use `Connection: Keep-Alive` by default.
   * `http.Agent.appendMessage()` was removed.
   * `http.getAgent()` was removed. Use `http.globalAgent` instead.
   * Not `http.Agent` but `http.ClientRequest` emits `'upgrade'` event.
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
   * `process.memoryUsage().vsize` was removed. You don't need it.
   * `process.stdout` and `process.stderr` are blocking when they refer to regular files or TTY file descriptors.
   * `process.stdin`, `process.stdout` and `process.stderr` are getters now.
     You can override it (if you really want to) like this:

     ```javascript
     process.__defineGetter__('stdout', function() {
       return your_object;
     });
     ```

 * `stream`
   * `stream.pipe()` doesn't emit `'pause'/'resume'` events on the source stream which doesn't implement `pause()/resume()`.

 * `tty`
   * `tty.setWindowSize()` was removed.
   * `tty.getWindowSize(fd)` was removed. Use `process.stdout.getWindowSize()` instead.

 * V8 (v3.1 to v3.6)
   * `RegExp` was no longer a `Function` (compliant with ES5). Use `RegExp.exec()` instead.
   * `Date`'s string format without timezone (e.g., `new Date('2011-06-06')`) has been based on UTC, not local timezone (compliant with ES5). Use timezone explicitly (e.g., `new Date('2011-06-06 00:00:00 +09:00')`).
   * All standard properties of `Error` have been made non-enumerable (compliant with ES5). use `util.inspect(err, true)` if you want to show it.


## Added:

 * `assert`
   * `assert(val)` as a shorthand for `assert.ok(val)`
 * `Buffer`
   * `'hex'` encoding.
   * `Buffer.readInt8()/readInt16LE()/readInt16BE()/readInt32LE()/readInt32BE()`
   * `Buffer.readUInt8()/readUInt16LE()/readUInt16BE()/readUInt32LE()/readUInt32BE()`
   * `Buffer.readFloatLE()/readFloatBE()/readDoubleLE()/readDoubleBE()`
   * `Buffer.writeInt8()/writeInt16LE()/writeInt16BE()/writeInt32LE()/writeInt32BE()`
   * `Buffer.writeUInt8()/writeUInt16LE()/writeUInt16BE()/writeUInt32LE()/writeUInt32BE()`
   * `Buffer.writeFloatLE()/writeFloatBE()/writeDoubleLE()/writeDoubleBE()`
   * `Buffer.fill()`
   * Typed Arrays
 * `child_process`
   * `child_process.fork()`
 * `cluster`
   * `node cluster` see http://nodejs.org/docs/latest/api/cluster.html
 * `crypto`
   * `crypto.createDiffieHellman()`, `crypto.pbkdf2()`, `crypto.randomBytes()`
 * `fs`
   * `fs.watch()`
   * `fs.utimes()/utimesSync()`, `fs.futimes()/futimesSync()`
   * `start` option to `fs.createReadStream()` and `fs.createWriteStream()`.
 * `http`
   * `http.ClientRequest.setTimeout()/setNoDelay()/setSocketKeepAlive()`
   * `auth` option to `http.request()`.
 * `https`
   * `passphrase` option to `https.createServer()`, `https.request()`, and `https.get()`.
 * Module system
   * `.json` module.
   * `module.require()`
 * `net`
   * `net.connect()`
   * `net.Socket.remotePort`, `bytesRead`, `bytesWritten`
 * `os`
   * `os.arch()`, `os.platform()`, `os.uptime()`, `os.networkInterfaces()`
 * `path`
   * `path.relative()`
 * `process`
   * `process.arch`, `process.uptime()`
 * `tls`
   * `passphrase` option to `tls.createServer()` and `tls.connect()`.
   * `sessionIdContext` option to `tls.createServer()`.
   * `tls.CryptoStream.getSession()` and `session` option to `tls.connect()`.
   * `tls.CleartextStream.address()`, `remoteAddress`, `remotePort`.
   * `tls.Server` supports [NPN (Next Protocol Negotiation) and SNI (Server Name Indication)](http://nodejs.org/docs/latest/api/tls.html#nPN_and_SNI).
 * `util`
   * `util.format()`, `util.isArray()`, `util.isRegExp()`, `util.isDate()`, `util.isError()`.
 * `zlib` module http://nodejs.org/docs/latest/api/zlib.html