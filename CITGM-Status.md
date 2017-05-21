[CITGM known flakes](https://github.com/nodejs/node/wiki/CITGM-know-flakes)

## 2017-05-17
* since we restored reasonable CITGM on `master` with [#13092](https://github.com/nodejs/node/pull/13092), this morning we had:  
  [45 failures , 79 skipped / 936](https://ci.nodejs.org/view/Node.js-citgm/job/citgm-smoker/801/testReport/)  
* After [`fs: Revert throw on invalid callbacks`](https://github.com/nodejs/node/pull/12976) landed, we bumped up to:  
  [37 failures , 66 skipped / 936](https://ci.nodejs.org/view/Node.js-citgm/job/citgm-smoker/805/testReport/)
  <sub>this is a run I did on a PR, but IMHO it's representative of the status quo</sub>  
  #### hot spot fails:
  * [10/12] spdy-v3.4.4
  * [7/10] yeoman-generator-v1.1.1  


## 2017-05-18
* [30 failures , 72 skipped / 936](https://ci.nodejs.org/view/Node.js-citgm/job/citgm-smoker/806/testReport/)  
  #### hot spot fails:
  * [10/12] spdy-v3.4.4
  * [7/10] yeoman-generator-v1.1.1


---

[archive as spreadsheet](https://docs.google.com/spreadsheets/d/1VimEU1-gQ4aOIZxGGRqD8XVeriMUrM7nzBKgxxLQYlc/pubhtml)  
[archive as wiki](https://github.com/nodejs/node/wiki/CITGM-results-table)
