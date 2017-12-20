Flaky tests are tests that fail intermittently. This can happen if there is an underlying bug that only happens intermittently. Or it could also happen if a test is timing-dependent and affected by environmental conditions such as the speed of the machine running the test.

Flaky tests should be fixed to be made non-flaky. Usually this means fixing the underlying bug, or changing the test so that it doesn’t depend on timing.

When only tests that are marked as flaky fail in a CI run, the corresponding build status is set to unstable (yellow) instead of failure (red). You can still navigate to the TAP test results and see which flaky tests failed. Flaky test failures are reduced from fatal to warning level by adding a # TODO directive in the test output (see the [TAP specification](https://testanything.org/tap-specification.html) for additional information).

### What to do when you encounter a new flaky test
Sometimes when you run CI, a test will fail and you (and other reviewers) are sure that the failure is not related to the changes in the PR. Still, it’s blocking the PR from being merged. In this case, you can add a commit to your PR or create a separate PR, to mark the test as flaky. Tests are marked as flaky by adding an entry for them in the status file in the test directory. For example, for tests under tests/sequential, you’ll need to edit test/sequential/sequential.status. For the specific format to use, please refer to the comments in the status files themselves.

Tests can be marked as flaky for specific platforms, or for all platforms. It’s recommended to start with one platform if the test only failed on one platform, and to move it to the section for all platforms when the test shows failures on two or more platforms.

**IMPORTANT:** once your commit that marks one or more tests as flaky is merged, please file a follow-up issue for investigating the flaky test and resolving the problem. One issue per flaky test should be filed, unless the tests and the failures are very similar to each other. The issue should be marked with the P-1 label, and assigned to the next milestone for the branch in which the test is marked as flaky. You should also mention @nodejs/collaborators in the issue. 

Once the issue is resolved, a pull request should be submitted to remove the test from the list of flaky tests.
