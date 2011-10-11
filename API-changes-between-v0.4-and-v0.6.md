Changed:

 * The `customFds` option to the `child_process.spawn` method is deprecated. 
 * You can no longer send a file descriptor through a unix pipe. Instead you can send a handle via `child_process.fork`.


Added:

 * zlib module
 * `child_process.fork`