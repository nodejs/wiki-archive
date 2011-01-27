### What is the difference between Node "core" and "userland" modules
  
https://github.com/ry/node/wiki/node-core-vs-userland

### What is the versioning scheme?

Odd versions are unstable, even versions are stable. v0.2 is even/stable. v0.3 is odd/unstable. The next stable release will be v0.4. The stable branch takes bug fixes only - it does not change the JavaScript API, addon API, nor ABI (you don't have to rebuild modules after upgrading node with-in a stable branch).