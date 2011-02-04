Many people are running code on the stable branch v0.2. Soon the unstable branch, v0.3, will become stable. You can help the community by documenting what API changes were made between the two.

## JavaScript API

* Instead of testing exceptions with `if (exception.errno == require('constants').EADDRINUSE)`, the exceptions now have an member `code` which is just the string. So you can now do: `if (exception.code == 'EADDRINUSE')`

* `sys` module is now the `util` module (soft deprecation)

* `querystring.parse()` and `querystring.stringify()` were [made simpler](https://github.com/ry/node/commit/422d3c93bc7391e105cfb4363011088c27ec86a6). They no longer handle deep objects.

* v0.3.6 introduced a new interface for making outbound http requests: `http.request()` and `http.get()`. Previously users had to construct a `http.Client` instance and issue `client.request()` calls. The old `http.Client` interface is deprecated. The old interface is still supported. `setSecure()` is missing and will not be implemented. Use the new interface for making HTTPS requests. See [the `https` module documentation](https://github.com/ry/node/blob/v0.3.7/doc/api/https.markdown) for the new interface. The old HTTPS implementation was never working properly.

The old http client API:

    var http = require('http');
    var google = http.createClient(80, 'www.google.com');
    var request = google.request('GET', '/',
      {'host': 'www.google.com'});
    request.end();
    request.on('response', function (response) {
      console.log('STATUS: ' + response.statusCode);
      console.log('HEADERS: ' + JSON.stringify(response.headers));
      response.setEncoding('utf8');
      response.on('data', function (chunk) {
        console.log('BODY: ' + chunk);
      });
    });

The new http client API:

    var options = {
      host: 'www.google.com',
      path: '/',
    };
    http.get(options, function(response) {
      console.log('STATUS: ' + response.statusCode);
      console.log('HEADERS: ' + JSON.stringify(response.headers));
      response.setEncoding('utf8');
      response.on('data', function (chunk) {
        console.log('BODY: ' + chunk);
      });
    });





* The internal `process.binding('stdio')` module got exposed and renamed to `require('tty')`

* `readline.createInterface` now takes three arguments, an input stream, an output stream, and a completion callback. Previously only a output stream was provided.

* [`require()` now calls realpath on modules.](https://github.com/ry/node/commit/0853730c35e567b1cd2e553986298e57f3908f02) That means if the module is a symlink relative requires will be relative to where the actual file is.

* NEW: [require.resolve](http://nodejs.org/docs/v0.3.6/api/all.html#require.resolve)

* NEW: [path.resolve](http://nodejs.org/docs/v0.3.6/api/all.html#path.resolve)

* REMOVED: `path.split`, `path.normalizeArray`


## C++ API

* The internal `node::Buffer` class had dramatic changes early on in the v0.3. Any addons which used `node::Buffer` will require heavy rewrite. You might find [this](https://github.com/pkrumins/node-png/blob/791d4c6df1402daa15dc7930f084d95c48e63c98/src/buffer_compat.h) and [this](https://github.com/pkrumins/node-png/blob/791d4c6df1402daa15dc7930f084d95c48e63c98/src/buffer_compat.cpp) helpful. Also see [buffer.h](https://github.com/ry/node/blob/v0.3.7/src/node_buffer.h)

* The binary interface has changed between v0.2 and v0.3. You must recompile all addons. v0.4 will introduce new ABI stability, until then expect to recompile all addons every time you upgrade Node on the v0.3 branch.

## Broken Modules

It appears that connect-auth (with the GitHub module) is broken in v0.3.  [example application.](https://github.com/aaronblohowiak/connect-auth-37-bug-example)