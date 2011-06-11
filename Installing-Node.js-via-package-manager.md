## Debian
[Node.js is available in official repo for Debian Wheezy(testing) and Debian Sid(unstable)](http://packages.debian.org/search?searchon=names&keywords=nodejs).  
For Debian Squeeze:

    root@host: ~ # echo deb http://ftp.us.debian.org/debian/ sid main > /etc/apt/sources.list.d/sid.list
    root@host: ~ # apt-get update
    root@host: ~ # apt-get install nodejs # Documentation is great.

## Ubuntu

Example install:

    sudo apt-get install python-software-properties
    sudo add-apt-repository ppa:jerome-etienne/neoip
    sudo apt-get update
    sudo apt-get install nodejs

It installs current stable nodejs on the current stable ubuntu.

## On openSUSE BuildService
[Node.js stable repos list](http://bit.ly/nodejs_repos).

Available RPM packages for: CentOS 5; Fedora 13; openSUSE 11.3 and Factory.

Example install on openSUSE 11.3:

    sudo zypper ar http://download.opensuse.org/repositories/home:/SannisDev/openSUSE_11.3/ SannisDevBuildService 
    sudo zypper in nodejs nodejs-devel

## Arch Linux
Node.js [stable](https://aur.archlinux.org/packages.php?ID=32930) and [unstable](https://aur.archlinux.org/packages.php?ID=44279) are both available in the [AUR](http://aur.archlinux.org/).

Example install using [packer](https://aur.archlinux.org/packages.php?ID=33378):

    packer -S nodejs

Also, npm (nodejs package manager) is available as an arch package. It can be installed through packer

    packer -S nodejs-npm

or through yaourt (another aur tool):

    yaourt -S nodejs-npm

## OSX
Using [a package](https://sites.google.com/site/nodejsmacosx)

> Simply [download](https://sites.google.com/site/nodejsmacosx) and double-click.

Using [homebrew](https://github.com/mxcl/homebrew):

    brew install node

Using [macports](http://www.macports.org/):

    port install nodejs