Occasionally, in the discussions in the NodeJS mailing lists and IRC channels, you may hear things referred to as "node-core" and "userland".

Of course, traditionally, "userland" or "userspace" refer to everything outside the operating system kernel.  In that sense, Node itself is a "userland" program.

However, in the context of NodeJS, "core" refers to the modules and bindings that are compiled into NodeJS.  In general, they provide a hook into very well-understood low-level functionality which almost all networking programs are going to require: TCP, HTTP, DNS, the File System, child processes, and a few other things.  If something is fancy enough to argue about, there's a good chance it won't be part of node-core.  HTTP is about as big as it gets, and if it wasn't so popular, it'd certainly not be a part of node.

There are also some things in node-core that are simply too painful to do without in a JavaScript environment, or which have been created to implement some BOM constructs which are not part of the JavaScript language, but may as well be (eg, setTimeout and setInterval).

Everything else is "userland".  This includes: npm, express, request, coffee-script, mysql clients, redis clients, and so on.  You can often install these programs using [npm](http://npmjs.org/).

The question of what is properly "node-core" and what belongs in "userland" is a constant battleground.  In general, node is based on the philosophy that it should *not* come with "batteries included".  It is easier to move things out of node-core than it is to move them in.

## This is a Good Thing.

One goal of node's minimal core library is to encourage people to implement things in creative ways, without forcing their ideas onto everyone.  With a tiny core and a vibrant user space, we can all flourish and experiment without the onerous burden of having to always agree all the time.