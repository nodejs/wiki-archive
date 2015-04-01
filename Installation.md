# Installing/Building Node.js

***

# Installing
* [Linux](#installing-on-linux)
* [Mac](#installing-on-mac)
* [Windows](#installing-on-windows)
* [SunOS](#installing-on-sunos)
* [Linaro Ubuntu](#installing-on-linaro-ubuntu-arm-udoo)
* [Via package manager](#installing-via-package-manager)

***

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
You can install a pre-built version of node.js via [the downloads page](http://nodejs.org/download/) available in a **.tar.gz**.
Or you can use the automatic [Installer](https://github.com/taaem/nodejs-linux-installer/releases).

## Installing on Mac
You can install a pre-built version of node.js via [the downloads page](http://nodejs.org/download/) using a **.pkg** or available in a **.tar.gz**.

## Installing on Windows
You can install a pre-built version of node.js via [the downloads page](http://nodejs.org/download/) using a **.exe** or a **.msi**.

## Installing on SunOS
You can install a pre-built version of node.js via [the downloads page](http://nodejs.org/download/) available in a **.tar.gz**.

## Installing on Linaro Ubuntu (ARM, [UDOO](http://udoo.org))
[Compiled binaries and build instructions are available here.](https://github.com/pilwon/nodejs-for-linaro-ubuntu)

## Installing via package manager
See [[Installing Node.js via package manager]] for more information.

### Building Prerequisites

* **GCC** 4.2 or newer

* **GNU make** 3.81 or newer. Pre-installed on most systems. Sometimes called `gmake`.

* [**python**](http://python.org) 2.6 or 2.7. The build tools distributed with
  Node run on python.

* **libssl-dev** (Node v0.6.x only.) Can usually be installed on *NIX systems with your favorite package manager. Pre-installed on OS X.

* **libexecinfo** (FreeBSD and OpenBSD only.) Required by V8. `pkg_add -r libexecinfo` installs it.

* [**ICU**](http://icu-project.org) (optional) to build the Intl (EcmaScript 402) support. See [Intl](Intl) for more details.

## Building on Linux

There's a number of ways to install Node.js on Linux, instructions for installing Node.js on specific Linux distributions using a package manager can be found at: [Installing Node.js via package manager](https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager).

The filenames vary with the Node's version. The following examples are for Node v0.6.18.

Do something like this

```sh
tar -zxf node-v0.6.18.tar.gz #Download this from nodejs.org
cd node-v0.6.18
./configure && make && sudo make install
```

If you are installing on an illumos 64 bit system consider the following to enable dtrace support 

```sh
tar -zxf node-v0.6.18.tar.gz #Download this from nodejs.org
cd node-v0.6.18
./configure --with-dtrace --dest-cpu=x64 && make && sudo make install
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


### Building on Mac

**OSX 10.8 with Xcode 4.5**

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

`vcbuild.bat nosign release x64` : Build in release mode in 64-bit computers

`vcbuild.bat nosign debug x64`   : Build in debug mode for 64-bit computers

`vcbuild.bat nosign release`     : Build in release mode in 32-bit computers

`vcbuild.bat clean`     : Clean Project

You need to have Microsoft Visual Studio 2012/2010 (Express edition is fine) as well as Python 2.6/2.7.  Openssl is not required. Make sure that python is in your PATH.

Below is a example of what building node.js will be like in 64-bit debug mode.
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

## Building on Cygwin
Cygwin is no longer supported, despite being [POSIX](http://en.wikipedia.org/wiki/Posix) compliant. The latest version that compiles is 0.4.12.

```
wget http://nodejs.org/dist/node-v0.4.12.tar.gz
tar xvfz node-v0.4.12.tar.gz
cd node-v0.4.12/
./configure
make
make install
```

# Other Information

## Managing multiple versions of Visual Studio
Lets assume that you have two versions of Visual Studio installed. In this case, you may build against a specific Visual Studio Version. If you want to force a specific version of Visual Studio, you may use the variable GYP_MSVS_VERSION.

Example : Force Visual Studio 2012
set GYP_MSVS_VERSION=2012

## Upgrading on Mac with `.pkg`

You can download the latest `.pkg` and run the installer and it will overwrite the existing version of Node currently installed.

## Known Issues

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

If you are compiling on a NFS mount and get errors at the linker stage or are getting complaints about `flock` not being available on your system, try changing the linker:

```
make LINK=g++
```