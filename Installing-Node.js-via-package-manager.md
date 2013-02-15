## Gentoo
Node.js is available in official gentoo portage tree. You have to unmask it.

    # emerge -aqv --autounmask-write nodejs
    # etc-update
    # emerge -aqv nodejs

## Debian
[Node.js is available in official repo for Debian Sid(unstable)](http://packages.debian.org/search?searchon=names&keywords=nodejs).

For Debian Squeeze, your best bet is to compile node by yourself (as `root`):

    apt-get install python g++ make
    mkdir ~/nodejs && cd $_
    wget -N http://nodejs.org/dist/node-latest.tar.gz
    tar xzvf node-latest.tar.gz && cd `ls -rd node-v*`
    ./configure
    make install

## Linux Mint

[How to install node.js on Debian-based Linux distros (Debian, Ubuntu, Mint etc)](http://oodavid.tumblr.com/post/15090798307/how-to-install-node-js-on-linux)

## Ubuntu

Example install:

    sudo apt-get install python-software-properties python g++ make
    sudo add-apt-repository ppa:chris-lea/node.js
    sudo apt-get update
    sudo apt-get install nodejs npm

It installs current stable Node on the current stable Ubuntu. Quantal (12.10) users may need to install the *software-properties-common* package for the `add-apt-repository` command to work: `sudo apt-get install software-properties-common`

If you want to compile Node C++ modules:

    sudo apt-get install nodejs-dev

Or, use the configure shell script from <http://apptob.org/> for installing node.js.

## openSUSE & SLE
[Node.js stable repos list](https://build.opensuse.org/package/show?package=nodejs&project=devel%3Alanguages%3Anodejs). Also node.js is available in openSUSE:Factory repository.

Available RPM packages for: openSUSE 11.4, 12.1, Factory and Tumbleweed; SLE 11 (with SP1 and SP2 variations).

Example install on openSUSE 12.1:

    sudo zypper ar http://download.opensuse.org/repositories/devel:/languages:/nodejs/openSUSE_12.1/ NodeJSBuildService 
    sudo zypper in nodejs nodejs-devel

## RHEL, Fedora etc

Node.js is currently being implemented in Fedora 18 and will become a regular part of the distribution starting with Fedora 19.

To try the unstable 0.9.x series with Fedora 18 right now, run:

    sudo yum --enablerepo=updates-testing install nodejs npm

## Arch Linux
Node.js is available in the Community Repository.

    pacman -S nodejs

## FreeBSD and OpenBSD
Node.js is available through the ports system.

    /usr/ports/www/node

Development versions are also available using ports 

    cd /usr/ports/www/node-devel/ && make install clean

or packages on FreeBSD

    pkg_add -r node-devel

## OSX
Using [a package](http://nodejs.org/#download)

> Simply [download Macintosh Installer](http://nodejs.org/#download).

Using [homebrew](https://github.com/mxcl/homebrew):

    brew install node

Using [macports](http://www.macports.org/):

    port install nodejs  

## Windows
Using [a package](http://nodejs.org/#download)

> Simply [download Windows Installer](http://nodejs.org/#download).

Using [chocolatey](http://chocolatey.org) to install [Node](http://chocolatey.org/packages/nodejs):  

    cinst nodejs  

or for [full install with NPM](http://chocolatey.org/packages/nodejs.install):  

    cinst nodejs.install
