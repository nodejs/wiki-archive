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
    git checkout v0.4.12 # optional.  Note that master is unstable.
    ./configure
    make -j2 # -j sets the number of jobs to run
    [sudo] make install

**NOTE**: In mac OSX 10.7 compilation fails if you use make -j _n_ , you can use 
    make

You may wish to install Node in a custom folder instead of a global directory. If so, use something like this, after cloning and checking out:

    ./configure --prefix=/opt/node
    make -j2 # Again, -j sets the number of jobs to run
    [sudo] make install
    echo 'export NODE_PATH=/opt/node:/opt/node/lib/node_modules' >> ~/.profile # ~/.bash_profile or ~/.bashrc on some systems

You may want to put the node executables in your path as well for easier use:

    echo 'export PATH=$PATH:/opt/node/bin' >> ~/.profile # ~/.bash_profile or ~/.bashrc on some systems

To reload the path in your current instance of Terminal or bash, use:

    . ~/.profile # Or ~/.bash_profile

If you have any installation problems, look at [Troubleshooting
Installation](https://github.com/ry/node/wiki/Troubleshooting-Installation), try an [alternative installation method](https://gist.github.com/579814), or stop into [#node.js](http://webchat.freenode.net/?channels=node.js&uio=d4) and ask questions.

The default `node` is *not* Node.js on Ubuntu/Debian; `nodejs` is, but is incredibly old because of the fast development pace of Node.js at this time.

**Pre-built binaries**

You can also install node from packages: [[Installing Node.js via package manager]]

**Configure shell script for Ubuntu**

Rock-solid Node.js Platform on Ubuntu. Configure shell script for install node.js using http://apptob.org

## Step 3b - Building on Windows

**Pre-built binaries**

Windows Build (Node v0.6.0): http://nodejs.org/dist/v0.6.0/node.exe

You can also install the binaries using [Chocolatey](http://chocolatey.org/packages/nodejs).

**Building with VC++**

Building with Microsoft VC++ is the best way to natively compile Node starting with the 0.5.x branch. If you don't have Visual Studio you can download and use [VC++ 2010 Express](http://www.microsoft.com/visualstudio/en-us/products/2010-editions/visual-cpp-express).

Don't forget you also need [Python](http://www.python.org/download/releases/2.7.2/) to compile under Windows. Do not use 3.x versions of Python as they are incompatible with build scripts distributed with Node. Add Python directory (C:\PythonXX by default) to system PATH variable.

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


## Step 4 - Install NPM **(Windows see below)**

NPM is a package manager that has become the de-facto standard for
installing additional node libraries and programs. Here's the quick
and easy one-liner for installing on Unix.

    # curl http://npmjs.org/install.sh | sh

To install a library e.g. Express:

    # npm install express

And visit
[https://github.com/isaacs/npm](https://github.com/isaacs/npm) for
details.

### NPM for Windows Native - Experimental
**IMPORTANT** Isaacs will explode your computer across space and time through sheer force of will if you create issues without following the instructions exactly or without fully providing the requisite debug info. This is in an early stage of functionality and **will** break. Make sure you're using the latest version of Node because the 0.5.x branch has seen rapid development, particularly for Windows native, with large chunks of functionality added or changed between minor releases. (0.5.8 or later recommended).

http://npmjs.org/doc/README.html#Installing-on-Windows-Experimental


---
If you have any installation problems, and are sure you've followed each step correctly, stop into [#node.js](http://webchat.freenode.net/?channels=node.js&uio=d4) and ask questions. Cheers!