[CITGM known flakes](https://github.com/nodejs/node/wiki/CITGM-know-flakes)

## 2017-05-23 [485be99](https://github.com/nodejs/node/commit/485be99e684f5e296c2aba28a401d5ef42fc3cb4)

## 2017-05-22 [171a43a](https://github.com/nodejs/node/commit/171a43a98685d5cca6710d2d6bf4d20008de3426)
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

## 2017-05-21 [bfade5a](https://github.com/nodejs/node/commit/bfade5aacd639fbac920647bf1ca4a6fb6df9e0d)
  [30 failures , 72 skipped / 936](https://ci.nodejs.org/view/Node.js-citgm/job/citgm-smoker/806/testReport/)  
  #### hot spot fails:
  * [10/12] spdy-v3.4.4
  * [7/10] yeoman-generator-v1.1.1

## 2017-05-20 #2 [8250bfd](https://github.com/nodejs/node/commit/8250bfd1e5188d5dada58aedf7a991e959d5eaa9)
  [37 failures , 66 skipped / 936](https://ci.nodejs.org/view/Node.js-citgm/job/citgm-smoker/805/testReport/)  
  After [`fs: Revert throw on invalid callbacks`](https://github.com/nodejs/node/pull/12976) landed  
  <sub>this is a run I did on a PR, but IMHO it's representative of the status quo</sub>
  #### hot spot fails:
  * [10/12] spdy-v3.4.4
  * [7/10] yeoman-generator-v1.1.1  

## 2017-05-20 [5254975](https://github.com/nodejs/node/commit/525497596a51ef2e6653b930ca525046d27c9fd5)
  [45 failures , 79 skipped / 936](https://ci.nodejs.org/view/Node.js-citgm/job/citgm-smoker/801/testReport/)  
  First good run since we restored reasonable CITGM on `master` with [#13092](https://github.com/nodejs/node/pull/13092)

---

[archive as spreadsheet](https://docs.google.com/spreadsheets/d/1VimEU1-gQ4aOIZxGGRqD8XVeriMUrM7nzBKgxxLQYlc/pubhtml)  
[archive as wiki](https://github.com/nodejs/node/wiki/CITGM-results-table)
