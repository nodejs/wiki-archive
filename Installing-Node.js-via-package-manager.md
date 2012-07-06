## Gentoo
Node.js is available in official gentoo portage tree. You have to unmask it.

    # emerge -aqv --autounmask-write nodejs
    # etc-update
    # emerge -aqv nodejs

## Debian
[Node.js is available in official repo for Debian Sid(unstable)](http://packages.debian.org/search?searchon=names&keywords=nodejs).

For Debian Squeeze, your best bet is to compile node by yourself (as `root`):

    # apt-get update 
    # apt-get install git-core curl build-essential python openssl libssl-dev
    # mkdir -p /tmp/build/node && cd /tmp/build/node
    # git clone https://github.com/joyent/node.git .
    # git checkout v0.8.1
    # ./configure --openssl-libpath=/usr/lib/ssl
    # make
    # make test
    # make install
    # node -v

This last command should print the version of Node matching the git tag you checked out.

## Linux Mint

[How to install node.js on Debian-based Linux distros (Debian, Ubuntu, Mint etc)](http://oodavid.tumblr.com/post/15090798307/how-to-install-node-js-on-linux)

## Ubuntu

Example install:

    sudo apt-get install python-software-properties
    sudo apt-add-repository ppa:chris-lea/node.js
    sudo apt-get update
    sudo apt-get install nodejs npm

It installs current stable Node on the current stable ubuntu.

If you want to compile Node C++ modules:

    sudo apt-get install nodejs-dev

Or configure shell script for install node.js using http://apptob.org

## openSUSE & SLES
[Node.js stable repos list](https://build.opensuse.org/package/show?package=nodejs&project=devel%3Alanguages%3Anodejs).

Available RPM packages for: openSUSE 11.4, 12.1, Factory and Tumbleweed.

Example install on openSUSE 11.4:

    sudo zypper ar http://download.opensuse.org/repositories/devel:/languages:/nodejs/openSUSE_11.4/ NodeJSBuildService 
    sudo zypper in nodejs nodejs-devel

## Fedora 15 and 16

To install node on **Fedora**:

    sudo yum localinstall --nogpgcheck http://nodejs.tchol.org/repocfg/fedora/nodejs-stable-release.noarch.rpm
    sudo yum install nodejs-compat-symlinks npm

## RHEL/CentOS/Scientific Linux 5 and 6

To install node on **RHEL and friends**:

    wget http://nodejs.tchol.org/repocfg/el/nodejs-stable-release.noarch.rpm
    yum localinstall --nogpgcheck nodejs-stable-release.noarch.rpm
    yum install nodejs-compat-symlinks npm
    rm nodejs-stable-release.noarch.rpm

## Amazon Linux

First install the repository:

    sudo yum localinstall --nogpgcheck http://nodejs.tchol.org/repocfg/amzn1/nodejs-stable-release.noarch.rpm   

Then install the packages:

    sudo yum install nodejs-compat-symlinks npm

## Arch Linux
Node.js is available in the Community Repository.

    pacman -S nodejs

## FreeBSD and OpenBSD
Node.js is available through the ports system.

    /usr/ports/www/node


## OSX
Using [a package](http://nodejs.org/#download)

> Simply [download Macintosh Installer](http://nodejs.org/#download).

Using [homebrew](https://github.com/mxcl/homebrew):

    brew install node

**Warning:** brew installs are known to be buggy

Using [macports](http://www.macports.org/):

    port install nodejs  

## Windows
Using [a package](http://nodejs.org/#download)

> Simply [download Windows Installer](http://nodejs.org/#download).

Using [chocolatey](http://chocolatey.org) to install [Node](http://chocolatey.org/packages/nodejs):  

    cinst nodejs  

or for [full install with NPM](http://chocolatey.org/packages/nodejs.install):  

    cinst nodejs.install