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

### Known Issues

  If you receive an error during `./configure` like this
  
```
File "/home/flo/node-v0.6.6/tools/waf-light", line 157, in <module>
     import Scripting
File "/home/flo/node-v0.6.6/tools/wafadmin/Scripting.py", line 146
     except Utils.WafError, e:
                          ^
SyntaxError: invalid syntax
```

   it is because Python3 is your default Python version. To fix this issue you have to set Python2 temporary as your default Python:

```
export PYTHON=`which python2`
```

Maybe you need to change your PYTHONHOME as well. If you have any further installation problems stop into [#node.js](http://webchat.freenode.net/?channels=node.js&uio=d4) on irc.freenode.net and ask questions.

## GNU/Linux and other UNIX

Do something like this

```
tar -zxf node-v0.6.7.tar.gz
cd node-v0.6.7
./configure
make
sudo make install
```

Or, if you'd like to install from the repository

```
git clone https://github.com/joyent/node.git
cd node
git checkout v0.6.7
./configure
make
sudo make install
```

You may wish to install Node in a custom folder instead of a global directory. 

    ./configure --prefix=/opt/node
    make
    sudo make install

You may want to put the node executables in your path as well for easier use. Add this line to your `~/.profile` or `~/.bash_profile` or `~/.bashrc` or `~/.zshenv`

    export PATH=$PATH:/opt/node/bin

If you have SpiderMonkey installed, you may have some some conflicting includes. Set `CXXFLAGS="-I./deps/v8/src"` before building to prioritize the v8 files over SpiderMonkey's.


## Windows


You just need 'node.msi'. Click node.msi , and then click 'next' ...