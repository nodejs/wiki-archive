Troubleshooting a Node Installation

You may encounter the following issues when trying to build or test Node.js

## Mac OS X 10.6.2

### /usr/sbin not in $PATH:

**Solution:**

    $ nano ~/.profile
    // PATH="/usr/sbin/:$PATH"
    $ source ~/.profile

### gpg-error Not Found on ./configure

Download and install MacGPG from [macgpg.sourceforge.net](http://macgpg.sourceforge.net/)

## FreeBSD

### execinfo.h: No such file or directory:

**Solution:**

Install libexecinfo (devel/libexecinfo).

### load of errors beginning with "Need an operator"

**Solution:**

You should use gmake instead of make, as of 0.3.0 where the build system changed.
Also, calling autoconf and/or ./configure may help.

## lib32stdc missing

**Solution:**

    $ sudo apt-get  libc6-dev-i386 lib32stdc++6 
    $ sudo ln -s /usr/lib32/libstdc++.so.6 /usr/lib32/libstdc++.so
    $ git clone git://github.com/ry/node.git
    $ cd node
    $ ./configure 
    $ make
    $ sudo make install

## Thanks

[Travis Swicegood](http://www.travisswicegood.com/index.php/2009/07/11/compiling-node-js-on-ubuntu-9-04)
