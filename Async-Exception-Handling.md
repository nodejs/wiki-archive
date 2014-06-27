For excpection handling best practice is to use [Domains](http://nodejs.org/docs/latest/api/domain.html)

As of [v0.11.9](http://nodejs.org/dist/v0.11.11/docs/api/process.html#process_async_listeners) (with an API change in [0.11.12](http://nodejs.org/dist/v0.11.12/docs/api/tracing.html#tracing_async_listeners)), a lower-level hook to get at the asynchronous event flow has be added.
You can follow the progress at https://github.com/joyent/node/pull/6011

==

**A few modules are already using it:**
 - [asynctrace](https://github.com/Empeeric/asynctrace) - Deep stack traces
 - [node-cls](https://github.com/Empeeric/node-cls) - Continuation Local Storage