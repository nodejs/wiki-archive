# 2017-05-20 #2

| Configuration Name     | Duration    | All | Failed | Skipped |
|------------------------|-------------|-----|--------|---------|
| nodes=ubuntu1404-64    | 1 hr 9 min  | 80  | 4      | 3       |
| nodes=fedora23         | 59 min      | 80  | 6      | 1       |
| nodes=osx1010          | 1 hr 13 min | 80  | 5      | 2       |
| nodes=aix61-ppc64      | 5 hr 29 min | 76  | 1      | 12      |
| nodes=ppcle-ubuntu1404 | 52 min      | 78  | 1      | 8       |
| nodes=debian8-64       | 50 min      | 80  | 2      | 2       |
| nodes=win-vs2015       | 1 hr 40 min | 66  | 4      | 12      |
| nodes=ubuntu1204-64    | 1 hr 0 min  | 80  | 3      | 4       |
| nodes=fedora22         | 1 hr 8 min  | 80  | 4      | 1       |
| nodes=ppcbe-ubuntu1404 | 46 min      | 78  | 2      | 7       |
| nodes=ubuntu1604-64    | 54 min      | 80  | 3      | 4       |
| nodes=rhel72-s390x     | 23 min      | 78  | 2      | 10      |

## Failed Tests

### nodes=ubuntu1404-64
| Test Name                     | Duration     | Age |
|-------------------------------|--------------|-----|
| citgm.serialport-v4.0.7       | 1 min 0 sec  | 2   |
| citgm.ftp-v0.3.10             | 12 sec       | 9   |
| citgm.spdy-v3.4.4             | 20 sec       | 10  |
| citgm.yeoman-generator-v1.1.1 | 1 min 51 sec | 10  |

### nodes=fedora23
| Test Name                     | Duration     | Age |
|-------------------------------|--------------|-----|
| citgm.node-sass-v4.5.3        | 1 min 51 sec | 2   |
| citgm.ffi-v2.2.0              | 14 sec       | 6   |
| citgm.watchify-v3.9.0         | 29 sec       | 9   |
| citgm.winston-v2.3.1          | 16 sec       | 9   |
| citgm.spdy-v3.4.4             | 19 sec       | 11  |
| citgm.yeoman-generator-v1.1.1 | 1 min 40 sec | 11  |

### nodes=osx1010
| Test Name                     | Duration     | Age |
|-------------------------------|--------------|-----|
| citgm.watchify-v3.9.0         | 52 sec       | 11  |
| citgm.spdy-v3.4.4             | 21 sec       | 11  |
| citgm.winston-v2.3.1          | 22 sec       | 11  |
| citgm.yeoman-generator-v1.1.1 | 2 min 6 sec  | 11  |
| citgm.ffi-v2.2.0              | 32 sec       | 11  |

### nodes=aix61-ppc64
| Test Name                     | Duration     | Age |
|-------------------------------|--------------|-----|
| citgm.ftp-v0.3.10             | 29 sec       | 9   |

### nodes=ppcle-ubuntu1404
| Test Name                     | Duration     | Age |
|-------------------------------|--------------|-----|
| citgm.spdy-v3.4.4             | 16 sec       | 11  |

### nodes=debian8-64
| Test Name                     | Duration     | Age |
|-------------------------------|--------------|-----|
| citgm.spdy-v3.4.4             | 15 sec       | 11  |
| citgm.yeoman-generator-v1.1.1 | 1 min 23 sec | 11  |

### nodes=win-vs2015
| Test Name                     | Duration     | Age |
|-------------------------------|--------------|-----|
| citgm.sax-v1.2.2              | 1 min 33 sec | 1   |
| citgm.coffee-script-v1.12.6   | 39 sec       | 21  |
| citgm.acorn-v5.0.3            | 57 sec       | 57  |
| citgm.esprima-v3.1.3          | 2 min 30 sec | 62  |

### nodes=ubuntu1204-64
| Test Name                     | Duration     | Age |
|-------------------------------|--------------|-----|
| citgm.ftp-v0.3.10             | 3.9 sec      | 2   |
| citgm.spdy-v3.4.4             | 17 sec       | 11  |
| citgm.yeoman-generator-v1.1.1 | 1 min 32 sec | 11  |

### nodes=fedora22
| Test Name                     | Duration     | Age |
|-------------------------------|--------------|-----|
| citgm.watchify-v3.9.0         | 30 sec       | 9   |
| citgm.winston-v2.3.1          | 17 sec       | 9   |
| citgm.spdy-v3.4.4             | 23 sec       | 11  |
| citgm.yeoman-generator-v1.1.1 | 1 min 43 sec | 11  |

### nodes=ppcbe-ubuntu1404
| Test Name                     | Duration     | Age |
|-------------------------------|--------------|-----|
| citgm.spdy-v3.4.4             | 15 sec       | 11  |
| citgm.leveldown-v1.7.0        | 29 sec       | 13  |

### nodes=ubuntu1604-64
| Test Name                     | Duration     | Age |
|-------------------------------|--------------|-----|
| citgm.spdy-v3.4.4             | 16 sec       | 11  |
| citgm.yeoman-generator-v1.1.1 | 1 min 28 sec | 11  |
| citgm.readable-stream-v2.2.9  | 1 min 21 sec | 100 |

### nodes=rhel72-s390x
| Test Name                     | Duration     | Age |
|-------------------------------|--------------|-----|
| citgm.ftp-v0.3.10             | 2.4 sec      | 9   |
| citgm.spdy-v3.4.4             | 9.7 sec      | 11  |