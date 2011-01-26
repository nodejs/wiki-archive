Building node.js on Cygwin (Windows)
====

**NOTE: v0.2.6 and v0.3.1 build on Cygwin. The latests versions do not.**



This tutorial will guide you through setting up the latest stable version of node.js on Cygwin. You don't need to have a working Cygwin install.

1. Grab and install [Cygwin](http://www.cygwin.com/).
2. Using `setup.exe` from Cygwin (1), install the following packages required to compile node.js:

   * `devel  → gcc-g++`
   * `devel  → git`
   * `devel  → make`
   * `devel  → openssl`
   * `libs   → openssl-devel`
   * `devel  → pkg-config`
   * `devel  → zlib-devel`
   * `python → python`

   You may also want to install the following:

   * `editors → vim` or `editors → nano` for (4) below
   * `web → curl` if you wish to install [[npm|https://github.com/isaacs/npm]], node's package manager

   You can use the search box at the top-left to locate packages quickly.

2. It's time to clone the Git repository and build node.js. Start a new Cygwin shell (bash, zsh, etc.), open `Start → Cygwin → Cygwin Bash Shell`. Run the following commands:

       $ cd ~
       $ git clone git://github.com/ry/node.git
       $ cd node
       $ git fetch --all
       # if the above fails complaining --all is not recognised, try: git fetch origin
       $ git tag
       $ git checkout [latest stable tag from previous command, e.g., v0.2.5]
       $ ./configure
       $ make
       $ make install

   It is recommended you checkout a stable tag since most of the time building **master** fails.
   If you receive an error at any of the above steps, look further down for possible solutions.

3. Set up Domain Name Resolution (DNS)

    Cygwin internally uses Windows for DNS queries. node.js uses the c-ares library that relies on `/etc/resolv.conf`. Cygwin ships with an empty `/etc/resolv.conf`. In order to enabled networking from your scripts, add these IPs to the file (Google Public DNS):

       $ vim /etc/resolv.conf

       nameserver 8.8.8.8
       nameserver 8.8.4.4

For Vim newbies:  use `i` to enter insert mode, `<Esc>:wq` to exit insert mode, enter the command window, **w**rite and **q**uit. If you are uncomfortable with Vim, use `nano /etc/resolv.conf` instead.

Build Problems
====

python.exe: Can't Open File 'waf-light':
----

    C:\Program Files\Python26\python.exe: can't open file '/home/stan/node/tools/waf-light': [Errno 2] No such file or directory

This is not an issue with node.js. You are using the Windows version of `python` in Cygwin. It doesn't know how to process Cygwin style path names. You need to remove the Windows version (or make sure is not in the `PATH`) and install the Cygwin version of Python using `setup.exe`.

    # Update PATH before running ./configure for node.js
    export PATH=/usr/bin:$PATH

Unable to Remap to Same Address as Parent
----

    fatal error – unable to remap \\?\C:\cygwin\lib\python2.6\lib-dynload\time.dll to same address as parent: 0×360000 != 0×3E0000

This is not an issue with node.js either. Install `base → rebase` using `setup.exe` first then close all Cygwin instances. Start `dash` or `ash` (located in the `bin` directory under Cygwin's installation) and run:

    $ /bin/rebaseall -v

It should finish with no errors. If instead the above results in an error like:

    rebaseall:'/cygdrive/c/Users/ADMINI~1/AppData/Local/Temp' is not writable

Open up a Cygwin shell and run:
   
    chmod 777 ~/AppData/Local/Temp

Close your shell window and repeat the steps above.

Once you are done, restart your PC. Remember to close all open Cygwin shells before using `rebaseall`.

../deps/v8/tools/jsmin.py SyntaxError: invalid syntax
----

Cygwin uses a different way of handling symlinks than a regular Unix system. If you cloned node.js using msysGit it's likely you'll end up here as well. Open up `jsmin.py` in an editor – you will see the actual path it needs to be symlinked to. To address this, start a new Cygwin shell and `cd` to the cloned node.js directory. Run:

    # from the node.js cloned repository directory, run:
    $ cd tools
    $ ln -fs `cat jsmin.py`
    $ cd ..

Re-run `./configure`.

Exception: supported architectures are arm, ia32, x64 but NOT 'x86'.
----

Cygwin is returning the wrong CPU architecture (usually `uname` from minGw gets in the way). Use the `dest-cpu` flag with the value of `ia32`:

    $ ./configure --dest-cpu=ia32

Error: prototype for ‘v8::internal::Sampler::Sampler(int, bool)’ does not match any in class ‘v8::internal::Sampler’
----
This is a v8 issue. To resolve this:

Download and apply this patch for [deps/v8/src/platform-cygwin.cc](http://nodejs.googlegroups.com/attach/8c24ccb4b209572f/cygwin_033.patch?gda=3gXBKEcAAADJGyyZmWU5Ct7DEbvXm_oaF2J3Emxhh_MkFLdNUXdQOhhcOFKy5Im8CsmuOI0s2BYbQwFxJw55cVwemAxM-EWmeV4duv6pDMGhhhZdjQlNAw&view=1&part=4)

    cd node-v0.3.3/
    git apply ./cygwin_033.patch

Build failed:  -> task failed (err #2): 	{task: libv8.a SConstruct -> libv8.a}
----
This comes about when $SHELL has a windows based path instead of  a unix based one.  E.g., C:\bin\bash instead of of /bin/bash. So, try, 
    export SHELL=/bin/bash
and re-run make.


Help! I've done EVERYTHING above and I'm still having issues…
====

If you have followed all of the steps above and you still need further help, pop into the [#node.js IRC channel](http://webchat.freenode.net?channels=node.js) on `irc.freenode.net`.
