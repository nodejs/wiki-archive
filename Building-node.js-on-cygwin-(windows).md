I could install 0.10.18 on cygwin. "--dest-os=linux" was needed. 

```
curl -O http://nodejs.org/dist/v0.10.18/node-v0.10.18.tar.gz
tar xvfz node-v0.10.18.tar.gz
cd node-v0.10.18/
./configure --dest-os=linux
make
make install
```

----

some people have had success building Node.js 0.10.x on MinGW+MSYS.  
  http://opensourcepack.blogspot.co.uk/2013/06/nodejs-with-posix-path-support.html