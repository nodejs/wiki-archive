##Step 1 - Pick Your Platform

Node should install out of the box on Linux, Macintosh, and Solaris.

With some effort you should be able to get it running on other Unix platforms and Windows (either via Cygwin or MinGW).

##Step 2a - Building on Unix (including BSD and Mac)

Just as you install any other program

    mkdir ~/local
    ./configure --prefix=$HOME/local/node
    make
    make install
    export PATH=$HOME/local/node/bin:$PATH

If you have any installation problems, looak at [installation notes](https://github.com/ry/node/wiki/Installation-Notes).

Also you can install node from packages: [RPM and DEB packages for Node.js](https://github.com/ry/node/wiki/RPM-and-DEB-packages-for-Node.js).

##Step 2b - Building on Windows

There are two ways of building Node. One is over the Cygwin emulation layer the other is using MinGW (GNU toolchain for windows). See the [Cygwin](https://github.com/ry/node/wiki/Building-node.js-on-Cygwin-%28Windows%29) and [MinGW](https://github.com/ry/node/wiki/Building-node.js-on-mingw) pages.

Neither builds are satisfactorily stable but it is possible to get something running.

##Step 3 - Install node libs with npm

NPM is the de-facto standard for installing additional node libraries and programs. To install npm: 
    curl http://npmjs.org/install.sh | sh
For installing a library e.g. express:
    npm install express
And visit [https://github.com/isaacs/npm](https://github.com/isaacs/npm) for details.
