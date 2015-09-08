# Installing/Building Node.js

***

# Installing
* [Linux](#installing-on-linux)
* [Mac](#installing-on-mac)
* [Windows](#installing-on-windows)
* [SunOS](#installing-on-sunos)

***

``` this is a work in process ```

# Building
* [Building Prerequisites](#building-prerequisites)
* [Linux](#building-on-linux)
* [Mac](#building-on-mac)
* [Windows](#building-on-windows)
* [Cygwin](#building-on-cygwin)

***

# Other information
* [Managing multiple versions of Visual Studio](#managing-multiple-versions-of-visual-studio)
* [Upgrading on Mac with .pkg](upgrading-on-mac-with-pkg)
* [Known Issues](#known-issues)

***

## Installing on Linux
You can install a pre-built version of Node.js via [the downloads page](http://iojs.org/download/) available in a **.tar.gz**.

## Installing on Mac
You can install a pre-built version of Node.js via [the downloads page](http://iojs.org/download/) using a **.pkg** or available in a **.tar.gz**.

## Installing on Windows
You can install a pre-built version of Node.js via [the downloads page](http://iojs.org/download/) using a **.exe** or a **.msi**.

## Installing on SunOS
You can install a pre-built version of Node.js via [the downloads page](http://iojs.org/download/) available in a **.tar.gz**.

## Installing via package manager
See [[Installing Node.js via package manager]] for more information.

### Building Prerequisites

* **GCC** 4.8 or newer, or **clang** 3.4 or newer

* **GNU make** 3.81 or newer. Pre-installed on most systems. Sometimes called `gmake` (required on FreeBSD).

* [**python**](http://python.org) 2.6 / 2.7. The build tools provided require Python to configure the project correctly.

* **libexecinfo** (FreeBSD and OpenBSD only.) Required by V8. `pkg_add -r libexecinfo` installs it.

* [**ICU**](http://icu-project.org) (optional) to build the Intl (EcmaScript 402) support. See [Intl](Intl) for more details.

## Building on Linux

The filenames vary with the Node.js's version. The following examples are for Node.js v1.3.0.

Do something like this

```sh
tar -zxf iojs-v1.3.0.tar.gz # Available at https://iojs.org/dist/v1.3.0/iojs-v1.3.0.tar.gz
cd iojs-v1.3.0
./configure && make && sudo make install
```

If you are installing on an illumos 64 bit system consider the following to enable dtrace support 

```sh
tar -zxf iojs-v1.3.0.tar.gz # Available at https://iojs.org/dist/v1.3.0/iojs-v1.3.0.tar.gz
cd iojs-v1.3.0
./configure --with-dtrace --dest-cpu=x64 && make && sudo make install
```

Or, if you'd like to install from the repository

```sh
git clone https://github.com/iojs/node.js.git
cd node.js
git checkout v1.3.0
./configure && make && sudo make install
```

You may wish to install Node.js in a custom folder instead of a global directory. 

    ./configure --prefix=/custom/folder && make && sudo make install

You can really speed up building process by adding `-j` argument with a number usually approximately equals number of cores plus one, so `make -j 5` would be appropriate for a quad-core processor.

You may want to put the Node.js executables in your path as well for easier use, if you specified a custom prefix. Node.js will be available after logging out and back in again, if you installed Node.js to the default /usr/local/ location. Add this line to your `~/.profile` or `~/.bash_profile` or `~/.bashrc` or `~/.zshenv`

    export PATH=$PATH:/custom/folder/bin

# Other Information

## Upgrading on Mac with `.pkg`

You can download the latest `.pkg` and run the installer and it will overwrite the existing version of Node.js currently installed.