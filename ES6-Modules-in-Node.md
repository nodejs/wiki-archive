A wiki page to summarise the latest happenings at: https://github.com/nodejs/node-eps/pull/3


## Specification

Glossary:

- CJS Module System refers to CJS's `require` and `module.exports` / `exports`
- ES Module System refers to ES6's `import` and `export`
- CJS Modules are considered files that use CJS's Module System exclusively
- ES Modules are considered files that use ES6's Module System exclusively

Requirements:

- Ability for CJS Module System to load both CJS Modules and ES Modules
- Ability for ES Module System to load both CJS Modules and ES Modules
- Ability to require another package without knowing which module system it uses
- Ability to publish and require packages that have files of both ES Modules and CJS Modules


## FAQ

### Why support ES Modules in node at all?

Still unclear.

### Should/will the ES Module System be able to import specific files, e.g. `import thing from 'package/lib/thing.js'`

Still unclear.

### Is there any plans to support files that use both module systems (aka CJS+ES Modules)?

Still unclear.

### `How can node identify whether a file is a CJS module or an ES6 module?` why does it even need to do this?

Why does there need to be two modes? Why can't there be a single file mode? Why can't every file be assumed to support both formats?

If it is constrained that import/require is interoperable between CJS and ES6, what does it take for export/module.exports to also be interoperable?

Still unclear.

Attempted questions on this:

- https://github.com/nodejs/node-eps/pull/3#issuecomment-184962845
- https://github.com/nodejs/node-eps/pull/3#issuecomment-185522878

Attempted answers:

- https://github.com/nodejs/node-eps/pull/3#issuecomment-184964610
- https://github.com/nodejs/node-eps/pull/3#issuecomment-185499502
- https://github.com/nodejs/node-eps/pull/3#issuecomment-185514226
- https://github.com/nodejs/node-eps/pull/3#issuecomment-185523778


## Detection Problem

This section serves to address the available methods for addressing  the question "How can node identify whether a file is a CJS module or an ES6 module?"

References:

- https://github.com/nodejs/node-eps/pull/3#issuecomment-184466200
- https://github.com/nodejs/node-eps/pull/3#issuecomment-184368922

### Option 1: In-Source Pragma

Details: This option will require users to add `"use module";` or something similar at the top of every file.

Cons:

- Unacceptable boilerplate tax on users _(this seems to be a very strong reason)_
- Implementers will have a hard time implementing a double parser considering that modules source text and javascript source text are parsed differently.


### Option 2: New file extension for ES6 Modules

Details: This option will require users to use .jsm or something similar as the file extension for any ES6 module. Likely to be used exclusively (will break older node versions) or to be used coupled with a .js file (e.g. `project/hello.js` and `project/hello.jsm` both exist, doing `require('project/hello')` will prefer .jsm file if it exists otherwise will use .js file).

Cons:

- Forces distinction on source files programmers haven't had to before (different start non-terminals or contextual expectations haven't led to different extensions in the past)

- It's solved in the browser by out-of-band configuration (`<script type="module">` and loader hooks to control the detection in users-land)

- Large distributed migration cost imposed on tools and consequently developers for a number of years; obstacle to adoption
hard to estimate how many things in the world depend on JavaScript === *.js; how many .htaccess files, config files, scripts, etc. break?

- It is a refactor hazard for popular modules containing more than one file, it will break existing dependents that are using .js extension in their require calls (e.g.: `require('foo/bar.js')`)

- Combinatorial explosion of JS extensions like .jsx and .ts (e.g.: how to signal that it is a module with typescript annotations?)

- Best-case success story is that .jsm becomes the new extension for JS, which is a loss; plausible story is that modules end up sometimes in .js and sometimes in .jsm

- Node shouldn't presume to solve a problem for other tools
- Many have already solved this problem without requiring a new extension

References:

- https://github.com/nodejs/node-eps/pull/3#issuecomment-184901008


### Option 3: Content-Sniffing in Node Semantics

Details: Engines providing a way to detect the type by inspection of the code.

Cons:

- Implementers unlikely to implement a parsing API that auto-detects the mode _(this is a very strong reason)_
- Implementing this is non-trivial
- Likely hurt the performance of importing

References

- https://github.com/nodejs/node-eps/pull/3#issuecomment-184847011

### Option 4: Meta in `package.json`

Cons:

- Node allows requiring files that are not part of packages, e.g. `require('C:/a.js')` where `C:/package.json` doesn't exist - another very edge case


#### Option 4a: Single Module Entry Point

Details: `package.json` has a `"main"` for CJS Module entry and `"module"` for ES6 Module entry.

``` javascript
{
  // ...
  "module": "lib/index.js",
  "main": "old/index.js",
  // ...
}
```

Note: `"module"` seems like a better option than our previous proposal `"jsnext:main"`, which is being used today in many modules.

Pros:

- Harmonizes with `<script type="module">`
- Allows simultaneous new and old entry point
- Replaces `"main"` (which eventually becomes "the old way") with an ergonomic standard way

Cons:

- Requires external method for specifying Module System of non-entry points, e.g. `require('lib/something-else.js')` - which kind of defeats the purpose of this option


#### Option 4b: Modules Listing

Details: `package.json` has a `modules` property that is an array of files or directories that use ES Modules.

``` javascript
{
  // ...
  // files:
  "modules": ["lib/hello.js", "bin/hello.js"],

  // directories:
  "modules": ["lib", "bin"],

  // files and directories:
  "modules": ["lib", "bin", "special.js"],

  // if package never uses CJS Modules
  "modules": ["."],
}
```

Pros:

- Solves requiring an ES Module that was not the entry point

Notes:

- Requires external solution for determining the entry point

Updates:

- Updated this proposal from the original files only regex approach, to an array of files and directories as it is cleaner, doesn't require regex knowledge, _and can actually be serialised to JSON!_ ~ @balupton

References:

- https://github.com/nodejs/node-eps/pull/3#issuecomment-184363319
- https://github.com/nodejs/node-eps/pull/3#issuecomment-184885213


#### Option 4c: Edition Listing

Details: `package.json` has a `"editions"` property that lists which editions the package contains, and what their syntaxes are.

``` javascript
{
  "editions": [{
    // source that contains special features that get compiled away
    "syntax": ["ESNext", "ES Modules", "Flow Type", "JSX"],
    "entry": "source/index.js"
  }, {
    // compiled for future browser and node support, only has confirmed upcoming ES features
    "syntax": ["ESNext", "ES Modules"],
    "entry": "esnext-esm/index.js"
  }, {
    // compiled for current browsers and node 0.12, 4, and 5 support
    "syntax": ["ES2015", "ES Modules"],
    "entry": "es2015-esm/index.js"
  }, {
    // compiled for current browsers and node 0.12, 4, and 5 support
    // uses babel-plugin-add-module-exports package to convert ES Modules to CJS Modules
    "syntax": ["ES2015", "CJS Modules"],
    "entry": "es2015-cjs/index.js"
  }, {
    // compiled with babel preset 2015-loose for node 0.10 and IE8 support without need for external es2015 polyfills
    // uses babel-plugin-add-module-exports package to convert ES Modules to CJS Modules
    "syntax": ["ES5", "CJS Modules"],
    "entry": "es5-cjs/index.js"
  }]
}
```

Pros:

- Entry point is selected by which edition the current environment supports, rather than just which edition uses ES Modules or CJS Modules (which may still make use of a feature that is not currently supported by the environment)

- For systems that only support `"main"` entry point, the package author can specify `"main"` to go to either to:

  - The ES2015 & CJS Module edition, e.g. `"main": "es2015-cjs/index.js"`
  - To a CJS Module script that automatically detects the correct edition to load

- Allows future node-core, parsers, bundlers, and compilers to intelligently decide which edition they can support either by built-in support into themselves, or via a `"main"` script that does the same detection

- Allow meta generators like [projectz](https://github.com/bevry/projectz) to automatically generate README documentation on which editions the package provides

- Solves the problem of bundlers like rollup, browserify, webpack re-compiling already compiled ES5 or ES2015 editions rather than the latest edition that is supported by that system and compiling down to the desired target

Notes:

- Requires external method for specifying Module System of non-entry points, e.g. `require('lib/something-else.js')`, however this is easily solved by any of these:
  - Adding a `"directory"` property to each direction, which would work well, however no way to use ES Modules outside of edition directories

  - Coupling with any other proposal.
  - E.g. Coupling with the Modules Listing Proposal would work well and wouldn't require additional tooling or compiler updates, e.g.
  
    ``` javascript
    {
      "modules": ["source", "esnext-esm", "es2015-esm", "non-edition-file.js"],
      "editions": [ /* ... */ ]
    }
    ```

  - E.g. Coupling with the JSM Extension Proposal would allow for the `-esm` and `-cjs` prefixes to go away, as each edition would support both CJS Modules and ES Modules:

    ``` javascript
    {
      "editions": [{
        // source that contains special features that get compiled away
        "syntax": ["ESNext", "Flow Type", "JSX"],
        "entry": "source/index.js"
      }, {
        // compiled for future browser and node support, only has confirmed upcoming ES features
        // uses some babel plugin package to also output .jsm files along with the .js files
        "syntax": ["ESNext"],
        "entry": "esnext/index"
      }, {
        // compiled for current browsers and node 0.12, 4, and 5 support
        // uses some babel plugin package to also output .jsm files along with the .js files
        "syntax": ["ES2015"],
        "entry": "es2015/index"
      }, {
        // compiled with babel preset 2015-loose for node 0.10 and IE8 support without need for external es2015 polyfills
        // uses some babel plugin package to also output .jsm files along with the .js files
        "syntax": ["ES5"],
        "entry": "es5-cjs/index"
      }]
    }
    ```

References:

- https://github.com/nodejs/node-eps/pull/3#issuecomment-185592052

Implementations:

- https://github.com/bevry/editions