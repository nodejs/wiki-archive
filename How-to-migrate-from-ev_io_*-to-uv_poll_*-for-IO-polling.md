For native modules that wanted to watch for IO events on file descriptors, the original functions to do this was `ev_io_*()`, exposed by `libev`. You would have code that looked _something_ like this:

``` c++
/** THIS IS THE OLD API, DON'T COPY THIS, SEE BELOW!! **/

struct my_struct {
  Persistent<Function> callback;
  int fd;
};

/* the "on fd IO event" callback; called on the main thread */
void on_fd_event (EV_P_ ev_io *io_watcher, int events) {
  HandleScope scope;

  my_struct *m = (my_struct *) io_watcher->data;

  // stop watcher
  ev_io_stop(EV_DEFAULT_UC_ io_watcher);
  delete io_watcher;

  /* Some code here that works with `fd` (and closes it if needed) and produces `Handle<Value> result` */

  Handle<Value> argv[1];
  argv[0] = Integer::New(result);

  TryCatch try_catch;
  m->callback->Call(Context::GetCurrent()->Global(), 1, argv);

  // cleanup
  m->callback.Dispose();
  delete m;

  if (try_catch.HasCaught())
    FatalException(try_catch);
}

/* the JS entry point */
Handle<Value> start_fd_watch (const Arguments& args) {
  HandleScope scope;

  /* Some code here that opens `fd` and do something */

  my_struct *m = new my_struct;
  m->fd = fd;
  m->callback = Persistent<Function>::New(Local<Function>::Cast(args[0]));

  // create IO watcher and start watching
  ev_io* io_watcher = new ev_io;
  io_watcher->data = m;
  ev_init(io_watcher, on_fd_event);
  ev_io_set(io_watcher, fd, EV_READ /* or other flags */);
  ev_io_start(EV_DEFAULT_UC_ io_watcher);

  return Undefined();
}
```

Now, there is a new preferred method to poll IO events: `uv_poll_*()`, exposed by `libuv`. The API is similar but slightly different:

``` c++
/** THIS IS THE CURRENT API. COPY THIS!!! **/

struct my_struct {
  Persistent<Function> callback;
  int fd;
};

/* the "on fd IO event" callback; called on the main thread */
void on_fd_event (uv_poll_t* handle, int status, int events) {
  HandleScope scope;

  my_struct *m = (my_struct *) handle->data;

  // stop watcher
  uv_poll_stop(handle);
  delete handle;

  /* Some code here that works with `fd` (and closes it if needed) and produces `Handle<Value> result` */

  Handle<Value> argv[1];
  argv[0] = Integer::New(result);

  TryCatch try_catch;
  m->callback->Call(Context::GetCurrent()->Global(), 1, argv);

  // cleanup
  m->callback.Dispose();
  delete m;

  if (try_catch.HasCaught())
    FatalException(try_catch);
}

/* the JS entry point */
Handle<Value> start_fd_watch (const Arguments& args) {
  HandleScope scope;

  /* Some code here that opens `fd` and do something */

  my_struct *m = new my_struct;
  m->fd = fd;
  m->callback = Persistent<Function>::New(Local<Function>::Cast(args[0]));

  // create poll handle and start polling
  uv_poll_t* handle = new uv_poll_t;
  handle->data = m;
  uv_poll_init(uv_default_loop(), handle, fd);
  uv_poll_start(handle, EV_READ /* or other flags */, on_fd_event);

  return Undefined();
}
```

Rundown:

  * Call `uv_queue_work()` instead of `eio_custom()` to initiate async work on the thread pool.
  * The "after work" callback functions now accept a `uv_work_t *` pointer as their arguments, instead of a `eio_req * `pointer.
  * You must manually create the `uv_work_t` instance, and pass it to `uv_queue_work()`. It's not done for you like with `libeio`.
  * You can attach a custom `void *` to the `data` field of the `uv_work_t` struct.
  * Don't forget to `free()` or `delete` the `uv_work_t` struct at the end of the "after work" callback function.