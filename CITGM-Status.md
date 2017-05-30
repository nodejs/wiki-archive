[CITGM known flakes](https://github.com/nodejs/node/wiki/CITGM-know-flakes)

#### hot spot fails:
  * [ 7/11] yeoman-generator-v1.1.1
  * [ 4/11] ffi-v2.2.0
  * [ 4/11] zeromq_v4_2_1

---
# 2017-05-30 [8be9e4739ae6f20c816f6dbdaf9067160113563d](https://github.com/nodejs/node/commit/235cbbe4d8087ebd62dd9271a4d585458ffed45a)
  [49 failures , 64 skipped / 888](https://ci.nodejs.org/view/Node.js-citgm/job/citgm-smoker/847/testReport/)
  #### Notes:
  * this is a PR that landed in `master` and is the `v8.0.0`

# 2017-05-24 [235cbbe](https://github.com/nodejs/node/commit/235cbbe4d8087ebd62dd9271a4d585458ffed45a)
  [62 failures , 64 skipped / 971](https://ci.nodejs.org/view/Node.js-citgm/job/citgm-smoker/828/testReport/)
  #### Notes:
  * New fails (`zeromq` and `node-gyp`) because of node-gyp rejecting `pre` label:  
  `not ok 178 Error: "pre" versions of node cannot be installed, use the --nodedir flag instead`
  #### new fails (see note):
  * zeromq_v4_2_1 [ [`aix61-ppc64`](https://ci.nodejs.org/view/Node.js-citgm/job/citgm-smoker/828/nodes=aix61-ppc64/testReport/(root)/citgm/zeromq_v4_2_1/), [`ppcbe-ubuntu1404`](https://ci.nodejs.org/view/Node.js-citgm/job/citgm-smoker/828/nodes=ppcle-ubuntu1404/testReport/(root)/citgm/zeromq_v4_2_1/), [`debian8-64`](https://ci.nodejs.org/view/Node.js-citgm/job/citgm-smoker/828/nodes=debian8-64/testReport/(root)/citgm/zeromq_v4_2_1/), [`ppcbe-ubuntu1404`](https://ci.nodejs.org/view/Node.js-citgm/job/citgm-smoker/828/nodes=ppcbe-ubuntu1404/testReport/(root)/citgm/zeromq_v4_2_1/) ]
  * node_gyp_v3_6_1 [ [`ppcle-ubuntu1404`](https://ci.nodejs.org/view/Node.js-citgm/job/citgm-smoker/828/nodes=ppcle-ubuntu1404/testReport/(root)/citgm/node_gyp_v3_6_1/) ]

# 2017-05-24 [ccd3ead](https://github.com/nodejs/node/commit/ccd3eadbd7dae3a23d43bf490fa9d3019324370e)
  [61 failures , 78 skipped / 971](https://ci.nodejs.org/view/Node.js-citgm/job/citgm-smoker/826/testReport/)
  ### Notes:
  * `rhel72-s390x` restored
  * No new fails

# 2017-05-24 `v8.RC.1` [3af9fa0](https://github.com/nodejs/node/commit/3af9fa0490f45977898212816dad5656d28f109f)
  [, / 971](https://ci.nodejs.org/view/Node.js-citgm/job/citgm-smoker/827/testReport/)
  ### Notes:
  * `v8` RC1 baseline

# 2017-05-23 [9836cf5](https://github.com/nodejs/node/commit/9836cf571708a82396218957cacb3ed1ed468d05)
  [33 failures , 74 skipped / 891](https://ci.nodejs.org/view/Node.js-citgm/job/citgm-smoker/823/testReport/)  
  #### new fails:
  * [3 / 11] sodium-native-v1.10.0 [aix61-ppc64, win-vs2015, ppcbe-ubuntu1404]
  * [3 / 11] zeromq-v4.2.1 [aix61-ppc64, ppcle-ubuntu1404, ppcbe-ubuntu1404]
  #### Notes:
  * This is a run I did for a PR (rebased on the quoted commit) but it got better coverage than the one before
  * Added 3 new modules [`zeromq`, `node-gyp`, `sodium-native`] but `rhel72-s390x` did not finish
  * spdy-v3.4.7 - new version passes on all  
  * spdy-transport-v2.0.19 - new version passes on all  

# 2017-05-22 [171a43a](https://github.com/nodejs/node/commit/171a43a98685d5cca6710d2d6bf4d20008de3426)
  [46 failures , 71 skipped / 939](https://ci.nodejs.org/job/citgm-smoker/811/testReport/)
  #### hot spot fails:
  * [10/12] spdy-v3.4.4
  * [10/12] spdy-transport-v2.0.18
  * [ 7/12] yeoman-generator-v1.1.1
  * [ 4/12] ffi-v2.2.0
  ### New fails:
  * spdy-transport-v2.0.18 @ [*] (not tested on [win-vs2015, aix])
  * split2-v2.1.1 @ [aix]
  * node-report-v2.1.2 @ [aix]
  * graceful-fs-v4.1.11 @ [win10]
  * node-sass-v4.5.3 @ [ubuntu1204-64] - `sass-spec spec output_styles compact scss huge`: timeout
  * watchify-v3.9.0 @ [osx1010] - `62-67`: timeout
  * node-report-v2.1.2 @ [aix] - `test-fatal-error`: timeout
  * ftp-v0.3.10 @ [ubuntu1204-64]
  * ffi-v2.2.0 [fedora22]

# 2017-05-21 [bfade5a](https://github.com/nodejs/node/commit/bfade5aacd639fbac920647bf1ca4a6fb6df9e0d)
  [30 failures , 72 skipped / 936](https://ci.nodejs.org/view/Node.js-citgm/job/citgm-smoker/806/testReport/)  
  #### hot spot fails:
  * [10/12] spdy-v3.4.4
  * [7/10] yeoman-generator-v1.1.1

# 2017-05-20 #2 [8250bfd](https://github.com/nodejs/node/commit/8250bfd1e5188d5dada58aedf7a991e959d5eaa9)
  [37 failures , 66 skipped / 936](https://ci.nodejs.org/view/Node.js-citgm/job/citgm-smoker/805/testReport/)  
  After [`fs: Revert throw on invalid callbacks`](https://github.com/nodejs/node/pull/12976) landed  
  <sub>this is a run I did on a PR, but IMHO it's representative of the status quo</sub>
  #### hot spot fails:
  * [10/12] spdy-v3.4.4
  * [7/10] yeoman-generator-v1.1.1  

# 2017-05-20 [5254975](https://github.com/nodejs/node/commit/525497596a51ef2e6653b930ca525046d27c9fd5)
  [45 failures , 79 skipped / 936](https://ci.nodejs.org/view/Node.js-citgm/job/citgm-smoker/801/testReport/)  
  First good run since we restored reasonable CITGM on `master` with [#13092](https://github.com/nodejs/node/pull/13092)

---

[archive as spreadsheet](https://docs.google.com/spreadsheets/d/1VimEU1-gQ4aOIZxGGRqD8XVeriMUrM7nzBKgxxLQYlc/pubhtml)  
[archive as wiki](https://github.com/nodejs/node/wiki/CITGM-results-table)
