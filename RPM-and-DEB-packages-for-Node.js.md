## Debian
[Node.js is available in official repo for Debian Sqeeze and Debian Sid](http://packages.debian.org/search?searchon=names&keywords=nodejs).

## Ubuntu

Example install:

    sudo add-apt-repository ppa:jerome-etienne/neoip
    sudo apt-get update
    sudo apt-get install nodejs

## On openSUSE BuildService
* [Node.js v0.2.x repos list](http://bit.ly/nodejs_repos)
* [Node.js v0.3.x repos list](http://bit.ly/nodejs3_repos)

Available RPM packages for: CentOS 5; Fedora 12 and 13; openSUSE 11.2, 11.3 and factory.
Available DEB packages of Node.js v0.2.x for: Debian 5.0; Ubuntu 9.10 and 10.04.

Example install on openSUSE 11.3:

    sudo zypper ar http://download.opensuse.org/repositories/home:/SannisDev/openSUSE_11.3/ SannisDevBuildService 
    sudo zypper in nodejs nodejs-devel