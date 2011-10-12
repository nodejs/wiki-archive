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
 * `child_process.fork`
 * `fs.watch`
 * `assert(val)` as a shorthand for `assert.ok(val)`
 * `process.arch`, `os.arch()`