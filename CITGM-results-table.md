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
| Test Name                                   | Duration     | Age |
|---------------------------------------------|--------------|-----|
| citgm.ftp-v0.3.10                           | 3.9 sec      | 5   |
| citgm.winston-v2.3.1                        | 15 sec       | 5   |
| citgm.spdy-v3.4.4                           | 15 sec       | 6   |
| citgm.yeoman-generator-v1.1.1               | 1 min 20 sec | 6   |

### nodes=fedora23
| Test Name                                   | Duration     | Age |
|---------------------------------------------|--------------|-----|
| citgm.ffi-v2.2.0                            | 16 sec       | 2   |
| citgm.watchify-v3.9.0                       | 30 sec       | 5   |
| citgm.winston-v2.3.1                        | 17 sec       | 5   |
| citgm.spdy-v3.4.4                           | 18 sec       | 7   |
| citgm.yeoman-generator-v1.1.1               | 1 min 38 sec | 7   |

### nodes=osx1010
| Test Name                                   | Duration     | Age |
|---------------------------------------------|--------------|-----|
| citgm.watchify-v3.9.0                       | 1 min 5 sec  | 7   |
| citgm.spdy-v3.4.4                           | 26 sec       | 7   |
| citgm.winston-v2.3.1                        | 22 sec       | 7   |
| citgm.yeoman-generator-v1.1.1               | 2 min 17 sec | 7   |
| citgm.ffi-v2.2.0                            | 1 min 44 sec | 7   |

### nodes=aix61-ppc64
| Test Name                                   | Duration     | Age |
|---------------------------------------------|--------------|-----|
| citgm.lodash-v4.17.4                        | 10 min       | 1   |
| citgm.ftp-v0.3.10                           | 25 sec       | 5   |

### nodes=ppcle-ubuntu1404
| Test Name                                   | Duration     | Age |
|---------------------------------------------|--------------|-----|
| citgm.spdy-transport-v2.0.18                | 18 sec       | 1   |
| citgm.spdy-v3.4.4                           | 13 sec       | 7   |

### nodes=debian8-64
| Test Name                                   | Duration     | Age |
|---------------------------------------------|--------------|-----|
| citgm.ftp-v0.3.10                           | 4.1 sec      | 1   |
| citgm.ffi-v2.2.0                            | 12 sec       | 1   |
| citgm.watchify-v3.9.0                       | 24 sec       | 5   |
| citgm.winston-v2.3.1                        | 17 sec       | 5   |
| citgm.spdy-v3.4.4                           | 16 sec       | 7   |
| citgm.yeoman-generator-v1.1.1               | 1 min 20 sec | 7   |

### nodes=win-vs2015
| Test Name                                   | Duration     | Age |
|---------------------------------------------|--------------|-----|
| citgm.bluebird-v3.5.0                       | 6.1 sec      | 1   |
| citgm.watchify-v3.9.0                       | 47 sec       | 5   |
| citgm.coffee-script-v1.12.6                 | 38 sec       | 17  |
| citgm.acorn-v5.0.3                          | 53 sec       | 53  |
| citgm.esprima-v3.1.3                        | 2 min 10 sec | 58  |

### nodes=ubuntu1204-64
| Test Name                                   | Duration     | Age |
|---------------------------------------------|--------------|-----|
| citgm.winston-v2.3.1                        | 16 sec       | 5   |
| citgm.spdy-v3.4.4                           | 17 sec       | 7   |
| citgm.yeoman-generator-v1.1.1               | 1 min 21 sec | 7   |

### nodes=fedora22
| Test Name                                   | Duration     | Age |
|---------------------------------------------|--------------|-----|
| citgm.watchify-v3.9.0                       | 27 sec       | 5   |
| citgm.winston-v2.3.1                        | 16 sec       | 5   |
| citgm.spdy-v3.4.4                           | 22 sec       | 7   |
| citgm.yeoman-generator-v1.1.1               | 1 min 38 sec | 7   |

### nodes=ppcbe-ubuntu1404
| Test Name                                   | Duration     | Age |
|---------------------------------------------|--------------|-----|
| citgm.spdy-v3.4.4                           | 15 sec       | 7   |
| citgm.leveldown-v1.7.0                      | 26 sec       | 9   |

### nodes=ubuntu1604-64
| Test Name                                   | Duration     | Age |
|---------------------------------------------|--------------|-----|
| citgm.winston-v2.3.1                        | 18 sec       | 5   |
| citgm.spdy-v3.4.4                           | 19 sec       | 7   |
| citgm.yeoman-generator-v1.1.1               | 1 min 33 sec | 7   |
| citgm.readable-stream-v2.2.9                | 1 min 23 sec | 96  |

### nodes=rhel72-s390x
| Test Name                                   | Duration     | Age |
|---------------------------------------------|--------------|-----|
| citgm.ftp-v0.3.10                           | 2 sec        | 5   |
| citgm.winston-v2.3.1                        | 10 sec       | 5   |
| citgm.spdy-v3.4.4                           | 9.9 sec      | 7   |