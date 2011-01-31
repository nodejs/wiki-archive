# Building and Installing Node.js

## Step 1 - Pick Your Platform

Node should install out of the box on Linux, Macintosh, and Solaris.

With some effort you should be able to get it running on other Unix
platforms and Windows (either via Cygwin or MinGW).

## Step 2 - Prerequisites

Node has several dependencies, but fortunately most of them are
distributed along with it.  If you are building from source you should
only need 2 things.

* **python** - version 2.4 or higher. The build tools distributed with
  Node run on python.

* **libssl-dev** - If you plan to use SSL/TLS encryption in your
  networking. You'll need this.  Libssl is the library used in the
  [openssl](http://www.openssl.org/) tool. On Linux and Unix systems
  it can usually be installed with your favorite package manager. The
  lib comes pre- installed on OS X.

## Step 3a - Installing on Unix (including BSD and Mac)

**Building from source**

Use make to build and install Node

    $ export JOBS=2 # optional, sets number of parallel commands.
    $ mkdir ~/local
    $ ./configure --prefix=$HOME/local/node
    $ make
    $ make install
    $ export PATH=$HOME/local/node/bin:$PATH

If you have any installation problems, look at [Troubleshooting
Installation](https://github.com/ry/node/wiki/Troubleshooting-Installation).

**Pre-built binaries**

You can also install node from packages: [RPM and DEB packages for
Node.js](https://github.com/ry/node/wiki/RPM-and-DEB-packages-for-Node.js).

## Step 3b - Building on Windows

**Building from source**

There are two ways of building Node on Windows. One is over the Cygwin
emulation layer the other is using MinGW (GNU toolchain for
windows). See the
[Cygwin](https://github.com/ry/node/wiki/Building-node.js-on-Cygwin-%28Windows%29)
and [MinGW](https://github.com/ry/node/wiki/Building-node.js-on-mingw)
pages.

Neither builds are satisfactorily stable but it is possible to get
something running.

## Step 4 - Install NPM

NPM is a package manager that has become the de-facto standard for
installing additional node libraries and programs. Here's the quick
and easy one-liner for installing on Unix.

    $ curl http://npmjs.org/install.sh | sh

To install a library e.g. Express:

    $ npm install express

And visit
[https://github.com/isaacs/npm](https://github.com/isaacs/npm) for
details.
