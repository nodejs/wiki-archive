Your code may "just work" when you upgrade.  Install the new version of node (with nave or similar) and run your tests.  You might be surprised that they run a little faster without much change otherwise. Great! Get some coffee and maybe a blueberry scone.

However, if something broke, these are the likely culprits:

#### Cool new stuff
   [require.resolve](http://nodejs.org/docs/v0.3.6/api/all.html#require.resolve)


#### C++ API
   [Buffer](https://github.com/ry/node/blob/master/src/node_buffer.h)


#### JS API
* sys module is now the util module (soft deprecation)