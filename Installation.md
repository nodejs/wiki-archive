##Step 1 - Pick Your Platform

Node should install out of the box on Linux, Macintosh, and Solaris.

With some effort you should be able to get it running on other Unix platforms and Windows (either via Cygwin or MinGW).

##Step 2a - Building on Unix

Just as you install any other program

    mkdir ~/local
    ./configure --prefix=$HOME/local/node
    make
    make install
    export PATH=$HOME/local/node/bin:$PATH


##Step 2b - Building on Windows

There are two ways of building Node. One is over the Cygwin emulation layer the other is using MinGW (GNU toolchain for windows). See the [Cygwin](https://github.com/ry/node/wiki/Building-node.js-on-Cygwin-%28Windows%29) and [MinGW](https://github.com/ry/node/wiki/Building-node.js-on-mingw) pages.

Neither builds are satisfactorily stable but it is possible to get something running.