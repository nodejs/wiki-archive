Cygwin is no longer supported, despite being [POSIX](http://en.wikipedia.org/wiki/Posix) compliant. The latest version that compiles is 0.4.12. 

```
wget http://nodejs.org/dist/node-v0.4.12.tar.gz
tar xvfz node-v0.4.12.tar.gz
cd node-v0.4.12/
./configure
make
make install
```

Source: http://stackoverflow.com/a/10114528/148844

Update: some people have had success building Node.js 0.10 on MinGW+MSYS. Source: http://opensourcepack.blogspot.co.uk/2013/06/nodejs-with-posix-path-support.html