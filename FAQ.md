### Where are #node.js irc logs?
[[http://logs.nodejs.org/node.js/]]

### What is the "official" spelling/capitalization/pronunciation?

- When referring to the software or the project in general, it's Node.js or simply Node.  It is a proper noun, so capitalize it.  The `.js` appears with the first use, to disambiguate from other things called "Node", and `Node` (without the .js) afterwards.  One way to think of this is that `Node.js` is the full name, and `Node` is the more familiar first name.
- When referring specifically to the binary executable, it's always `node`, lower-case to match the binary's name itself.
- When referring to the libs that are included with the binary, as opposed to the libs published by Node users in The npm Registry, it's `node-core`.  (See [[node core vs userland]].)
- "npm" is always lowercase, even when it appears at the start of a sentence, unless used in a context where all-caps are used (such as the title of a man page.)
- "Node.js" is pronounced either "node dot jay ess", or "node jay ess", or "node point javascript".
- "npm" is pronounced "en pee em", spelled out.

### What is the difference between Node &quot;core&quot; and &quot;userland&quot; modules
  
[[node core vs userland]]
### What is the versioning scheme?

Odd versions are unstable, even versions are stable. v0.2 and v0.4 are even/stable. v0.3 and v0.5 are odd/unstable. The current stable series is v0.10.x. The next stable series will be v0.12.x. The stable branch takes bug fixes only - it does not change the JavaScript API, addon API, nor ABI (you don&#39;t have to rebuild modules after upgrading node with-in a stable branch).

### What is an easy way to manage Node.js versions / installations?

* [[n|https://github.com/visionmedia/n]]
* [[nvm|https://github.com/creationix/nvm]]
* [[nave|https://github.com/isaacs/nave]]
* [[nodebrew|https://github.com/hokaccha/nodebrew]]

### What is the memory limit on a node process?

Currently, by default v8 has a memory limit of 512mb on 32-bit systems, and 1gb on 64-bit systems.  The limit can be raised by setting --max-old-space-size to a maximum of ~1gb (32-bit) and ~1.7gb (64-bit), but it is recommended that you split your single process into several workers if you are hitting memory limits.

### Crashes on 32-bit FreeBSD systems

Known issue: https://github.com/joyent/node/issues/4412

### [Meta] How have release sizes changed over time?
![Graph of size in MB vs node release, by dtrejo.com](http://c0848462.cdn.cloudfiles.rackspacecloud.com/c97716dd8e4f943501bfcaadcecfdd75d06407afde.jpg &quot;Graph of size in MB vs node release, by dtrejo.com&quot;)