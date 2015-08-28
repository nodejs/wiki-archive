This guide describes the procedure for testing pull requests or arbitrary commits with Jenkins. For a description of how to land pull requests with Jenkins, please refer to ["Merging pull requests with Jenkins"]( https://github.com/nodejs/node/wiki/Merging-pull-requests-with-Jenkins).

### Basic workflow for testing pull requests
Starting the node-test-pull-request Jenkins job requires having an account on the [Node.js Jenkins platform](https://jenkins-iojs.nodesource.com/). This is currently restricted to Node.js collaborators. 
Any collaborator can trigger the job when it’s determined that the pull request should be tested. Since the pull request will also be tested before merging when [using node-accept-pull-request](https://github.com/nodejs/node/wiki/Merging-pull-requests-with-Jenkins), testing the pull request in advance is not mandatory, and may be skipped if the author has already tested the changes on all platforms affected by the changes and/or if the change is trivial and there is high confidence that it will not break anything.

To test a pull-request, simply follow these steps:

1. Point your browser to the [node-test-pull-request](https://jenkins-iojs.nodesource.com/job/node-test-commit/) Jenkins job.
1. On the left hand side, you should see "Build with parameters". If not, it probably means that you're not logged in. You need to be logged in to start this job.
1. Click on "Build with parameters".
1. Fill out the form:
  * Enter the github org and repo name where the pull request lives (typically nodejs and node respectively).
  * Enter the pull-request number in the PR_ID text field. 
  * Before building, changes are rebased onto the PR’s base branch. This behavior can be customized by using the REBASE_ONTO drop-down. In there, you can either select a predefined branch name, or disable rebasing altogether. If your desired branch name is not in there, you can use the node-test-commit job instead (described below).
1. Click on the "Build" button.

A new job number with a progress bar running should appear in the bottom left of the page, in the "build history" section. A colored ball appears on the left of the new job with the following possible colors:
* Green for all builds and tests passing.
* Red for at least one build or test failing. Note that unlike node-accept-pull-request, node-test-pull-request treats [flaky tests](https://github.com/nodejs/node/wiki/Flaky-tests) as fatal.

It is common practice to post a comment on the pull request on GitHub, containing a link to the job run.

### Advanced workflow with node-test-commit
If you need to test arbitrary changes that are not part of pull requests, or if you need more advanced options, you can use the [node-test-commit](https://jenkins-iojs.nodesource.com/job/node-test-commit/) job. This job is used internally by node-test-pull-request and provides additional functionality. node-test-pull-request is just a simplified front-end for node-test-commit for the common case of testing pull requests.

You can follow the same steps as you would for the basic workflow, with the following differences:
* Instead of entering a PR_ID, you enter the name of the git ref that you want to test in GIT_REMOTE_REF. This can be branch, a pull request head, or even a commit SHA1. The format is the same as the remote portion of the refspec passed to the [git fetch](http://git-scm.com/docs/git-fetch) command. Specifically:
  * To test a branch, enter `refs/heads/<branch_name>`
  * To test a tag, enter `refs/tags/<tag_name>`
  * To test a pull request head, enter `refs/pull/<pr_id>/head`
  * To test a specific commit, enter `<commit_sha1>`
* You can specify a branch to rebase onto before building, with the REBASE_ONTO field. If you leave it empty, no rebasing will be performed. Otherwise you can rebase onto pretty much any ref. **IMPORTANT**: branch names here must be specified as `origin/<branch_name>`.
* NODES_SUBSET allows specifying a specific subset of platforms and configurations for the testing. The `auto` option determines the right configuration automatically based on the version of Node.js in the changes (determined from node_version.h). The other options are mostly useful for testing the Jenkins job itself.
* By checking IGNORE_FLAKY_TESTS, you can make the test runs mark the builds as unstable (yellow) instead of failure (red), when only [flaky tests](https://github.com/nodejs/node/wiki/Flaky-tests) fail.

For any issues with the above procedures or this guide, please mention @nodejs/jenkins-admins on GitHub.