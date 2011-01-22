Many people are running code on the stable branch v0.2. Soon the unstable branch, v0.3, will become stable. You can help the community by documenting what API changes were made between the two.

## C++ API

* The internal node::Buffer class had dramatic changes early on in the v0.3. Any addons which used node:Buffer will require heavy rewrite. You might find [this](https://github.com/pkrumins/node-png/blob/791d4c6df1402daa15dc7930f084d95c48e63c98/src/buffer_compat.h) helpful. [buffer.h](https://github.com/ry/node/blob/v0.3.6/src/node_buffer.h)

## JavaScript API

* sys module is now the util module (soft deprecation)

* querystring.parse and querystring.stringify were [made simpler](https://github.com/ry/node/commit/422d3c93bc7391e105cfb4363011088c27ec86a6). They no longer handle deep objects.

* v0.3.6 introduced a new interface for making outbound http requests: http.request() and http.get(). Previously users had to construct a http.Client instance and issue client.request() calls. The old http.Client interface should be considered deprecated. That said, there is a legacy interface for http.Client accessible which runs on top of http.request() - you may find inconsistencies between v0.3.6+ http.Client and the http.Client before v0.3.6. These inconsistencies are considered bugs and should be reported.

* The stdio module got renamed to tty

* NEW: [require.resolve](http://nodejs.org/docs/v0.3.6/api/all.html#require.resolve)

* NEW: [path.resolve](http://nodejs.org/docs/v0.3.6/api/all.html#path.resolve)

* REMOVED: path.split, path.normalizeArray

## Internal C++ API (inside of NODE_ROOT/src/*.cc)

## Internal Javascript API (inside of NODE_ROOT/lib/*.js

 