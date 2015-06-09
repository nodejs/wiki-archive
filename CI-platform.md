# Node.js CI platform

## Overview

The Node.js CI platform is available at http://jenkins.nodejs.org. As its name
suggests, this URL represents a Jenkins server to which Jenkins agents connect
and provide resources to run build and test jobs.

This CI platform is responsible, among other things, for building release
artefacts and integrating changes into the codebase. As a result, it is a
critical piece of the project's infrastructure.

### Current jobs

Currently, the main jobs that are running on this CI platform are:

* [nodejs-release](http://jenkins.nodejs.org/job/nodejs-release/). It produces
Node.js releases' artefacts such as binary and source packages.

* [node-accept-pull-request](http://jenkins.nodejs.org/job/node-accept-pull-request/).
It takes care of integrating any change into joyent/node's repository.

* [node-review](http://jenkins.nodejs.org/job/node-review/). This job is mainly
used when working on a large set of changes to do a pre-validation of some
work before going through the normal contribution process to integrate that
change.

These jobs are usually meta-jobs that contain several sub-jobs. For instance,
the nodejs-release job contains the following sub-jobs: [nodejs-btar](http://jenkins.nodejs.org/job/nodejs-btar/),  [nodejs-indep](http://jenkins.nodejs.org/job/nodejs-indep/) and [nodejs-msi](http://jenkins.nodejs.org/job/nodejs-msi/).

Sometimes, clones of these jobs exist and have slightly different names. Most
of the times the names of these clone jobs use suffixes to differentiate
themselves from the original jobs.

For instance, there's another meta-job named [nodejs-release-
julien](http://jenkins.nodejs.org/job/nodejs-release-julien/) that is a clone
of the [nodejs-release](http://jenkins.nodejs.org/job /nodejs-release/) job. The
difference between the two jobs is that nodejs-release-julien pulls code from
Julien's fork of joyent/node instead of from joyent/node directly, and that it
uploads release artefacts to Julien's account on one of the build machines.

In this case, the same functionality could and should be achieved by using
jobs parameters, but in other cases cloning an existing job is a good way to
experiment with modifications without breaking existing behavior on which
other users of the CI platform rely.

#### nodejs-release

This job is currently used for two use cases:

1. Doing an actual development, stable or maintenance release.

2. Creating release artefacts from any branch. For instance, you might want to
create packages that are easy to install to give them to end users so that
they can test some changes that haven't made it into an official release yet.

It is composed of three distinct sub-jobs:

* [nodejs-btar](http://jenkins.nodejs.org/job/nodejs-btar/) which is responsible
for building binary tarballs for all UNIX platforms currently supported
(SmartOS, Linux and OSX).

* [nodejs-indep](http://jenkins.nodejs.org/job/nodejs-indep/) which creates both
the source tarball and the API documentation tarball.

* [nodejs-msi](http://jenkins.nodejs.org/job/nodejs-msi/) which creates the
Windows installers and binaries.

In order to trigger a build of the nodejs-release job, point your browser to
jenkins.nodejs.org/job/nodejs-release and click on [Build with
parameters](http://jenkins.nodejs.org/job/nodejs-release/build?delay=0sec). On
that page you'll be asked to fill in three text fields that correspond to the
following parameters:

* GIT_BRANCH: the branch of the joyent/node repository to use to checkout the
source code to build.

* INT_SIGN: the identifier of a key to use to sign the OSX binary installer.
_This is not needed because OSX package signing doesn't work on CI machines
currently_.

* APP_SIGN: the identifier of a key to use to sign the OSX application. _This is
not needed because OSX package signing doesn't work on CI machines currently_.

Node that when doing an actuall official release, the creation of the OSX
binary installer must be done from

#### node-accept-pull-request

#### node-review

#### libuv

### Making changes to existing jobs

### Supported platforms

### Build machines

## Administration

### Restarting the Jenkins agent on build machines


### Windows machines administration

To access Windows machines, you will need to connect to them using the Remote
Desktop Protocol (RDP). There are RDP clients for most operating systems.

For other administration tasks, such as resetting platform, you should log in to
the Azure portal.

The informations needed to connect to these machines via RDP or to the Azure
portal should have been sent to you if you're an administrator of the CI
platform. Otherwise, send an email to team@nodejs.org.

### UNIX machines (except OSX)

Administration of UNIX CI machines is done via SSH. All UNIX machines, except OSX ones,
are VMs or containers hosted on Joyent's Public cloud. If you're an
administrator of the CI platform, you must have received a login/password to
access a management dashboard on Joyent's Public Cloud from which you can:

* Manage existing build machines.
* Create new build machines.
* Delete existing build machines. __Be very careful though before delete any
build machine, and make sure you get the approval from the rest of the
administrators of the CI platform before proceeding.__

From this management dashboard, you can also get the public IP address of any
of the build machines and use SSH to get access to a remote shell.

### OSX machines

OSX machines are not hosted in a cloud platform. Instead, they are actual
physical machines with a public IP. Currently, there is only one such machine
and it's hosted at Joyent's offices. To connect to this machine, you will need
to:

1. Connect to Joyent's VPN.
2. Use SSH to get a shell on that machine.

If you're an administrator of the CI platform and you need access to the OSX
build machine, contact team@nodejs.org and all the required information will
be sent to you by email.
