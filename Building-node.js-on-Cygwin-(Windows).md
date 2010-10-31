Building node.js on Cygwin (Windows)
====

This tutorial will guide you through setting up the latest stable version of node.js on Cygwin. You don't need to have a working Cygwin install.

1. Install [Cygwin](http://www.cygwin.com/).
2. Use `setup.exe` from Cygwin and install the following packages required to compile node.js:

   * `devel  -> gcc-g++`
   * `devel  -> git`
   * `devel  -> make`
   * `devel  -> openssl`
   * `libs   -> openssl-devel`
   * `devel  -> pkg-config`
   * `devel  -> zlib-devel`
   * `python -> python`

   You can use the search box at the top-left to locate packages quickly.

2. It's time to clone the Git repository and build node.js. Start a new Cygwin shell (bash, zsh, etc.), open `Start -> Cygwin -> Cygwin Bash Shell`. Run the following commands:

       $ cd ~
       $ git clone git://github.com/ry/node.git
       $ cd node
       $ git fetch --all
       $ git tag
       $ git checkout [latest-stable-tag, e.g., v0.2.4]
       $ ./configure
       $ make
       $ make install

   It is recommended you checkout a stable tag since most of the time building **master** on Cygwin fails.
   If you receive an error at any of the above steps, look further down for possible solutions.

3. Set up Domain Name Resolution (DNS)

    Cygwin internally uses Windows for DNS queries. node.js uses the c-ares library that relies on `/etc/resolv.conf`. Cygwin ships with an empty `/etc/resolv.conf`. In order to enabled networking from your scripts, add these IPs to the file (Google Public DNS):

       $ vim /etc/resolv.conf

       nameserver 8.8.8.8
       nameserver 8.8.4.4

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

This is not an issue with node.js either. Install `base -> rebase` using `setup.exe` first then close all Cygwin instances. Using `dash` or `ash` as a shell run ):

    $ /bin/rebaseall -v

Once you are done, restart your PC.

../deps/v8/tools/jsmin.py SyntaxError: invalid syntax

(on vista: 1. close the cygwin shell 2. type cmd in the search box of the start menu. 3. type cd cygwin/bin 4. type ash 5. type ./rebaseall -v)
----

Cygwin uses a different way of handling symlinks than a regular Unix system. If you open up `jsmin.py` in an editor, you will see the actual path it needs to be symlinked to.

    # from the node.js cloned repository directory, run:
    % cd tools
    % ln -fs `cat jsmin.py`

Re-run `./configure`.

Help! I've done EVERYTHING above and I'm still having trouble
====

If you have tried all of the above steps and you still need some help, pop into the [#node.js chat channel](http://webchat.freenode.net?channels=node.js) on irc.freenode.net.
