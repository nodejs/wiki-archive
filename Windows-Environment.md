# Windows Environment

Some npm modules require compilation of C++ native code. To be able to install such modules, a few additional steps are required after installing node:

1. Install [VC++ Build Tools Technical Preview](https://www.microsoft.com/en-us/download/details.aspx?id=49983)
> :bulb: [Windows 7 only] requires [.NET Framework 4.5.1](http://www.microsoft.com/en-us/download/details.aspx?id=40773)

2. Install [Python 2.7](https://www.python.org/downloads/), and add it to your `PATH` (by selecting the option in the installer or manually afterwards)
3. Launch cmd and run:
  * `npm config set python python2.7`
  * `npm config set msvs_version 2015 --global`

For more information, visit https://github.com/Microsoft/nodejs-guidelines/blob/master/windows-environment.md

## Useful resources

* https://github.com/Microsoft/nodejs-guidelines
