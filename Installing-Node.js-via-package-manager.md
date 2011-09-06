## Debian
[Node.js is available in official repo for Debian Sid(unstable)](http://packages.debian.org/search?searchon=names&keywords=nodejs).  
For Debian Squeeze:

    root@host: ~ # echo deb http://ftp.us.debian.org/debian/ sid main > /etc/apt/sources.list.d/sid.list
    root@host: ~ # apt-get update
    root@host: ~ # apt-get install nodejs # Documentation is great.

## Ubuntu

Example install:

    sudo apt-get install python-software-properties
    sudo add-apt-repository ppa:chris-lea/node.js
    sudo apt-get update
    sudo apt-get install nodejs

It installs current stable nodejs on the current stable ubuntu.

If you want to compile nodejs C++ modules:

    sudo apt-get install nodejs-dev

Or configure shell script install for node.js http://apptob.org

## On openSUSE BuildService
[Node.js stable repos list](http://bit.ly/nodejs_repos).

Available RPM packages for: CentOS 5; Fedora 13; openSUSE 11.3; openSUSE 11.4 and Factory.

Example install on openSUSE 11.4:

    sudo zypper ar http://download.opensuse.org/repositories/home:/SannisDev/openSUSE_11.4/ SannisDevBuildService 
    sudo zypper in nodejs nodejs-devel

## Fedora
Sadly, Fedora does not yet provide a package for Node.js, but it is under review https://bugzilla.redhat.com/show_bug.cgi?id=732552 

## Arch Linux
Node.js is available in the Community Repository.

    pacman -S nodejs

## OSX
Using [a package](https://sites.google.com/site/nodejsmacosx)

> Simply [download](https://sites.google.com/site/nodejsmacosx) and double-click.

Using [homebrew](https://github.com/mxcl/homebrew):

    brew install node

Using [macports](http://www.macports.org/):

    port install nodejs  

## Windows
 Note: Windows builds are not yet satisfactorily stable but it is possible to get something running.  
  
Using [chocolatey](https://github.com/chocolatey/chocolatey/wiki):  

    cinst nodejs  