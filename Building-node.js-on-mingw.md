The latests versions of Node (v0.3.6 and better) can be built with mingw. This is work in progress; expect this node build to be unstable and very much incomplete.

# Prerequisites

* Windows xp sp2 or later
* The latest mingw
  * use mingw-get-inst from [mingw.org](http://www.mingw.org/wiki/InstallationHOWTOforMinGW)
  * install the developer toolkit as well as C and C++ compilers.
* Python 2.7 from [python.org](http://www.python.org/download/)
* Git, preferably [msysgit](http://code.google.com/p/msysgit/)

# Build steps

This will assume you're checking out and compiling node in `c:\node`.

* Make sure python and git directories are in your path environment variable. 
  A guide to editing path on windows can be found [here](http://www.java.com/en/download/help/path.xml).
* Open the mingw bash shell
* `cd /c`
* `git clone https://github.com/ry/node.git`
* `cd /c/node`
* `./configure --without-ssl`
* `make`
* `./node.exe`

# Building with SSL support

To build node with ssl support you need to build OpenSSL first.

* Download OpenSSL from [openssl.org](http://www.openssl.org/source/)
* Untar it. By default the node build script will look for openssl in `..\openssl`, so if you put node in `c:\node` it will expect that openssl is `c:\openssl`.
* Configure it by running `./configure no-shared mingw` from the mingw shell
* `make`
* Do not attempt to `make install`
* `cd` to the node directory
* Reconfigure node without the `--without-ssl` option. 
* If `./configure` does not detect openssl automatically, use the `--openssl-libpath` and `--openssl-includes` options to tell it where to look.
* Rebuild node with `make`

# Known issues

These are known issues with the build process. A list of known issues with mingw-built node can be found found in [TODO.win32](https://github.com/ry/node/raw/master/TODO.win32).

### Git doesn't work from the mingw bash shell
Some people have reported problems getting this to work. If you are affected by this, use git from the windows command prompt.

### Build fails with msvc / Microsoft Visual Studio installed
Having Microsoft Visual Studio installed confuses the V8 build system. This is a know issue, there is currently no good solution for it.

Then you need to fix toolician for scons: 'tools/scons/scons-local-1.2.0/SCons/Tool/'__init__'.py'.

        if str(platform) == 'win32':
            "prefer Microsoft tools on Windows"
            linkers = ['mslink', 'gnulink', 'ilink', 'linkloc', 'ilink32' ]
            c_compilers = ['msvc', 'mingw', 'gcc', 'intelc', 'icl', 'icc', 'cc', 'bcc32' ]

Fix above to following(changing order of c_compilers).

        if str(platform) == 'win32':
            "prefer Microsoft tools on Windows"
            linkers = ['mslink', 'gnulink', 'ilink', 'linkloc', 'ilink32' ]
            c_compilers = ['mingw', 'msvc', 'gcc', 'intelc', 'icl', 'icc', 'cc', 'bcc32' ]