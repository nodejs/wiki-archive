## Install Node only

Run the following (as root):

    echo "deb http://ftp.us.debian.org/debian wheezy-backports main" >> /etc/apt/sources.list
    apt-get update
    apt-get install nodejs

## Install Node and NPM

Run the following (as root):

    echo "deb http://ftp.us.debian.org/debian wheezy-backports main" >> /etc/apt/sources.list
    apt-get update
    apt-get install nodejs-legacy
    curl -L --insecure https://www.npmjs.org/install.sh | bash

### Using it in a Dockerfile

You could use it in your Dockerfile like this:

    FROM debian:wheezy
    MAINTAINER Your Self "yourself@example.com"
    # method from https://github.com/joyent/node/wiki/backports.debian.org
    RUN echo "deb http://ftp.us.debian.org/debian wheezy-backports main" >> /etc/apt/sources.list \
        && DEBIAN_FRONTEND=noninteactive apt-get -y update \
        && apt-get install -y curl nodejs-legacy \
        && curl -L --insecure -O https://www.npmjs.org/install.sh \
        && /bin/bash install.sh \

[An example of such Dockerfile using this](https://registry.hub.docker.com/u/tezcatl/nodemon/dockerfile/)