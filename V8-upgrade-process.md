# Introduction

This document aims at describing what needs to be done when upgrading the version of V8 that is included in Node.js' source tree.

## Floating patches

### How to land a patch from upstream V8?

Sometimes, it is necessary to cherry-pick changes made to newer V8 versions upstream and land them in the current V8 version used in `deps/v8`. In order to do that, you'll need to do two things:

1. Cherry-pick the commit from V8's GitHub mirror into node's repository.
2. Change the commit message to mention that it's a back-port from V8 upstream, and add the appropriate metadata.

#### Cherry-picking commits from V8's GitHub mirror

Clone V8's GitHub mirror: `git clone git@github.com:v8/v8-git-mirror.git`.
Identify the commit to backport, and within your node local repository, use the following command:
```
git --git-dir=../../v8-git-mirror/.git format-patch -k -1 --stdout sha1 | git am -k --directory=deps/v8
```
where `../../v8-git-mirror/` is the path to V8's GitHub mirror relative to your local node repository, and `sha1` is the sha of the commit you want to cherry-pick.

At this point, the original commit from V8 should be committed in your local tree. It's time to change the commit message with `git commit --amend` to alter the first line and add in the body that this commit is a back-port from V8.

A good example of such a commit message is https://github.com/joyent/node/commit/2fc5eeb3da0cbc5c17674818bc679d3d564d0d06. Note that in this commit message, the following lines:
```
PR-URL: #9200
Reviewed-by: Trevor Norris <trev.norris@gmail.com>
Reviewed-by: Julien Gilli <julien.gilli@joyent.com>
```
were added by the Jenkins job that merges changes, you don't need to add that yourself.

### Existing floating patches
* https://github.com/joyent/node/commit/3589a62104eac1daae782ab479bec09f4df4dc9a: `build: fix build for SmartOS`.
* https://github.com/v8/v8-git-mirror/commit/2ad2237507c5b5f9047b8d94d2f4997327eae852: fixes issues with using `--debug-brk` with `--use-strict`.
* https://github.com/joyent/node/commit/ac21a5390c6e02f0bc9052c7794dcabc33a0a0c3. However the commit message needs to be changed to something like: `mdb_v8: fix dictionary properties access`.
* https://github.com/joyent/node/commit/2320210639516025a5ad0744e8ff45b687212422: allows to not lose IRHydra support.
* https://github.com/joyent/node/commit/1314cfe4760744de2bebe3e54abdf957c57caeae: `deps: fix postmortem-metadata generator in v8`.

### Patches that need to be upstreamed

* https://github.com/joyent/node/issues/9147: `fix to dictionary properties access`.

## Testing

### Node.js tests suite

The first step is to run the standard Node.js tests suite. The best way to do that is to push the branch that contains the V8 upgrade to joyent/node __in a feature branch (something like v8-upgrade)__ and to run the the [node-review-unix Jenkins job](jenkins.nodejs.org/job/node-review-unix) on that branch.

### Testing post-mortem debugging on SmartOS

Post-mortem debugging on SmartOS relies on information generated both by the build process and by V8 at runtime. When V8 changes, sometimes the metadata generated at build time needs to change to match V8's runtime data structures.

In order to test post-mortem debugging on SmartOS, simply build a node binary with the default build options and run the following tests: `test/pummel/test-postmortem-details.js`, `test/pummel/test-postmortem-findjsobjects.js`, `test/pummel/test-postmortem-jsstack.js`. All these tests must exit with a 0 exit code.