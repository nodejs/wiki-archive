# Building and Installing Node.js

Node has several dependencies.  If you are building from source you should
only need 2 things:

* **python** - version 2.5 or higher. The build tools distributed with
  Node run on python.

* **libssl-dev** - If you plan to use SSL/TLS encryption in your
  networking, you'll need this.  Libssl is the library used in the
  [openssl](http://www.openssl.org/) tool. On Linux and Unix systems
  it can usually be installed with your favorite package manager. The
  lib comes pre- installed on OS X.

## Unix

Do something like this

```
tar -zxf node-v0.6.5.tar.gz
cd node-v0.6.5
./configure
make
sudo make install
```

You may wish to install Node in a custom folder instead of a global directory. 

    ./configure --prefix=/opt/node
    make
    sudo make install

You may want to put the node executables in your path as well for easier use:

    echo 'export PATH=$PATH:/opt/node/bin' >> ~/.profile # ~/.bash_profile or ~/.bashrc on some systems

To reload the path in your current instance of Terminal or bash, use:

    . ~/.profile # Or ~/.bash_profile

If you have any installation problems, look at [Troubleshooting
Installation](https://github.com/ry/node/wiki/Troubleshooting-Installation), try an [alternative installation method](https://gist.github.com/579814), or stop into [#node.js](http://webchat.freenode.net/?channels=node.js&uio=d4) and ask questions.

## Windows

You need python and Microsoft Visual Studio but not OpenSSL. In `cmd.exe` do the following

```
C:\Users\ryan>tar -zxf node-v0.6.5.tar.gz
C:\Users\ryan>cd node-v0.6.5
C:\Users\ryan\node-v0.6.5>vcbuild.bat release
[Wait 20 minutes]
C:\Users\ryan\node-v0.6.5>Release\node.exe
> process.versions
{ node: '0.6.5',
  v8: '3.6.6.11',
  ares: '1.7.5-DEV',
  uv: '0.6',
  openssl: '0.9.8r' }
>
```

The executable will be in `Release\node.exe`.