The simplest way to contribute to Node.js is by using GitHub's pull request system. This guide will walk you through from creating a fork to having your first pull request accepted.

## First steps

Go to the repository page and hit that big **Fork** button in the top right. This will create your very own personal copy of the repository to which you can commit. Now `clone` the repository to your machine:

```
git clone git@github.com:seriousu/node.git
cd node
```

To receive the latest changes we need to let the repository know where the `upstream` repository lives. _Pull requests that don't stay up-to-date don't get pulled_.

```
    git remote -v add upstream https://github.com/joyent/node.git
    git fetch upstream
```

Now you're setup and anxious to start hacking away with some awesome ideas. **STOP**. If you want a snowball's chance in hell of having your changes pulled, they need to be self contained. To do this, start by creating and checking out a new branch from `master`:

```
git checkout master
git checkout -b my_awesome_idea
```

Now you can begin to make changes and commit them to your hearts content.

## Your first commit

Quick note on committing and commit messages. First, make sure each commit contains only the relevant code changes for that commit. Don't change a single important line, then pollute it with random syntax and white space changes. If you find yourself in that circumstance read up on [git interactive staging](http://git-scm.com/book/en/Git-Tools-Interactive-Staging).

Now that you have staged an appropriate number of changes for a single commit, don't screw it up by using a useless commit message. Such as: 'fixed a small bug'. To prevent yourself from creating these useless commit messages, get in the habit of not using the `-m` parameter with `git commit`.

Node has a very standard commit message style. Here's a great example:

```txt
child_process: don't `resume()` created socket

Calling `resume()` on a stream switches it to the old mode which causes
piping stdio from a child process to fail.

Fixes joyent/node#4510.
```

This demonstrates three key things:
* First line is prepended with area of functionality and followed by a short and descriptive message.
* Followed by descriptive text explaining what caused the bug.
* Ended by referencing the bug in question.

GitHub automatically links to bugs in commit messages in the form of `<usr>/<repo>#<issue>` or `GH-<issue>`. Use it, please.

Usually userland pull requests won't be large enough for multiple commits. So if you've already committed but would like to add additional changes to that commit use the following:

```
git commit --amend
```

This will bring up the commit message box which can be edited and saved. Once this is done your commit history will be rewritten. So in order to push these new changes to the server they'll have to be forced. Force pushing changes will overwrite the remote history with what you currently have in your local repository:

```
git push -f origin my_awesome_branch
```

## Staying up-to-date

It's highly unlikely that in the time you create a change, submit the pull request and have it accepted there will be no changes to the repository. So now it's time to get those changes into our repository. Do this with the following:

```
git checkout master
git fetch upstream
git merge --ff upstream/master
```

The `--ff` will `fast-forward` (meaning not leaving a trace of a merge) to your `master` branch. _If you can't fast-forward your master branch something has gone wrong_.

And now to combine the latest from `master` to `my_awesome_idea`. How do we keep our changes up on the current `upstream` branch? If you're thinking `merge` you're mistaken. Merging your changes will make all kinds of ugly in the pull request, and nobody wants ugly.

Instead we'll be using `rebase`. This will rewind your commits then reapply them to the latest. Do that with the following:

```
git checkout my_awesome_branch
git rebase master
```

It's possible there will be conflicts with your changes. You can either fix the conflicts then `git rebase --continue` or just `git rebase --abort` to cancel the `rebase`. Afterwards force push your changes with the same `git push -f origin my_awesome_idea`. Now when you check your pull request again all you'll see are the update SHA's of each commit. And GitHub has a very nice code commenting feature where any previous comments will only be rendered outdated if the section of code was changed by the `rebase`.

## Submission and review

If you're submitting a pull request, chances are you want your work accepted. Before that can happen you need to sign [the CLA](http://nodejs.org/cla.html). If it's your first time contributing to the project save everyone a step and mention in the comment box that you already have.

Now you're ready to submit! It's actually kind of anticlimactic. Go to your repo page and click the **Pull Request** button in the top right. Now **STOP**. Have you? Yes. Ok, now look at this page. Look at it again. It's very important to understand what's on this page.

On the top left you'll see `base repo` and `base branch`. This defines where you want the pull request to go. The `base repo` should automatically select the `upstream` or cloned repository. If you've branched from `master` (which is all we've discussed) then make sure `base branch` is also set to `master`.

On the top right you'll see `head repo` and `head branch`. Your fork should automatically be selected in `head repo`. Now the really important field. Make sure `head branch` is the branch you've put all your blood sweet and tears into. If your pull request is only a single commit then the text fields should fill with the commit message. In any case make sure you fully explain why your pull request is so important. Now, you can hit **Send pull request**. There, submitted. Feel better now? Me too.

Everybody is interested in getting the best result possible, so you should be ready to respond to questions or concerns about your pull request. Don't take offense to what other developers say. Especially the [core development team](https://github.com/joyent/node/wiki/Project-Organization). If they bring something up it's for a good reason. Listen, learn and try to understand why their suggestion works. In the case you disagree with them, provide empirical reasons why (e.g. benchmarks, test cases, etc.). The community has a tendency to ignore developers who are too attached to their work.

From your pull request users can either leave general comments or code review comments. Code reviews will be set as **outdated** when the specific line of code reviewed are changed as you update the pull request branch. Use these comments to improve your pull request. Make changes, `rebase` your commit(s), continue to take feedback. Be prepared to consistently revisit and revise your work. Don't be surprised if it takes longer to manage the pull request than it did to make the code change.

For a more extreme example of a pull request see [joyent/node#4348](https://github.com/joyent/node/pull/4348). Isaacs worked on this pull request for months, continued to `rebase` the commits against `master` and finally put it up for code review. This was a truly amazing amount of work that once was ready, seamlessly merged.

On a final note: Don't get discouraged. Node is held to high standards, and it can take months to begin to feel comfortable with how the workflow operates and why things are done the way they are. Jump on IRC. Chances are someone is there ready to help.

Welcome to the community.