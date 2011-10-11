Changed:

 * The `customFds` option to the `child_process.spawn` method is deprecated. 
 * You can no longer send a file descriptor through a unix pipe. Instead you can send a handle via `child_process.fork`.
 * fs.symlink takes an optional mode argument, which can either be 'dir' or 'file'.  By default mode is 'file'.  This argument is only needed for Windows (it's ignored on other platforms).

Added:

 * zlib module
 * `child_process.fork`
 * `fs.watch`
 * `assert(val)` as a shorthand for `assert.ok(val)`