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
    curl https://www.npmjs.org/install.sh | sh
