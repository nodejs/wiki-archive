## Debian
[Node.js is available in official repo for Debian Wheezy(testing) and Debian Sid(unstable)](http://packages.debian.org/search?searchon=names&keywords=nodejs).  
For Debian Squeeze:

    root@host: ~ # echo deb http://ftp.us.debian.org/debian/ sid main > /etc/apt/sources.list.d/sid.list
    root@host: ~ # /usr/bin/aptitude update;/usr/bin/aptitude safe-upgrade # Nice idea to alias this to 'cu'
    root@host: ~ # aptitude install nodejs # Documentation is great.

## Ubuntu

Example install:

    sudo apt-get install python-software-properties
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

## Arch Linux
Node.js [stable](https://aur.archlinux.org/packages.php?ID=32930) and [unstable](https://aur.archlinux.org/packages.php?ID=44279) are both available in the [AUR](http://aur.archlinux.org/).

Example install using [packer](https://aur.archlinux.org/packages.php?ID=33378):

    packer -S nodejs

## OSX
Using [homebrew](https://github.com/mxcl/homebrew):

    brew install node

Using [macports](http://www.macports.org/):

    port install nodejs