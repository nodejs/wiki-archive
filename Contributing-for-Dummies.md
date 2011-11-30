
Contributing to the node.js project through github can be quite convenient
and easy, but does require some knowledge of github and git so you
"make all the right moves".

You contribute to node.js by making "pull requests" from your fork on github.
Each pull request will contain one or more "commits" containing your changes.
You will want to generate these and submit them so that they are easily
usable by the core committers.

Below are some suggestions that might help the new contributor to
smooth the process of generating new contributions and pull requests.


I'll assume you've already forked the joyent/node repository on github
and now have your own github repository seriousu/node.  You create your
local repository with

```
    git clone git@github.com:seriousu/node.git
    cd node
    git branch
```

## Let branch 'master' be mirror of current node.js repository

Your fork and repository starts out with one local branch, 'master'.
It is tempting to just start hackering in this default branch.  Don't.

Instead, keep the 'master' branch reserved as your branch that tracks the
node.js repository.  By not doing any local changes in the 'master' branch
you make it easy to keep it refreshed with the latest commits from github.

Let git know where the node.js 'upstream' repository is, which is
where you originally forked from.

```
    git remote -v add upstream git://github.com/joyent/node.git
    git remote
```

'origin' is your repository on github, and 'upstream' is now the name
you will use when you want to refer to the joyent/node repository.

You can refresh from the 'upstream' repository at any time, grabbing the
very latest changes:

```
    git checkout master
    git pull --rebase upstream master
    git log
```

## Use separate branches for each pull request

You may need to generate more than one active pull request.  A pull request
can take awhile to be reviewed and processed, and you might have to respond
to review comments by revising your commits.  It is quite hard to have
multiple pull requests and commits active without separating each into its
own branch.

As noted above, you will likely want to keep a local copy of node.js
up-to-date with the repository at github.  It is probably best to simply
reserve the 'master' branch for this.

After you have created your fork of node.js and cloned that to your local
system, can create a new branch for your next work by using

```
    git checkout -b add_my_idea

        ... amazing hackliness here ...

    git add ...
    git commit

    git push origin add_my_idea
```


## commit comments

When your pull request is accepted a core committer will apply your
commits to node.  Your commit comments will end up in the node commit
log and so should be usable.  As the "[[Contributing]]" page asks,
please keep the summary line short at 50 characters or less.  Add a
blank line and then any additional comment text.

But not too much text, as all this is going into the node commit log.
I've tried writing long narratives on how I found the goofs / nits / fantastic
refactorings ... and found they were _too_ long.  Instead, now I enter a
short simple description, and put the fascinating monographs into the
comments attached to the pull request page.


## Reviewing pull requests

Everybody is interested in getting the best result possible, so you should
be ready to respond to questions or concerns about your pull request.

People may leave questions in the form of comments in the pull request
page.  Take a look inside a couple of the
[current requests](https://github.com/joyent/node/pulls)
You will see people exchanging messages back and forth, or even just
leaving notes to themselves about what needs to be done next.

People can also leave comments about particular lines or areas within
your changes.  If you bring up a commit and scroll down to the display
of changed lines you might see "line notes".  This can make a question
easier to understand because it is right next to the line(s) being
asked about.

One question caused me to realize that some code I'd
copied from /lib/ was just plain wrong.  My code _and_ the original
lib/ code were made better by that question.

Questions can make your changes better.  Your answers might even tell
the core guys something they didn't know.


## Revising

One request I got was to split one big commit into two parts, two
commits, the obvious stuff that could be applied even if the rest of
the request was rejected, and the more confusing stuff which was going
to need more discussion before being applied.

Another request was to change some 'fancy' code into something more
easily understandable.  It is easy to try to 'fix' too much.


```
    git rebase -i HEAD^^


        reword
        edit
```

***to-be-continued***