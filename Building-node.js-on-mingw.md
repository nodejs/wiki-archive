The latests versions of Node (v0.3.6 and better) can be built with mingw. This is work in progress; expect this node build to be unstable and very much incomplete.

# Prerequisites

* windows xp sp1 or later
* the latest mingw
  * use mingw-get-inst from [mingw.org](http://www.mingw.org/wiki/InstallationHOWTOforMinGW)
  * install the developer toolkit as well as C and C++ compilers.
* python 2.7 from [python.org](http://www.python.org/download/)
* git, preferably [msysgit](http://code.google.com/p/msysgit/)

# Build steps

This will assume you're checking out and compiling node in c:\node

* Make sure python and git directories are in your path environment variable. 
  A guide to editing path on windows can be found [here](http://www.java.com/en/download/help/path.xml)
* Open the mingw bash shell
* `cd /c`
* `git clone https://github.com/ry/node.git`
* `cd /c/node`
* `./configure --without-ssl`
* `make`
* `./node.exe`

# Known issues

These are known issues with the build process. A list of known issues with mingw-built node can be found found in [TODO.win32](https://github.com/ry/node/raw/master/TODO.win32).

### Git doesn't work from the mingw bash shell
Some people have reported problems getting this to work. If you are affected by this, use git from the windows command prompt.

### Build fails with msvc / Microsoft Visual Studio installed
Having Microsoft Visual Studio installed confuses the V8 build system. This is a know issue, there is currently no good solution for it.

### ./configure can't find openssl
Linking with openssl is currently unsupported. Use `./configure --without-ssl`

