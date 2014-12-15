# Note: These instructions will apply to v0.12 and following.
- An exception is the [Chromium ICU](#building-using-chromiums-icu) instructions which are already relevant to node.
- [ ] TODO: remove the above notice when https://github.com/joyent/node/pull/7719 is in master
- [ ] TODO: as of https://github.com/joyent/node/pull/8719 the option `--with-intl=none` will be the default, AND ICU will be downloaded automatically if need be. Need to reword the wiki here. 

# What is `Intl`?
The `Intl` object is available when [EcmaScript 402](http://www.ecma-international.org/ecma-402/1.0/)
support is enabled.
Node.js uses [ICU4C](http://icu-project.org) to implement the `Intl` object natively.

# Unix/Macintosh: Using a pre-built ICU (system-icu)

The Intl package can use an ICU which is already built.  
## Installing a pre-built ICU
Here are some ways to obtain a pre-built ICU:

### From a package manager

Run one of these or similar as appropriate for your system:

     apt-get install libicu-dev
     yum install libicu-dev

### Building ICU yourself

* Download ICU source
  [http://icu-project.org/download](http://icu-project.org/download)

* Follow the enclosed `readme.html` to build ICU, particularly paying attention to the `--prefix` argument

* build ICU and then:

     make install

## Verify an installed ICU

     pkg-config --modversion icu-i18n

If this command fails, node will not be able to find the installed ICU. 
Verify that the `PKG_CONFIG_PATH` points to the newly installed `icu-i18n.pc` file

## Configure node

     ./configure --with-intl=system-icu

* Download the latest
  [icu4c-**##.#**-src.tgz](http://icu-project.org/download) (or `.zip`)
* Unpack the source as `deps/icu` (you should have `deps/icu/source/...`)

# Building node with an embedded ICU

## Download ICU source
First: Unpack latest ICU
  [icu4c-**##.#**-src.tgz](http://icu-project.org/download) (or `.zip`)
  as `deps/icu` (You'll have: `deps/icu/source/...`)

## Unix/Macintosh

* ./configure ... --with-intl=full-icu

Or, to build the "small" variant (English only):

* ./configure ... --with-intl=small-icu

## Windows

* vsbuild.bat ... `full-icu`

Or, to build the "small" variant (English only):

* vsbuild.bat ... `small-icu`

# Using the "small" build

   * If you use the "small-icu" option (the default),
     you can provide additional data at runtime.
      * Two methods:
        * The `NODE_ICU_DATA` env variable:   `env
          NODE_ICU_DATA=/some/path node`
        * The `--icu-data-dir` parameter:   `node
          --icu-data-dir=/some/path`
      * Example:  If you use the path `/some/path`, then ICU 53 on
        Little Endian (l) finds:
        * individual files such as `/some/path/icudt53l/root.res`
        * a packaged data file `/some/path/icudt53l.dat`
      * Notes:
        * See `u_setDataDirectory()` and
        [the ICU Users Guide](http://userguide.icu-project.org/icudata)
        for many more details.
        * "53l" will be "53b" on a big endian machine.
   * Note that this option is also useful for [updating ICU's time zone data](http://userguide.icu-project.org/datetime/timezone#TOC-Update-the-time-zone-data-for-ICU4C).

# Verifying an `Intl` build
- [btest402.js](https://github.com/srl295/btest402) is a very basic test of whether `Intl` is built correctly.

# Using `Intl` build
- See: [EcmaScript 402](http://www.ecma-international.org/ecma-402/1.0/)
- Tutorial: [Working with Intl](http://code.tutsplus.com/tutorials/working-with-intl--cms-21082)

# Building using Chromium's ICU

Note: this build is missing some locales, is an older revision, and has a larger output size.
It is included here for completeness.

    svn checkout --force --revision 214189 \
        http://src.chromium.org/svn/trunk/deps/third_party/icu46 \
        deps/v8/third_party/icu46
    ./configure --with-icu-path=deps/v8/third_party/icu46/icu.gyp
    make
    make install

# FAQ
* none (yet!)