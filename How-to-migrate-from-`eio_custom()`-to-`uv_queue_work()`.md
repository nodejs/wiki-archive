For native modules that wanted to utilize the thread pool, the original function to do this was `eio_custom()`, exposed by `libeio`. You would have code that looked _something_ like this:

``` c++
/** THIS IS THE OLD API, DON'T COPY THIS, SEE BELOW!! **/

/* the "do work" callback; called on the thread pool */
int doing_work (eio_req *req) {
  /* Something computationally expensive here */
  req->rtn = 1 + 1;
  return 0;
}

/* the "after work" callback; called on the main thread */
int after_doing_work (eio_req *req) {
  HandleScope scope;

  my_struct *m = (my_struct *)req->data;

  Handle<Value> argv[1];
  argv[0] = Integer::New(r->rtn);

  TryCatch try_catch;
  m->callback->Call(Context::GetCurrent()->Global(), 1, argv);
  if (try_catch.HasCaught())
    FatalException(try_catch);

  // cleanup
  m->callback.Dispose();
  delete m;
  return 0;
}

/* the JS entry point */
Handle<Value> start_doing_work (const Arguments& args) {
  HandleScope scope;

  my_struct *m = new my_struct;
  m->callback = Persistent<Function>::New(Local<Function>::Cast(args[0]));

  eio_custom(doing_work, EIO_PRI_DEFAULT, after_doing_work, m);

  return Undefined();
}
```

Now, starting with node `v0.5.6`, there is a new preferred method of utilizing the thread pool for CPU-intensive tasks: `uv_queue_work`, exposed by `libuv`. The API is similar but slightly different:

``` c++
/** THIS IS THE CURRENT API. COPY THIS!!! **/

/* the "do work" callback; called on the thread pool */
void doing_work (uv_work_t *req) {
  /* Something computationally expensive here */
  req->rtn = 1 + 1;
}

/* the "after work" callback; called on the main thread */
void after_doing_work (uv_work_t *req) {
  HandleScope scope;

  my_struct *m = (my_struct *)req->data;

  Handle<Value> argv[1];
  argv[0] = Integer::New(r->rtn);

  TryCatch try_catch;
  m->callback->Call(Context::GetCurrent()->Global(), 1, argv);
  if (try_catch.HasCaught())
    FatalException(try_catch);

  // cleanup
  m->callback.Dispose();
  delete m;
  delete req;
}

/* the JS entry point */
Handle<Value> start_doing_work (const Arguments& args) {
  HandleScope scope;

  uv_work_t *req = new uv_work_t;
  my_struct *m = new my_struct;
  req->data = m;
  m->callback = Persistent<Function>::New(Local<Function>::Cast(args[0]));

  uv_queue_work(uv_default_loop(), req, doing_work, after_doing_work);

  return Undefined();
}
```

Rundown:

  * Call `uv_queue_work()` instead of `eio_custom()` to initiate async work on the thread pool.
  * The "do work" and "after work" callback functions now accept a `uv_work_t *` pointer as their arguments, instead of a `eio_req * `pointer.
  * You must manually create the `uv_work_t` instance, and pass it to `uv_queue_work()`. It's not done for you like with `libeio`.
  * You can attach a custom `void *` to the `data` field of the `uv_work_t` struct.
  * Don't forget to `free()` or `delete` the `uv_work_t` struct at the end of the "after work" callback function.

If you desire backwards-compatibility with node <= `v0.4.x`, then check out this [shim header file](https://gist.github.com/1368935), which will conditionally use `uv_queue_work()` when available or fall back to using `eio_custom()` when it's not.