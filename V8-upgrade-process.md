# Introduction

This document aims at describing what needs to be done when upgrading the version of V8 that is included in Node.js' source tree.

## Introduction to V8's development process

### Git repositories

V8 has two (at least, maybe more) Git repositories:

1. The reference repository at https://chromium.googlesource.com/v8/v8.git.
2. A mirror on GitHub at https://github.com/v8/v8-git-mirror/.

While using the GitHub mirror may seem more convenient, it lacks some important data such as `remotes/branch-heads/*` branches that help to determine what is the current stable version for a given release cycle.

### Release cycle

V8 releases happen predictably every six weeks and branch names works somewhat similar to Linux kernel versions:
3.29 -> 3.30 -> 4.1 -> 4.2 -> 4.3 -> ... Every one of those branches, a while after having been created, becomes the stable branch.
3.x -> 4.x -> 5.x happens irregularly every couple of years when they feel like it, and is just naming.

The current stable, beta and canary versions for each support platform can be found here: https://omahaproxy.appspot.com.

API changes are documented here: https://docs.google.com/document/d/1g8JFi8T_oAE_7uAri7Njtig7fKaPDfotU6huOa1alds/edit.

## Floating patches

### How to land a patch from upstream V8?

Sometimes, it is necessary to cherry-pick changes made to newer V8 versions upstream and land them in the current V8 version used in `deps/v8`. In order to do that, you'll need to do two things:

1. Cherry-pick the commit from V8's repository into node's repository.
2. Change the commit message to mention that it's a back-port from V8 upstream, and add the appropriate metadata.

#### Cherry-picking commits from V8's GitHub mirror

First, clone V8's GitHub mirror: [follow the instructions online](https://chromium.googlesource.com/v8/v8.git).
Then, identify the commit to backport, and within your node local repository, use the following command:
```
git --git-dir=v8/.git format-patch -k -1 --stdout sha1 | git am -k --directory=deps/v8
```
where `v8` is the path to V8's Git repository (preferably a local clone of https://chromium.googlesource.com/v8/v8.git) relative to your local node repository, and `sha1` is the sha of the commit you want to cherry-pick.

At this point, the original commit from V8 should be committed in your local tree. It's time to change the commit message with `git commit --amend` to alter the first line and add in the body that this commit is a back-port from V8.

A good example of such a commit message is https://github.com/joyent/node/commit/2fc5eeb3da0cbc5c17674818bc679d3d564d0d06. Note that in this commit message, the following lines:
```
PR-URL: #9200
Reviewed-by: Trevor Norris <trev.norris@gmail.com>
Reviewed-by: Julien Gilli <julien.gilli@joyent.com>
```
were added by the Jenkins job that merges changes, you don't need to add that yourself.

### Existing floating patches

#### v0.10.x

* https://github.com/v8/v8-git-mirror/commit/2ad2237507c5b5f9047b8d94d2f4997327eae852: fixes issues with using `--debug-brk` with `--use-strict`.
* https://github.com/joyent/node/commit/431eb172f97434a3b0868a610bc14d8ff7d9efd9 (`deps: log V8 version in profiler log file`) and https://github.com/joyent/node/commit/2b095bb76c62d2e27f0fb9ca716f17198e925f62 (`deps: add test for V8 version in profiler's log`).
* https://github.com/joyent/node/commit/5a60e0d904c38c2bdb04785203b1b784967c870d: v8: back-port JitCodeEvent patch from upstream.

#### v0.12.x

* https://github.com/joyent/node/commit/3589a62104eac1daae782ab479bec09f4df4dc9a: `build: fix build for SmartOS`.
* https://github.com/v8/v8-git-mirror/commit/2ad2237507c5b5f9047b8d94d2f4997327eae852: fixes issues with using `--debug-brk` with `--use-strict`.
* https://github.com/joyent/node/commit/ac21a5390c6e02f0bc9052c7794dcabc33a0a0c3. However the commit message needs to be changed to something like: `mdb_v8: fix dictionary properties access`.
* https://github.com/joyent/node/commit/2320210639516025a5ad0744e8ff45b687212422: allows to not lose IRHydra support.
* https://github.com/joyent/node/commit/1314cfe4760744de2bebe3e54abdf957c57caeae: `deps: fix postmortem-metadata generator in v8`.
* https://github.com/joyent/node/commit/431eb172f97434a3b0868a610bc14d8ff7d9efd9 (`deps: log V8 version in profiler log file`) and https://github.com/joyent/node/commit/2b095bb76c62d2e27f0fb9ca716f17198e925f62 (`deps: add test for V8 version in profiler's log`).
* https://github.com/joyent/node/commit/2a5f4bd7ce40b9537c5d77558687858c7246d896: `v8: fix issue with let bindings in for loops`.
* https://github.com/joyent/node/commit/80cdae855fe349f0cf3b5c646060e4799107392f: `deps: don't busy loop in v8 cpu profiler thread`.
* https://github.com/joyent/node/commit/6157697bd5bdbf58b19b9d3177249a7bbb467f79: `deps: revert v8 Array.prototype.values() removal`.
* https://github.com/joyent/node/commit/b81a643f9ae341b0c23cecc54daccdc8d7bc746a: `V8: avoid deadlock when profiling is active`.
* https://github.com/joyent/node/commit/88a27a96216ac7aa728419a693ce164e59fb29c6: `v8: cherry-pick JitCodeEvent patch from upstream`.

### Patches that need to be upstreamed

* https://github.com/joyent/node/issues/9147: `fix to dictionary properties access`.

## Performing the upgrade process

Performing a V8 upgrade is a simple process once the existing floating patches are identified:

1. Checkout the correct V8 tag in your local V8 git repository. For instance:
  * `git clone https://chromium.googlesource.com/v8/v8.git`.
  * `cd v8`.
  * `git checkout 3.28.71.19`.
2. Replace the content of the previous V8 release with the content from the new release: `rm -rf /path/to/node/deps/v8 && git checkout-index -a -f --prefix=/path/to/node/deps/v8/`. The trailing slash for the `--prefix` option in the second command is important.
3. Apply the floating patches mentioned in the [floating patches section](#floating-patches).
4. Run the tests as described in the [testing](#testing) section.

## Testing

### Node.js tests suite

The first step is to run the standard Node.js tests suite. The best way to do that is to push the branch that contains the V8 upgrade to joyent/node __in a feature branch (something like v8-upgrade)__ and to run the the [node-review-unix Jenkins job](jenkins.nodejs.org/job/node-review-unix) on that branch.

### Testing post-mortem debugging on SmartOS

Post-mortem debugging on SmartOS relies on information generated both by the build process and by V8 at runtime. When V8 changes, sometimes the metadata generated at build time needs to change to match V8's runtime data structures.

In order to test post-mortem debugging on SmartOS, simply build a node binary with the default build options and run the following tests: `test/pummel/test-postmortem-details.js`, `test/pummel/test-postmortem-findjsobjects.js`, `test/pummel/test-postmortem-jsstack.js`. All these tests must exit with a 0 exit code.