The packages on this page are maintained and supported by their respective packagers, **not** the node.js core team.  Please report any issues you encounter to the package maintainer.  If it turns out your issue is a bug in node.js itself, the maintainer will report the issue upstream.

## Gentoo
Node.js is available in the portage tree.

    # emerge nodejs

## Debian, LMDE
[Node.js is available in official repo for Debian Sid(unstable)](http://packages.debian.org/search?searchon=names&keywords=nodejs).

For Debian Wheezy, your best bet is to compile node by yourself:

    sudo apt-get install python g++ make checkinstall
    mkdir ~/src && cd $_
    wget -N http://nodejs.org/dist/node-latest.tar.gz
    tar xzvf node-latest.tar.gz && cd node-v*
    ./configure
    checkinstall #(remove the "v" in front of the version number in the dialog)
    sudo dpkg -i node_*

Uninstall:

    sudo dpkg -r node

In case you get a permission denied on the `node` executable, an alternative path might be:

    umask 0022
    ./configure
    make
    checkinstall -D --umask 0022 --reset-uids --install=no
    dpkg -i node_*.deb


## Ubuntu, Mint, Elementary OS
At least for Ubuntu 13.04, Node is in the standard repository. To install, just run:

    sudo apt-get install nodejs

Obtaining the latest version of Node or installing on older Ubuntu and other apt-based distributions may require a few extra steps. Example install:

    sudo apt-get update
    sudo apt-get install python-software-properties python g++ make
    sudo add-apt-repository ppa:chris-lea/node.js
    sudo apt-get update
    ## Your version may be different. Look for "Version:" in /var/lib/apt/lists/ppa.launchpad.net_chris-lea_node.js_[...]_Packages (ellipsised part of path varies with setup)
    sudo apt-get install nodejs=0.10.18-1chl1~precise1
    # @@ Why does one need to specify the install version? @@

It installs current stable Node on the current stable Ubuntu. Quantal (12.10) users may need to install the *software-properties-common* package for the `add-apt-repository` command to work: `sudo apt-get install software-properties-common`

As of Node.js v0.10.0, the nodejs package from [Chris Lea](https://chrislea.com/2013/03/15/upgrading-from-node-js-0-8-x-to-0-10-0-from-my-ppa/)'s repo includes both npm and nodejs-dev.

There is a naming conflict with the node package (Amateur Packet Radio Node Program), and the nodejs binary has been renamed from `node` to `nodejs`. You'll need to symlink `/usr/bin/node` to `/usr/bin/nodejs` or you could uninstall the Amateur Packet Radio Node Program to avoid that conflict.

## openSUSE & SLE
[Node.js stable repos list](https://build.opensuse.org/package/show?package=nodejs&project=devel%3Alanguages%3Anodejs). Also node.js is available in openSUSE:Factory repository.

Available RPM packages for: openSUSE 11.4, 12.1, Factory and Tumbleweed; SLE 11 (with SP1 and SP2 variations).

Example install on openSUSE 12.1:

    sudo zypper ar http://download.opensuse.org/repositories/devel:/languages:/nodejs/openSUSE_12.1/ NodeJSBuildService 
    sudo zypper in nodejs nodejs-devel

## Fedora

[Node.js](https://apps.fedoraproject.org/packages/nodejs) and [npm](https://apps.fedoraproject.org/packages/npm) are available in Fedora 18 and later.  Just use your favorite graphical package manager or run this on a terminal to install both npm and node:

    sudo yum install npm

## RHEL/CentOS/Scientific Linux 6

Node.js and npm are available from the [Fedora Extra Packages for Enterprise Linux (EPEL)](https://fedoraproject.org/wiki/EPEL) repository.  If you haven't already done so, first [enable EPEL](https://fedoraproject.org/wiki/EPEL#How_can_I_use_these_extra_packages.3F).

To check if you have EPEL, run 

    yum repolist

if you don't see epel, download it (At the time of this writing, the last version is 6.8.)

    curl -O http://download-i2.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm

then install it

    sudo rpm -ivh epel-release-6-8.noarch.rpm


And then run the following command to install node and npm:

    sudo yum install npm

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