### Where are #node.js irc logs?
[[http://logs.nodejs.org/node.js/]]

### What is the "official" spelling/capitalization/pronunciation?

- When referring to the software or the project in general, it's Node.js or simply Node.  It is a proper noun, so capitalize it.  The `.js` appears with the first use, to disambiguate from other things called "Node", and `Node` (without the .js) afterwards.  One way to think of this is that `Node.js` is the full name, and `Node` is the more familiar first name.
- When referring specifically to the binary executable, it's always `node`, lower-case to match the binary's name itself.
- When referring to the libs that are included with the binary, as opposed to the libs published by Node users in The npm Registry, it's `node-core` or `node core`.  (See [[node core vs userland]].)
- "npm" is always lowercase, even when it appears at the start of a sentence, unless used in a context where all-caps are used (such as the title of a man page.)
- "Node.js" is pronounced either "node dot jay ess", or "node jay ess", or "node point javascript".
- "npm" is pronounced "en pee em", spelled out.

### What is the difference between Node &quot;core&quot; and &quot;userland&quot; modules
  
[[node core vs userland]]
### What is the versioning scheme?

Odd versions are unstable, even versions are stable. v0.2 and v0.4 are even/stable. v0.3 and v0.5 are odd/unstable. The current stable series is v0.10.x. The next stable series will be v0.12.x. The stable branch takes bug fixes only - it does not change the JavaScript API, add-on API, nor ABI (you don&#39;t have to rebuild modules after upgrading node with-in a stable branch).

### What is an easy way to manage Node.js versions / installations?

* [[n|https://github.com/visionmedia/n]]
* [[nvm|https://github.com/creationix/nvm]]
* [[nave|https://github.com/isaacs/nave]]
* [[nodebrew|https://github.com/hokaccha/nodebrew]]

### What is the memory limit on a node process?

Currently, by default v8 has a memory limit of 512mb on 32-bit systems, and 1gb on 64-bit systems. The limit can be raised by setting `--max_old_space_size` to a maximum of ~1024 (~1 GiB) (32-bit) and ~1741 (~1.7GiB) (64-bit), but it is recommended that you split your single process into several workers if you are hitting memory limits.

FAQ on Leap Seconds
===

### What are Leap Seconds? What impact do they have on node.js applications?

Leap seconds are seconds added or removed from UTC (Coordinated Universal Time) to keep
it in sync with the Earth's rotation. If leap seconds were not added
or removed, then UTC would drift from TAI. You can read much more about
leap seconds on [Wikipedia](https://en.wikipedia.org/wiki/Leap_second).

Because they are based on astronomical observations, leap seconds are scheduled and not
predicted, or predictable.  One has been scheduled for June 30th, 2015. What this means practically is that
23:59:59 will be followed by 23:59:60 before going on to 00:00:00. Leap seconds can be added (positive) or removed (negative). A negative leap second would mean that 23:59:58 would be followed by 00:00:00.

There have been [problems](https://en.wikipedia.org/wiki/Leap_second#Examples_of_problems_associated_with_the_leap_second)
caused by software not managing this second properly. Note also that depending on the specific implementation and
time sychronization mechanisms used, a particular system may not actually "see" the leap second, but it will occur as
a regular one second time correction at a later time.

As to how this affects node.js applications, as [specified by ECMA-262](http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.1), JavaScript
and thus node.js, explicitly *ignore* leap seconds when calculating the difference, in seconds, between
two dates. In any event, it wouldn't be possible to take those into account without
a historical table, and taking deltas more than 6 months in the future is impossible as the schedule is not known.

For example, the following example code ( counting the number of seconds since Midnight, Jan 1st, 1970 GMT) does not
include leap seconds.
```
var d0 = new Date(0);
var d1 = new Date(); // right now.
console.log('Unix time started',new Number((d1-d0)/1000).toLocaleString(),'seconds ago');
```

