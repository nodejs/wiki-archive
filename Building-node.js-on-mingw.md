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

# Known issues

These are known issues with the build process. A list of known issues with mingw-built node can be found found in [TODO.win32](https://github.com/ry/node/raw/master/TODO.win32).

### Git doesn't work from the mingw bash shell
Some people have reported problems getting this to work. If you are affected by this, use git from the windows command prompt.

### Build fails with msvc / Microsoft Visual Studio installed
Having Microsoft Visual Studio installed confuses the V8 build system. This is a know issue, there is currently no good solution for it.

Then you need to fix toolician for scons: 'tools/scons/scons-local-1.2.0/SCons/Tool/__init__.py'.

        if str(platform) == 'win32':
            "prefer Microsoft tools on Windows"
            linkers = ['mslink', 'gnulink', 'ilink', 'linkloc', 'ilink32' ]
            c_compilers = ['msvc', 'mingw', 'gcc', 'intelc', 'icl', 'icc', 'cc', 'bcc32' ]

Fix above to following(changing order of c_compilers).

        if str(platform) == 'win32':
            "prefer Microsoft tools on Windows"
            linkers = ['mslink', 'gnulink', 'ilink', 'linkloc', 'ilink32' ]
            c_compilers = ['mingw', 'msvc', 'gcc', 'intelc', 'icl', 'icc', 'cc', 'bcc32' ]


### ./configure can't find openssl
Linking with openssl is currently unsupported. Use `./configure --without-ssl`.

There is few openssl packages working on win32. But it is difference about linker options.
Below is a workaround that I tried.

1. Open editor to modify 'wscript'.
2. Comment out 262-264. and add following.

        conf.env["USE_OPENSSL"] = Options.options.use_openssl = True
        conf.env.append_value("CPPFLAGS", "-DHAVE_OPENSSL=1")

3. configure & mingw32-make. You'll get fail to link.
4. Open editor to modify 'build/c4che/default.cache.py'. And Change LIB and LIB_OPENSSL.

  I succeeded to compile nodejs-SSL on mingw32 with following.

    LIB = ['ws2_32', 'winmm', 'pthread.dll', 'ssl32']
    LIBPATH_ST = '-L%s'
    LIB_CEIL = ['m']
    LIB_DL = ['dl']
    LIB_OPENSSL = ['crypto', 'gdi32']

