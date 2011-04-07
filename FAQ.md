### What is the difference between Node "core" and "userland" modules
  
[[node core vs userland]]
### What is the versioning scheme?

Odd versions are unstable, even versions are stable. v0.2 and v0.4 are even/stable. v0.3 is odd/unstable. The next unstable release will be v0.5 and the next stable release will be v0.6. The stable branch takes bug fixes only - it does not change the JavaScript API, addon API, nor ABI (you don't have to rebuild modules after upgrading node with-in a stable branch).

### What is the correct capitalization of Node.js?

The official name of Node is "Node". The unofficial name is "Node.js" to disambiguate it from other nodes.

### What is an easy way to manage Node.js versions / installations?

* [[n|https://github.com/visionmedia/n]]
* [[nvm|https://github.com/creationix/nvm]]
* [[nave|https://github.com/isaacs/nave]]