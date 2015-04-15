# Introduction

This document aims at describing what needs to be done when upgrading the version of V8 that is included in Node.js' source tree.

## Floating patches

* https://github.com/joyent/node/commit/6ebd85e10535dfaa9181842fe73834e51d4d3e6c. See https://github.com/joyent/node/pull/9439 for more information about why it actually does not fix the original problem. Still, we might want to keep it for now for consistency
* https://github.com/joyent/node/commit/3589a62104eac1daae782ab479bec09f4df4dc9a.
* https://github.com/v8/v8-git-mirror/commit/2ad2237507c5b5f9047b8d94d2f4997327eae852: fixes issues with using `--debug-brk` with `--use-strict`.
* https://github.com/joyent/node/commit/ac21a5390c6e02f0bc9052c7794dcabc33a0a0c3. However the commit message needs to be changed to something like: `mdb_v8: fix dictionary properties access`.
* https://github.com/joyent/node/commit/2320210639516025a5ad0744e8ff45b687212422: allows to not lose IRHydra support.
* https://github.com/joyent/node/commit/1314cfe4760744de2bebe3e54abdf957c57caeae.

### Patches that need to be upstreamed

* https://github.com/joyent/node/issues/9147.

## Testing

### Node.js tests suite

The first step is to run the standard Node.js tests suite. The best way to do that is to push the branch that contains the V8 upgrade to joyent/node __in a feature branch (something like v8-upgrade)__ and to run the the [node-review-unix Jenkins job](jenkins.nodejs.org/job/node-review-unix) on that branch.

### Testing post-mortem debugging on SmartOS

Post-mortem debugging on SmartOS relies on information generated both by the build process and by V8 at runtime. When V8 changes, sometimes the metadata generated at build time needs to change to match V8's runtime data structures.

In order to test post-mortem debugging on SmartOS, simply build a node binary with the default build options and run the following tests: `test/pummel/test-postmortem-details.js`, `test/pummel/test-postmortem-findjsobjects.js`, `test/pummel/test-postmortem-jsstack.js`. All these tests must exit with a 0 exit code.