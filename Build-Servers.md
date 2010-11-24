# Build / Continuous Integration Servers

## Buildbot

[http://buildbot.nodejs.org:8010/](http://buildbot.nodejs.org:8010/)

Community-run Python-based continuous integration server.  Tests builds against Solaris, Ubuntu, FreeBSD.

## Bamboo

[http://build.nodejs.org/](http://build.nodejs.org/)

Joyent-sponsored continuous integration server using [Atlassian Bamboo](http://www.atlassian.com/software/bamboo/).  Tests builds against Solaris, Ubuntu, Windows/Cygwin.  OS X build hardware is on the way.

Contact [jim@joyent.com](mailto:jim@joyent.com) if you need access or want to help out.

## CDash

[http://my.cdash.org/index.php?project=node](http://my.cdash.org/index.php?project=node)

Test results from CMake builds.

# Bamboo Remote Agent Access

Contact [jim@joyent.com](mailto:jim@joyent.com) if you need access.

## Solaris

* Joyent SmartMachine (OpenSolaris Zone)
* ssh jill@solaris.build.nodejs.org
* built automatically from a script

## Ubuntu

* Ubuntu 10.04 LTS
* KVM-based Virtual Machine hosted by Joyent
* ssh jill@ubuntu.build.nodejs.org

## Windows / Cygwin

* KVM-based Virtual Machine hosted by Joyent
* RDP to connect to Adminstrator on 64.30.133.118
* ssh bamboo@cygwin.build.nodejs.org

