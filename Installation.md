# Building and installing Node.js

You may build the Node.js engine for any of the supported platforms.

For Windows and Mac some pre-built binaries are available; you may [install them](https://github.com/joyent/node/wiki/Installation#installing-without-building) without building Node for yourself.

## Prerequisites and known issues of building

### Prerequisites

* **GCC** 4.2 or newer

* **GNU make** 3.81 or newer. Pre-installed on most systems. Sometimes called `gmake`.

* [**python**](http://python.org) 2.6 or 2.7. The build tools distributed with
  Node run on python.

* **libssl-dev** (Node v0.6.x only.) Can usually be installed on *NIX systems with your favorite package manager. Pre-installed on OS X.

* **libexecinfo** (FreeBSD and OpenBSD only.) Required by V8. `pkg_add -r libexecinfo` installs it.

### Known Issues

  On UNIX platforms, make sure that the path doesn't contain whitespace: `/home/user/My Projects/node` won't work.

  If you receive an error during `./configure` like this
  
```
File "/home/flo/node-v0.6.6/tools/waf-light", line 157, in <module>
     import Scripting
File "/home/flo/node-v0.6.6/tools/wafadmin/Scripting.py", line 146
     except Utils.WafError, e:
                          ^
SyntaxError: invalid syntax
```

   it is because Python3 is your default Python version. To fix this issue you have to set Python2 temporary as your default Python:

```sh
export PYTHON=`which python2`
```

Maybe you need to change your `PYTHONHOME` as well. If that doesn't work, you can try creating symlinks to the old python in a directory which comes before python's in $PATH:

```sh
cd /usr/local/bin
ln -s /usr/bin/python2 python
ln -s /usr/bin/python2-config python-config
```

Remember to remove the symlinks when you're done. If you have any further installation problems stop into [#node.js](http://webchat.freenode.net/?channels=node.js&uio=d4) on irc.freenode.net and ask questions.

If you are compiling on a NFS mount and get errors at the linker stage, try this:

```
make LINK=g++
```


## Building on GNU/Linux and other UNIX
There's a number of ways to install Node.js on Linux, instructions for installing Node.js on specific Linux distributions using a package manager can be found at: [Installing Node.js via package manager](https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager).

The filenames vary with the Node's version. The following examples are for Node v0.6.18.

Do something like this

```sh
tar -zxf node-v0.6.18.tar.gz #Download this from nodejs.org
cd node-v0.6.18
./configure && make && sudo make install
```

Or, if you'd like to install from the repository

```sh
git clone https://github.com/joyent/node.git
cd node
git checkout v0.6.18 #Try checking nodejs.org for what the stable version is
./configure && make && sudo make install
```

You may wish to install Node in a custom folder instead of a global directory. 

    ./configure --prefix=/opt/node && make && sudo make install

You can really speed up building process by adding `-j` argument with a number usually approximately equals number of cores plus one, so `make -j 3` would be appropriate for dual-core processor.

You may want to put the node executables in your path as well for easier use. Add this line to your `~/.profile` or `~/.bash_profile` or `~/.bashrc` or `~/.zshenv`

    export PATH=$PATH:/opt/node/bin

If you have SpiderMonkey installed, you may have some conflicting includes. Set `CXXFLAGS="-I./deps/v8/src"` before building to prioritize the v8 files over SpiderMonkey's.

Or use the one liner to install the latest node.js : ```bash < <(curl http://h3manth.com/njs) ```

## Mac OSX
It's easiest to use a [package manager (as mentioned above)](https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager#osx) such as brew or macports.

### Building on Mac OSX 10.8 with Xcode 4.5 
1. Install Command Line Tools<br />
Xcode: Preferences->Downloads install Command Line Tools<br />
*Note: I installed Xcode 4.5 in `/Applications/Xcode`*

1. Download node.js src code
```
git clone https://github.com/joyent/node.git
cd node
git checkout v0.8.2
```

1. Compiling Source Code
```
export CC=/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/clang
export CXX=/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/clang++
./configure && make && sudo make install
```

## Building on Windows

*vcbuild.bat nosign release x64* : Build in release mode in 64-bits

*vcbuild.bat nosign debug x64*   : Build in debug mode for 64-bits

*vcbuild.bat nosign release*     : Build in release mode in 32-bits

*vcbuild.bat clean*     : Clean Project

You need to have Microsoft Visual Studio 2012 or 2010 (Express edition is fine) as well as Python 2.6 or 2.7.  Openssl is not required. Make sure that python is in your PATH.

The underneath is a example of building node in 64-bits debug mode.
```
c:\_GIT\node>.\vcbuild.bat debug x64
ctrpp not found in WinSDK path--using pre-gen files from tools/msvs/genfiles.
{ 'target_defaults': { 'cflags': [],
                       'default_configuration': 'Debug',
                       'defines': ['OPENSSL_NO_SSL2=1'],
                       'include_dirs': [],
                       'libraries': []},
  'variables': { 'clang': 0,
                 'host_arch': 'x64',
                 'node_install_npm': 'true',
                 'node_prefix': '',
                 'node_shared_cares': 'false',
                 'node_shared_http_parser': 'false',
                 'node_shared_libuv': 'false',
                 'node_shared_openssl': 'false',
                 'node_shared_v8': 'false',
                 'node_shared_zlib': 'false',
                 'node_tag': '',
                 'node_use_dtrace': 'false',
                 'node_use_etw': 'true',
                 'node_use_mdb': 'false',
                 'node_use_openssl': 'true',
                 'node_use_perfctr': 'true',
                 'node_use_systemtap': 'false',
                 'python': '**c:\\Python27\\python.exe**',
                 'target_arch': 'x64',
                 'v8_enable_gdbjit': 0,
                 'v8_no_strict_aliasing': 1,
                 'v8_use_snapshot': 'true',
                 'visibility': ''}}
creating  config.gypi
creating  config.mk
Project files generated.
  ares_platform.c
  inet.c
  js2c, and also js2c_experimental
  Assembling c:\_GIT\node\deps\openssl\openssl\crypto\bn\asm\x86_64-win32-masm.asm to Debug\obj\openssl\x86_64-win32-masm.obj.
  http_parser.c
  node_js2c
  ...
[Wait a few minutes]
C:\_GIT\node\Build\Debug\node.exe
> process.versions
{ http_parser: '1.0',
  node: '0.11.7-pre',
  v8: '3.20.14.1',
  uv: '0.11.9',
  zlib: '1.2.3',
  modules: '0x000C',
  openssl: '1.0.1e' }
>
```

The executable will be in `Build\Debug\node.exe`.

## Managing multiple version of Visual Studio
Lets assume that you have two versions of Visual Studio installed. In this case, you may build against a specific Visual Studio Version. If you want to force a specific version of Visual Studio, you may use the variable GYP_MSVS_VERSION.

Example : Force Visual Studio 2012
set GYP_MSVS_VERSION=2012

## Installing without building

You may obtain pre-compiled Node.js binaries for several platforms from [http://nodejs.org/download](http://nodejs.org/download).

### Installing on Windows

#### Manual install

**Installing Node manually is recommended as a workaround for any problems with automatic install. You also have much better understanding of the things that happen if you do those things yourself.**

The [http://nodejs.org/dist/latest/](http://nodejs.org/dist/latest/) directory contains executables of the last version of Node.js engine (the engine **only,** i.e. without npm):

* **32bit version:** [http://nodejs.org/dist/latest/node.exe](http://nodejs.org/dist/latest/node.exe)

* **64bit version:** [http://nodejs.org/dist/latest/x64/node.exe](http://nodejs.org/dist/latest/x64/node.exe)

The [http://nodejs.org/dist/npm/](http://nodejs.org/dist/npm/) directory contains the latest `.zip` archive of npm (such as `npm-1.1.16.zip` when npm v1.1.16 was the latest).

Manual installation steps:

1. Make a clean directory and add that directory to your system's `PATH` variable.

2. Download the latest `node.exe` to that directory.

3. Download the latest npm's `.zip` file and unpack its contents to the same directory.

Then, with the usual help of `PATH`, you'll be able to run scripts (`node scriptname.js`) and install modules (`npm install modulename`) in any directory.

##### Manual update

To update Node, download the latest [http://nodejs.org/dist/latest/node.exe](http://nodejs.org/dist/latest/node.exe) (or [http://nodejs.org/dist/latest/x64/node.exe](http://nodejs.org/dist/latest/x64/node.exe) for 64bit systems) and replace your old `node.exe` with it.

To update npm, run the `npm update npm -g` command.

#### Automatic install (with Microsoft Installer)

The [http://nodejs.org/dist/latest/](http://nodejs.org/dist/latest/) directory contains the latest `.msi` package (such as `node-v0.6.15.msi` when Node v0.6.15 was the latest) that you may use to install both Node.js engine and npm.

### Installing on Mac

The [http://nodejs.org/dist/latest/](http://nodejs.org/dist/latest/) directory contains the latest `.pkg` package (such as `node-v0.6.15.pkg` when Node v0.6.15 was the latest).

### Installing on Linaro Ubuntu

[Compiled binaries and build instructions are available here](https://github.com/pilwon/nodejs-for-linaro-ubuntu)

### Upgrading on Mac with `.pkg`

You can download the latest `.pkg` and run the installer and it will overwrite the existing version of Node currently installed.