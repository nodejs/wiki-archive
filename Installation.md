# Building and Installing Node.js

## Step 1 - Pick Your Platform

Node should install out of the box on Linux, Macintosh, Solaris, and Windows.

With some effort you should be able to get it running on other Unix platforms.

Mac OSX users also have the option of installing a precompiled package from [here](https://sites.google.com/site/nodejsmacosx/) that includes npm.

## Step 2 - Prerequisites

Node has several dependencies, but fortunately most of them are
distributed along with it.  If you are building from source you should
only need 2 things.

* **python** - version 2.6 or higher. The build tools distributed with
  Node run on python.

* **libssl-dev** - If you plan to use SSL/TLS encryption in your
  networking, you'll need this.  Libssl is the library used in the
  [openssl](http://www.openssl.org/) tool. On Linux and Unix systems
  it can usually be installed with your favorite package manager. The
  lib comes pre- installed on OS X.

## Step 3a - Installing on Unix (including BSD and Mac)

**Building from source**

Use make to build and install Node (execute the following on the command line)

    git clone --depth 1 git://github.com/joyent/node.git # or git clone git://github.com/joyent/node.git if you want to checkout a stable tag
    cd node
    git checkout v0.4.11 # optional.  Note that master is unstable.
    export JOBS=2 # optional, sets number of parallel commands.
    mkdir ~/local
    ./configure --prefix=$HOME/local/node
    make
    make install
    echo 'export PATH=$HOME/local/node/bin:$PATH' >> ~/.profile
    echo 'export NODE_PATH=$HOME/local/node:$HOME/local/node/lib/node_modules' >> ~/.profile
    source ~/.profile

If you have any installation problems, look at [Troubleshooting
Installation](https://github.com/ry/node/wiki/Troubleshooting-Installation), try an [alternative installation method](https://gist.github.com/579814), or stop into [#node.js](http://webchat.freenode.net/?channels=node.js&uio=d4) and ask questions.

`sudo apt-get install node` Wont work on ubuntu/debian machines. That is a different package with the same name. Use the above method itself for installing node.

**Pre-built binaries**

You can also install node from packages: [[Installing Node.js via package manager]]

**Configure shell script for Ubuntu**

Rock-solid Node.js Platform on Ubuntu. Configure shell script  for install node.js  using http://apptob.org


## Step 3b - Building on Windows

**Pre-built binaries**

Windows Build (Node v0.5.7): http://nodejs.org/dist/v0.5.7/node.exe


**Building with VC++**

Building with Microsoft VC++ is the best way to natively compile Node starting with the 0.5.x branch. If you don't have Visual Studio you can download and use [VC++ 2010 Express](http://www.microsoft.com/visualstudio/en-us/products/2010-editions/visual-cpp-express).

The batch file *vcbuild.bat* handles running gyp to generate the Visual Studio Solution files and will also run the VC++ compiler without requiring you to compile from the Visual Studio IDE. You should be able to complete the whole process and generate the Debug .exe by simply running vcbuild.bat from explorer or from the command prompt.

vcbuild.bat can be used to generate Visual Studio project files, build binaries, or both. By default it will generate project files and then build the debug version of Node. These are the command line options:

* debug
* release
* clean
* noprojgen
* nobuild

For testing:

* test
* test-all
* test-uv
* test-internet
* test-pummel
* test-simple
* test-message

****

Alternatively you can build Node using the MinGW toolchain: [MinGW build instructions](https://github.com/joyent/node/wiki/Building-node.js-on-mingw).

Cygwin can also be used but this is generally not recommended, and especially not for making native Windows binaries. [Cygwin build instructions](https://github.com/joyent/node/wiki/Building-node.js-on-Cygwin-%28Windows%29)


**Hosting in IIS on Windows**

It is possible to host node.js application in IIS on Windows using the [iisnode](https://github.com/tjanczuk/iisnode) IIS module. More details available [here](http://tomasz.janczuk.org/2011/08/hosting-nodejs-applications-in-iis-on.html). 


## Step 4 - Install NPM (not functional on Windows yet)

NPM is a package manager that has become the de-facto standard for
installing additional node libraries and programs. Here's the quick
and easy one-liner for installing on Unix.

    # curl http://npmjs.org/install.sh | sh

To install a library e.g. Express:

    # npm install express

And visit
[https://github.com/isaacs/npm](https://github.com/isaacs/npm) for
details.

**NPM does not work on Windows yet**