All pull requests should be merged using the [node-accept-pull-request](https://jenkins-iojs.nodesource.com/job/node-accept-pull-request/) Jenkins job.
node-accept-pull-request does the following:

1. It rebases the PR onto the target branch.
2. It modifies the commits messages for all the commits in the PR, adding metadata like PR-URL and Reviewed-By.
3. It builds and tests the changes on several platforms.
4. If all the builds and [non-flaky](https://github.com/nodejs/node/wiki/Flaky-tests) tests succeed, it does a fast-forward merge to integrate the changes it just tested, and pushes the updated branch to GitHub.

Because of the fast-forward merge, if someone else were to update and push the same branch while a run is in progress between steps (1) and (4), the branch update at (4) would fail. This is somewhat desirable, because we want the job to test the result of the rebase, and push exactly what it has tested. As a consequence, it is important that everybody sticks to the same workflow and uses Jenkins to merge pull requests instead of doing manual merges.

node-accept-pull-request can also be used for testing only, by unchecking the APPLY_CHANGES option (as described below). However, it's recommended to [use node-test-pull-request for testing](https://github.com/nodejs/node/wiki/Testing-pull-requests-with-Jenkins), as it is optimized for that use case. In particular, node-accept-pull-request runs are serialized (because of the fast-forward merge issue described above), while node-test-pull-request can have multiple concurrent runs.

### Basic workflow
Starting the node-accept-pull-request Jenkins job requires an account on the [Node.js Jenkins platform](https://jenkins-iojs.nodesource.com/). This is currently restricted to Node.js collaborators. 
The responsibility of triggering the job and potentially merging changes is on the collaborator who determines that a pull request is ready to be merged, according to the [Collaborator Guide](https://github.com/nodejs/node/blob/master/COLLABORATOR_GUIDE.md#accepting-modifications).
To merge a pull request, simply follow these steps:

1. Point your browser to the [node-accept-pull-request](https://jenkins-iojs.nodesource.com/job/node-accept-pull-request/) Jenkins job.
1. On the left hand side, you should see "Build with parameters". If not, it probably means that you're not logged in. You need to be logged in to start this job.
1. Click on "Build with parameters"
1. Fill out the information in the form:
  * Enter github org and repo name where the pull request lives (typically nodejs and node respectively).
  * Enter the pull-request number in the PR_ID text field. 
  * Leave MERGE_METHOD set to `Rebase`
  * NODES_SUBSET specifies which types of tests and platforms will be run. The `auto` option selects this automatically based on the branch and version of the code, and it’s good for most uses. If the PR modifies documentation exclusively, you can use `pure_docs_change` (warning: tests will be skipped. Please don’t abuse this).
  * Leave APPLY_CHANGES checked if you are ready to merge the PR. If you uncheck it, the job will to perform all the same functions, excepting pushing the changes back to GitHub. While this can be used for testing, it is recommended that you [use node-test-pull-request](https://github.com/nodejs/node/wiki/Testing-pull-requests-with-Jenkins) if you want to do testing only.  
  * Select reviewers from the drop-down lists. It doesn’t matter which drop-downs you use, but at least one reviewer is required according to the collaborators guide. The REVIEWED_BY4 field is free text, and can be used to add reviewers that are not found in the drop-downs and/or when you want to list four reviewers. Please write this in the same format as the drop-downs. If you didn’t find a reviewer in the drop-downs, please notify @nodejs/jenkins admins so that we can add them.
  * Note that existing PR-URL: and Reviewed-By: lines in the commit messages will be stripped, and replaced by the ones specified in the form. This only happens when PR-URL or Reviewed-By is found at a beginning of a line. If you want to preserve existing PR-URL: or Reviewed-By: you can simply indent them in your commit messages.
  * Additional metadata (FIXES, FIXES2, REF) is optional.
1. Click on the "Build" button.

A new job number with a progress bar running should appear in the bottom left of the page, in the "build history" section. A colored ball appears on the left of the new job with the following possible colors:
* Green for all builds and tests passing.
* Yellow for all build and tests passing but [flaky tests](https://github.com/nodejs/node/wiki/Flaky-tests) failing.
* Red for at least one build or non-flaky test failing.

If the color is green or yellow, the changes will be merged in the pull request’s target branch.
If a pull request fails to pass all tests on all platforms, its associated changes cannot be merged into the repository. Always make sure to notify the author of the pull request of test results, and if they failed, always provide links to the results of the Jenkins job that illustrate the failures.

### Workflow for making changes to other people’s pull request before merging
In some cases, a pull request might need some changes before it can be merged, but the author is not available or it simply might be faster for the committing collaborator to do it.
You can do this by using a temporary branch:

1. Pick a new branch name where you’ll make the changes. Since this branch will temporarily end up in GitHub, it’s a good practice to prefix the branch with your GitHub username. For example: `myusername-PR274-update`.
1. Fetch the PR changes locally: `git fetch <repository> refs/pull/<PR_ID>/head:<your_branch>`.
1. Checkout the branch with `git checkout <your_branch>`.
1. Make the desired changes and commit them.
1. Push your branch to GitHub.
1. Point your browser to [node-merge-commit](https://jenkins-iojs.nodesource.com/job/node-merge-commit/). This job is used internally by node-accept-pull-request, and provides additional functionality. You can follow the same steps as you would for the [basic workflow](https://github.com/nodejs/node/wiki/Merging-pull-requests-with-Jenkins#basic-workflow), with the following differences:
  * Instead of entering the PR_ID, you enter the name of your temporary branch and the branch that you want to merge the changes. Set GIT_REMOTE_REF to `refs/heads/<your_branch>` and TARGET_BRANCH to the target branch of the PR (without `refs/heads/`).
  * You need to enter the PR-URL yourself, so it can be added to the commit message.

Once the changes have been merged (or abandoned), please delete your branch from GitHub from the UI, or by issuing `git push --force <repository> :<your_branch>`.

### Workflow for doing branch merges
This is for making branch merges, e.g. for merging branch next into master or whenever you want a merge commit to be created between two branches. 
If there is a PR associated with the merge, you can simply use node-accept-pull-request, but specifying `Merge` instead of `Rebase` for the MERGE_METHOD option.
If there is no PR associated with the merge, you need to use the [node-merge-commit](https://jenkins-iojs.nodesource.com/job/node-merge-commit/) job. This job is used internally by node-accept-pull-request, and provides additional functionality. You can follow the same steps as you would for the [basic workflow](https://github.com/nodejs/node/wiki/Merging-pull-requests-with-Jenkins#basic-workflow), with the following differences:

* Instead of entering a PR_ID, you enter the name of the source branch and target branch for the merge. Set GIT_REMOTE_REF to `refs/heads/<source_branch>` and TARGET_BRANCH to the target branch of the merge (without `refs/heads/`).
* Specify `Merge` instead of `Rebase` for the MERGE_METHOD option.
* Leave PR-URL empty, and enter additional metadata as appropriate.
* When the `Merge` MERGE_METHOD is selected, the metadata provided in the form is only added to the newly created merge commit.

For any issues with the above procedures or this guide, please mention @nodejs/jenkins-admins on GitHub.