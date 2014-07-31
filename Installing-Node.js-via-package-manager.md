***Note:*** The packages on this page are maintained and supported by their respective packagers, **not** the Node.js core team. Please report any issues you encounter to the package maintainer. If it turns out your issue is a bug in Node.js itself, the maintainer will report the issue upstream.

## Ubuntu, Linux Mint

***And other Ubuntu-based Linux distributions***

Add Chris Lea's repository first before installing to avoid conflicts

```text
sudo add-apt-repository ppa:chris-lea/node.js
sudo apt-get update
```

If "add-apt-repository not found"：

```text
# for <= 12.04
sudo apt-get install python-software-properties
# for >= 12.10
sudo apt-get install software-properties-common
```

From Ubuntu 12.04 to 13.04, an old version (0.6.x) of Node is in the standard repository. For Ubuntu 13.10 and 14.04, 0.10.X versions are present. To install, just run:

```text
sudo apt-get install nodejs
```

To install npm on Ubuntu 13.10 and 14.04, run:

```text
sudo apt-get install npm
```

For programs that still depend on calling the "node" binary, run:

```text
sudo apt-get install nodejs-legacy
```

Obtaining a recent version of Node or installing on older Ubuntu and other apt-based distributions may require a few extra steps. Example install:

```text
sudo apt-get install python-software-properties
sudo add-apt-repository ppa:chris-lea/node.js
sudo apt-get update
sudo apt-get install nodejs
```

It installs current stable Node on the current stable Ubuntu. 12.10 and 13.04 users may need to install the *software-properties-common* package for the `add-apt-repository` command to work: `sudo apt-get install software-properties-common`.

The command `add-apt-repository` is actually provided by the *python-software-properties* package. So you will have to install this one as well `sudo apt-get install python-software-properties`.

As of Node.js v0.10.0, the nodejs package from [Chris Lea](https://chrislea.com/2013/03/15/upgrading-from-node-js-0-8-x-to-0-10-0-from-my-ppa/)'s repo includes both npm and nodejs-dev.

There is a naming conflict with the node package (Amateur Packet Radio Node Program), and the nodejs binary has been renamed from `node` to `nodejs`. You'll need to symlink `/usr/bin/node` to `/usr/bin/nodejs` or you could uninstall the Amateur Packet Radio Node Program to avoid that conflict.

_Optional:_ To avoid using `sudo` for global `npm` installs etc.: `npm config set prefix ~/npm`, add `$HOME/npm/bin` to `$PATH`, and append `export PATH=$HOME/npm/bin:$PATH` to your `.bashrc`.

## Debian and Linux Mint Debian

Node.js is available in [official repo](http://packages.debian.org/search?searchon=names&keywords=nodejs) for Debian Sid (unstable).

For Debian Wheezy, you have two options:

### Build from source

```text
sudo apt-get install python g++ make checkinstall fakeroot
src=$(mktemp -d) && cd $src
wget -N http://nodejs.org/dist/node-latest.tar.gz
tar xzvf node-latest.tar.gz && cd node-v*
./configure
sudo fakeroot checkinstall -y --install=no --pkgversion $(echo $(pwd) | sed -n -re's/.+node-v(.+)$/\1/p') make -j$(($(nproc)+1)) install
sudo dpkg -i node_*
```

#### Uninstall

```text
sudo dpkg -r node
```

### Backports

Alternatively, you can install `nodejs` from [`wheezy-backports`](backports.debian.org). If you rely on having `node` as an executable, install `nodejs-legacy` as well.

If you need `npm` as well, you can get it through the installer

```text
curl https://www.npmjs.org/install.sh | sudo sh
```

## Gentoo

Node.js is available in the portage tree.

```text
emerge nodejs
```

## openSUSE & SLE

[Download Node.js via openSUSE one-click](http://software.opensuse.org/download.html?project=devel%3Alanguages%3Anodejs&package=nodejs).

Available RPM packages for: openSUSE 11.4, 12.1, 12.2, 12.3, 13.1, Factory and Tumbleweed; SLE 11 (with SP1/SP2/SP3 variations).

Example install on openSUSE 13.1:

```text
sudo zypper ar \
  http://download.opensuse.org/repositories/devel:/languages:/nodejs/openSUSE_13.1/ \
  Node.js
sudo zypper in nodejs nodejs-devel
```

## Fedora

[Node.js](https://apps.fedoraproject.org/packages/nodejs) and [npm](https://apps.fedoraproject.org/packages/npm) are available in Fedora 18 and later. Just use your favorite graphical package manager or run this on a terminal to install both npm and node:

```text
sudo yum install nodejs npm
```

## Enterprise Linux (RHEL, CentOS, Fedora, etc.)

Node.js and npm are available from the [Extra Packages for Enterprise Linux](https://fedoraproject.org/wiki/EPEL) (EPEL) repository.

To check if you have EPEL, run

```text
yum repolist
```

If you don't see EPEL, [install it](https://fedoraproject.org/wiki/EPEL#How_can_I_use_these_extra_packages.3F
) via `yum`:

For Enterprise Linux version 6 (EPEL version 6.8 at the time of writing):

```text
yum install \
  http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
```

For Enterprise Linux version 7 Beta:

```text
yum install \
  http://dl.fedoraproject.org/pub/epel/beta/7/x86_64/epel-release-7-0.2.noarch.rpm
```

Then install the **nodejs** and **npm** packages:

```text
sudo yum install nodejs npm --enablerepo=epel
```

## Arch Linux

Node.js is available in the Community Repository.

```text
pacman -S nodejs
```

## FreeBSD and OpenBSD

Node.js is available through the ports system.

```text
/usr/ports/www/node
```

Development versions are also available using ports

```text
cd /usr/ports/www/node-devel/ && make install clean
```

or packages on FreeBSD

```text
pkg_add -r node-devel
```

Using [pkg-ng](https://wiki.freebsd.org/pkgng) on FreeBSD

```text
pkg install node
```

or the development versions

```text
pkg install node-devel
```

## OSX

Simply download the [Macintosh Installer](http://nodejs.org/#download) direct from the [nodejs.org](http://nodejs.org) web site.

### Alternatives

Using **[Homebrew](http://brew.sh/)**:

```text
brew install node
```

Using **[Fink](http://www.finkproject.org)**:

```text
fink install nodejs
```

Using **[MacPorts](http://www.macports.org/)**:

```text
port install nodejs
```

## Windows

Simply download the [Windows Installer](http://nodejs.org/#download) directly from the [nodejs.org](http://nodejs.org) web site.

### Alternatives

Using **[Chocolatey](http://chocolatey.org)**:

```text
cinst nodejs
# or for full install with npm
cinst nodejs.install
```

Using **[Scoop](http://scoop.sh/)**:

```text
scoop install nodejs
```
