If you encounter the following error on linux:

    error: could not configure a cxx compiler!

Run:

    sudo aptitude install build-essential

To permanently add node to the path in RHEL/Fedora, add it to your .bashrc by running:

    echo 'export PATH=$HOME/local/node/bin:$PATH' >> ~/.bashrc

If you have trouble building with the directions given try this (mac & maybe linux/bsd)  

in the root directory of the source code:  

    ./configure  
    make  
    sudo make install  

`configure` is currently broken for some versions of MacOS; for more details, see [How to compile Node.js v0.4.2 on MacOS 10.5.8](http://canonical.org/~kragen/compiling-node-on-macos.html). The working approach cited there is as follows:

    export PATH=/Developer/usr/bin:$PATH
    ISYSROOT="-isysroot /Developer/SDKs/MacOSX10.5.sdk"
    export LINKFLAGS=$ISYSROOT CXXFLAGS=$ISYSROOT CFLAGS=$ISYSROOT
    ./configure --prefix=$HOME --without-ssl
    make

Something in the libv8 build process seems to fail erratically when using ccache (on OS X with gcc4, anyway), so if you see errors building accessors.o, try removing ccache from your path.
