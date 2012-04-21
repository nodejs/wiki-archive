## Debian
[Node.js is available in official repo for Debian Sid(unstable)](http://packages.debian.org/search?searchon=names&keywords=nodejs).  
For Debian Squeeze:

    root@host: ~ # echo deb http://ftp.us.debian.org/debian/ sid main > /etc/apt/sources.list.d/sid.list
    root@host: ~ # apt-get update
    root@host: ~ # apt-get install nodejs # Documentation is great.

You may want to read about [Apt pinning](http://wiki.debian.org/AptPreferences) to limit the impact of using
Sid packages on a Squeeze systems. (Sid breaks toys)

## Ubuntu

Example install:

    sudo apt-get install python-software-properties
    sudo add-apt-repository ppa:chris-lea/node.js
    sudo apt-get update
    sudo apt-get install nodejs npm

It installs current stable Node on the current stable ubuntu.

If you want to compile Node C++ modules:

    sudo apt-get install nodejs-dev

Or configure shell script for install node.js using http://apptob.org

## On openSUSE BuildService
[Node.js stable repos list](https://build.opensuse.org/project/show?project=devel%3Alanguages%3Anodejs).

Available RPM packages for: openSUSE 11.4, 12.1 and Factory.

Example install on openSUSE 11.4:

    sudo zypper ar http://download.opensuse.org/repositories/devel:/languages:/nodejs/openSUSE_11.4/ NodeJSBuildService 
    sudo zypper in nodejs nodejs-devel

## Fedora, RHEL/CentOS/Scientific Linux, and Amazon Linux
Packages for Fedora 15 and 16, RHEL, CentOS, and Scientific Linux 5 and 6, as well as Amazon Linux are available from the [Node.js for Fedora and Enterprise Linux repository](http://nodejs.tchol.org/).

To install node and npm on **Fedora**:

    sudo yum localinstall --nogpgcheck http://nodejs.tchol.org/repocfg/fedora/nodejs-stable-release.noarch.rpm
    sudo yum install nodejs-compat-symlinks npm

To install node and npm on **RHEL and friends**:

    wget http://nodejs.tchol.org/repocfg/el/nodejs-stable-release.noarch.rpm
    yum localinstall --nogpgcheck nodejs-stable-release.noarch.rpm
    yum install nodejs-compat-symlinks npm

To install node and npm on **Amazon Linux**:

    sudo yum localinstall --nogpgcheck http://nodejs.tchol.org/repocfg/amzn1/nodejs-stable-release.noarch.rpm
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

Using [macports](http://www.macports.org/):

    port install nodejs  

## Windows
Using [a package](http://nodejs.org/#download)

> Simply [download Windows Installer](http://nodejs.org/#download).

Using [chocolatey](http://chocolatey.org) to install [Node](http://chocolatey.org/packages/nodejs):  

    cinst nodejs  

or for [full install with NPM](http://chocolatey.org/packages/nodejs.install):  

    cinst nodejs.install
