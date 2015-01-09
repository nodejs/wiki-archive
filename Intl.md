# What is `Intl`?
[EcmaScript 402](http://www.ecma-international.org/ecma-402/1.0/) describes
the global `Intl` (short for Internationalization) object
and other related functions and functionality.

Node.js (or more properly, the v8 engine) uses [ICU4C](http://icu-project.org)
to implement this `Intl` support in native C/C++. ICU's source is not
included with Node's source repository or source distributions.

There are multiple ways to build Node with ICU.
This page applies to v0.12 and following.

# Build with a specific ICU:

You can find other ICU releases at
[the ICU homepage](http://icu-project.org/download).
Download the file named something like `icu4c-**##.#**-src.tgz` (or
`.zip`).

Unix/Macintosh: from an already-unpacked ICU

```sh
./configure --with-intl=[small-icu,full-icu] --with-icu-source=/path/to/icu
```

Unix/Macintosh: from a local ICU tarball

```sh
./configure --with-intl=[small-icu,full-icu] --with-icu-source=/path/to/icu.tgz
```

Unix/Macintosh: from a tarball URL

```sh
./configure --with-intl=[small-icu,full-icu] --with-icu-source=http://url/to/icu.tgz
```

Windows: first unpack latest ICU to `deps/icu`
  [icu4c-**##.#**-src.tgz](http://icu-project.org/download) (or `.zip`)
  as `deps/icu` (You'll have: `deps/icu/source/...`)

```sh
vcbuild.bat small-icu|full-icu
```

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

   * If you use the "small-icu" option,
     you can provide additional data at runtime.
      * Two methods:
        * The `NODE_ICU_DATA` env variable:   `env
          NODE_ICU_DATA=/some/path node`
        * The `--icu-data-dir` parameter:   `node
          --icu-data-dir=/some/path`
      * Example:  If you use the path `/some/path`, then ICU 53 on
        Little Endian (l) finds:
        * individual files such as `/some/path/icudt53l/root.res`
        * a packaged data file `/some/path/icudt53l.dat` such as from http://apps.icu-project.org/datacustom/
      * Notes:
        * See `u_setDataDirectory()` and
        [the ICU Users Guide](http://userguide.icu-project.org/icudata)
        for many more details.
        * "53l" will be "53b" on a big endian machine.
   * With the `small-icu` mode, you can also choose different locales than "English only". For example,
       `--with-icu-locales=de,zh,fr` will include only German, Chinese and French but not English.
       The http://apps.icu-project.org/datacustom/ page will list currently available locale IDs.
   * Note that this option is also useful for [updating ICU's time zone data](http://userguide.icu-project.org/datetime/timezone#TOC-Update-the-time-zone-data-for-ICU4C).

# Verifying an `Intl` build
- `node test/simple/test-intl.js`
- [btest402.js](https://github.com/srl295/btest402) is a very basic but verbose test of whether `Intl` is built correctly.

# Using `Intl` build
- See: [EcmaScript 402](http://www.ecma-international.org/ecma-402/1.0/) and http://jsi18n.com/
- Tutorial: [Working with Intl](http://code.tutsplus.com/tutorials/working-with-intl--cms-21082)

# Updating Timezone data

From the ICU documentation:

> Time zone data changes often in response to governments around the world changing their local rules and the
> areas where they apply. ICU derives its tz data from the IANA Time Zone Database.
>
> The ICU project publishes updated timezone resource data in response to IANA updates, and these can be used to
> patch existing ICU installations. Several update strategies are possible, depending on the ICU version and
> configuration.

As the [ICU Userguide](http://userguide.icu-project.org/datetime/timezone#TOC-ICU4C-TZ-Update-with-Drop-in-.res-files-ICU-54-and-newer-) states, it is possible to update time zone data (when ICU 54 and following is used) by:
* setting the `ICU_TIMEZONE_FILES_DIR` variable to point to some directory, such as `/timezones`
* Download the `.res` files from the appropriate subdirectory of [the ICU  TZ site](http://source.icu-project.org/repos/icu/data/trunk/tzdata/icunew/) (from the `44/le` directory for little endian machines or the `44/be` directory for big endian machines) to the `/timezones` directory
* On node's next restart, it will use the `.res` files from the `ICU_TIMEZONE_FILES_DIR` variable to get the latest timezone data.

# Building using Chromium's ICU

Note: not recommended. This build is missing some locales, is an older revision,
and has a larger output size. It is included here for completeness.

    svn checkout --force --revision 214189 \
        http://src.chromium.org/svn/trunk/deps/third_party/icu46 \
        deps/v8/third_party/icu46
    ./configure --with-icu-path=deps/v8/third_party/icu46/icu.gyp
    make
    make install
