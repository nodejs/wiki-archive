A wiki page to summarise the latest happenings at: https://github.com/nodejs/node-eps/pull/3


## Specification

Glossary:

- CJS Module System refers to CJS `require` and `module.exports` / `exports`
- ES Module System refers to ES6 `import` and `export`
- CJS Modules are considered files that use CJS's Module System exclusively
- ES Modules are considered files that use ES6's Module System exclusively

Requirements:

- Ability for CJS Module System to load both CJS Modules and ES Modules
- Ability for ES Module System to load both CJS Modules and ES Modules
- Ability to require another package without knowing which module system it uses
- Ability to publish and require packages that have files of both ES Modules and CJS Modules


## FAQ

### Is there any plans to support files that use both module systems (aka CJS+ES Modules)?

Still open.


### `How can node identify whether a file is a CJS module or an ES6 module?` why does it even need to do this?

Still open.



## Detection Problem

This section serves to address the available methods for addressing  the question "How can node identify whether a file is a CJS module or an ES6 module?"


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


### Option 3: Content-Sniffing in Node Semantics

Details: Engines providing a way to detect the type by inspection of the code.

Cons:

- Implementers unlikely to implement a parsing API that auto-detects the mode _(this is a very strong reason)_
- Implementing this is non-trivial
- Likely hurt the performance of importing


### Option 4: Meta in `package.json`: Single Entry Point

Details: `package.json` can have main and module to signal where are the entry points for CJS and ES6 Module.

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

- Hard to detect the type of other files in the package without extra hinting or extra resolution process (e.g. `require('foo/lib/something.js')`)

- Node allows to require arbitrary files without requiring a package.json in place (another very edge case)
