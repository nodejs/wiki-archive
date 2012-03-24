# Uninstalling Node

## OSX (Might work for Linux, too)
```
which node
```
This returns something like ```/path/to/node```

```
cd /path/
rm -r bin/node bin/node-waf include/node lib/node lib/pkgconfig/nodejs.pc share/man/man1/node.1
