# 2017-05-21

| Configuration Name     | Duration    | All | Failed | Skipped |
|------------------------|-------------|-----|--------|---------|
| nodes=ubuntu1404-64    | 51 min      | 80  | 3      | 3       |
| nodes=fedora23         | 1 hr 0 min  | 80  | 3      | 2       |
| nodes=osx1010          | 1 hr 10 min | 80  | 3      | 2       |
| nodes=aix61-ppc64      | 5 hr 50 min | 76  | 2      | 14      |
| nodes=ppcle-ubuntu1404 | 50 min      | 78  | 1      | 8       |
| nodes=debian8-64       | 50 min      | 80  | 3      | 2       |
| nodes=win-vs2015       | 1 hr 20 min | 66  | 3      | 12      |
| nodes=ubuntu1204-64    | 58 min      | 80  | 2      | 5       |
| nodes=fedora22         | 1 hr 7 min  | 80  | 3      | 3       |
| nodes=ppcbe-ubuntu1404 | 47 min      | 78  | 2      | 7       |
| nodes=ubuntu1604-64    | 57 min      | 80  | 3      | 3       |
| nodes=rhel72-s390x     | 22 min      | 78  | 2      | 11      |

## Failed Tests

### nodes=ubuntu1404-64
| Test Name                     | Duration     | Age |
|-------------------------------|--------------|-----|
| citgm.ftp-v0.3.10             | 3.5 sec      | 10  |
| citgm.spdy-v3.4.4             | 14 sec       | 11  |
| citgm.yeoman-generator-v1.1.1 | 1 min 21 sec | 11  |

### nodes=fedora23
| Test Name                     | Duration     | Age |
|-------------------------------|--------------|-----|
| citgm.ffi-v2.2.0              | 15 sec       | 7   |
| citgm.spdy-v3.4.4             | 19 sec       | 12  |
| citgm.yeoman-generator-v1.1.1 | 1 min 39 sec | 12  |

### nodes=osx1010
| Test Name                     | Duration     | Age |
|-------------------------------|--------------|-----|
| citgm.spdy-v3.4.4             | 20 sec       | 12  |
| citgm.yeoman-generator-v1.1.1 | 1 min 44 sec | 12  |
| citgm.ffi-v2.2.0              | 2 min 35 sec | 12  |

### nodes=aix61-ppc64
| Test Name                     | Duration     | Age |
|-------------------------------|--------------|-----|
| citgm.browserify-v14.3.0      | 9 min 46 sec | 1   |
| citgm.ftp-v0.3.10             | 18 sec       | 10  |

### nodes=ppcle-ubuntu1404
| Test Name                     | Duration     | Age |
|-------------------------------|--------------|-----|
| citgm.spdy-v3.4.4             | 13 sec       | 12  |

### nodes=debian8-64
| Test Name                     | Duration     | Age |
|-------------------------------|--------------|-----|
| citgm.ffi-v2.2.0              | 11 sec       | 1   |
| citgm.spdy-v3.4.4             | 15 sec       | 12  |
| citgm.yeoman-generator-v1.1.1 | 1 min 21 sec | 12  |

### nodes=win-vs2015
| Test Name                     | Duration     | Age |
|-------------------------------|--------------|-----|
| citgm.coffee-script-v1.12.6   | 36 sec       | 22  |
| citgm.acorn-v5.0.3            | 50 sec       | 58  |
| citgm.esprima-v3.1.3          | 2 min 4 sec  | 63  |

### nodes=ubuntu1204-64
| Test Name                     | Duration     | Age |
|-------------------------------|--------------|-----|
| citgm.spdy-v3.4.4             | 18 sec       | 12  |
| citgm.yeoman-generator-v1.1.1 | 1 min 29 sec | 12  |

### nodes=fedora22
| Test Name                     | Duration     | Age |
|-------------------------------|--------------|-----|
| citgm.ffi-v2.2.0              | 24 sec       | 1   |
| citgm.spdy-v3.4.4             | 18 sec       | 12  |
| citgm.yeoman-generator-v1.1.1 | 1 min 36 sec | 12  |

### nodes=ppcbe-ubuntu1404
| Test Name                     | Duration     | Age |
|-------------------------------|--------------|-----|
| citgm.spdy-v3.4.4             | 15 sec       | 12  |
| citgm.leveldown-v1.7.0        | 30 sec       | 14  |

### nodes=ubuntu1604-64
| Test Name                     | Duration     | Age |
|-------------------------------|--------------|-----|
| citgm.spdy-v3.4.4             | 21 sec       | 12  |
| citgm.yeoman-generator-v1.1.1 | 1 min 28 sec | 12  |
| citgm.readable-stream-v2.2.9  | 1 min 30 sec | 101 |

### nodes=rhel72-s390x
| Test Name                     | Duration     | Age |
|-------------------------------|--------------|-----|
| citgm.ftp-v0.3.10             | 2.4 sec      | 10  |
| citgm.spdy-v3.4.4             | 9.2 sec      | 12  |