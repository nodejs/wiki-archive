### Where are #node.js irc logs?
[[http://nodejs.debuggable.com/]]

### What is the difference between Node "core" and "userland" modules
  
[[node core vs userland]]
### What is the versioning scheme?

Odd versions are unstable, even versions are stable. v0.2 and v0.4 are even/stable. v0.3 and v0.5 are odd/unstable. The next stable release will be v0.6. The stable branch takes bug fixes only - it does not change the JavaScript API, addon API, nor ABI (you don't have to rebuild modules after upgrading node with-in a stable branch).

### What is the correct capitalization of Node.js?

The official name of Node is "Node". The unofficial name is "Node.js" to disambiguate it from other nodes.

### What is an easy way to manage Node.js versions / installations?

* [[n|https://github.com/visionmedia/n]]
* [[nvm|https://github.com/creationix/nvm]]
* [[nave|https://github.com/isaacs/nave]]

### What is the memory limit on a node process?

Currently, by default v8 has a memory limit of 512mb on 32-bit systems, and 1gb on 64-bit systems.  The limit can be raised by setting --max-old-space-size to a maximum of ~1gb (32-bit) and ~1.7gb (64-bit), but it is recommended that you split your single process into several workers if you are hitting memory limits.

### [Meta] How have release sizes changed over time?
![Graph of size in MB vs node release, by dtrejo.com](http://c0848462.cdn.cloudfiles.rackspacecloud.com/c97716dd8e4f943501bfcaadcecfdd75d06407afde.jpg "Graph of size in MB vs node release, by dtrejo.com")