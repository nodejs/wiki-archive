When editing this page please be as detailed as possible. Examples are encouraged!

## Buffers

### Module API Changes

* `Buffer` class has been removed and replaced with a namespace. So `using node::Buffer` will no longer work.
* All `node::Buffer::New()` variants now return `Local<Object>` instead of `Buffer*`.
* The return value for `node::Buffer::New()` is an instantiated JS `Buffer` object.
* `node::Buffer::New(Handle<String>)` now accepts an optional second argument of `enum encoding`.
* API addition of `node::Buffer::Use()` which will use the passed `char*` instead of making a copy.
* `(new Buffer('text\0!', 'ascii')).toString()` outputs `'text !' in 0.10 and `'text\u0000!'` in 0.12.
* Writable stream `_write()` gets called with 'buffer' encoding when chuck is a Buffer ([#6119](https://github.com/joyent/node/issues/6119)).
* Writable stream emits 'finish' on next tick if there was a `write()` ([#6118](https://github.com/joyent/node/issues/6118)).

### JS API Changes

* External memory is now allocated using `smalloc`, instead of using `SlowBuffer` as the parent backing.
* `SlowBuffer` has been repurposed to return a `Buffer` instance that has no parent backing.
* `buffer.parent` now points to an object that has been allocated via `smalloc.alloc`, not a `Buffer` instance, and only if the buffer is a slice.
* `buffer.offset` is now a read-only prototype property equal to zero since no instance methods require working on the parent backing.
* API additions `Buffer.alloc()` and `Buffer.dipose()` have been added.
* `Buffer#fill()`  has been extended to fill with the entire passed value.

## Process

* `process.maxTickDepth` has been removed, allowing `process.nextTick` to starve I/O indefinitely.