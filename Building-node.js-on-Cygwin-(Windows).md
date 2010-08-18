Building node.js on Cygwin (Windows)
====

1. Install [Cygwin](http://www.cygwin.com/) and use `setup.exe` to install the following required packages: 

   * `devel  -> gcc-g++`
   * `devel  -> git`
   * `devel  -> make`
   * `devel  -> openssl`
   * `devel  -> pkg-config`
   * `python -> python`
 
2. Clone the repository and build node.js. Open `Start -> Cygwin -> Cygwin Bash Shell` and run the following commands:

       $ cd ~
       $ git clone git://github.com/ry/node.git
       $ cd node
       $ git fetch --all
       $ git tag
       $ git checkout [latest-stable-tag]
       $ ./configure
       $ make
       $ make install

   It is recommended you checkout a stable tag since most of the time building **master** on Cygwin fails.

3. Set up Domain Name Sesolution (DNS)

    Cygwin internally uses Windows for DNS queries. node.js uses the c-ares library that relies on `/etc/resolv.conf`. Cygwin ships with an empty `/etc/resolv.conf`. In order to enabled networking from your scripts, add these IPs to the file (Google Public DNS):

       $ vim /etc/resolv.conf

       nameserver 8.8.8.8
       nameserver 8.8.4.4

Build Problems
====

python.exe: can't open file 'waf-light':
----

This is not an issue with node.js. You are using the Windows version of `python` in Cygwin. It doesn't know how to process Cygwin style path names. You need to remove the Windows version (or make sure is not in the `PATH`) and install the Cygwin version of Python using `setup.exe`.

    # Full error details
    C:\Program Files\Python27\python.exe: can't open file '/cygdrive/c/node/tools/waf-light': [Errno 2] No such file or directory

unable to remap to same address as parent
----

This is not an issue with node.js either. Install `base -> rebase` first then close all Cygwin instances. Using `dash` or `ash` as a shell run:

    $ /bin/rebaseall -v

It will tell you what to do further.

    # Full error details
    fatal error – unable to remap \\?\C:\cygwin\lib\python2.6\lib-dynload\time.dll to same address as parent: 0×360000 != 0×3E0000

Help! I've done EVERYTHING above and I'm still having trouble
====

If you have tried all of the above steps and you still need some help, pop into the [#node.js chat channel](http://webchat.freenode.net?channels=node.js) on irc.freenode.net.
