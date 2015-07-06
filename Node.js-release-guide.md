# Node.js release guide

## Prerequisites

In order to go through the whole release process, you will first need to do
some required setup on your local machine.

### Signing keys

Each release is signed by the person doing the release. You will need to have a signing key that can be publicly verified to do a release of Node.js. If you don't have a key yet, one easy way to get a new one is to use https://keybase.io.

### SSH keys

In addition to signing keys, you will need SSH keys to get access to a remote shell nodejs.org. Simply generate a pair of SSH keys and paste your public key in an issue in github.com/nodejs/LTS to mention that you need to be able to access nodejs.org to do Node.js releases.

### Local machine setup

#### Setup the joyent/node and joyent/node-website

Currently, the release process is made for a setup where the `joyent/node` and `joyent/node-website` repositories sit side by side.
So you'll need to have both projects checked out in locations similar to:

```
~/foo/bar/node/v0.12
~/foo/bar/node/node-website
```
where `/foo/bar` is a common prefix.

#### Setup your signing identity with Git

Add the signing key ID (see ["Signing keys"](#signing-keys) to know how to get a signing key) to Git's config to be able to sign tags by adding the following content to your `~/.gitconfig` file:

```
[user]
  email = your@email.com
  name = Your Name
  signingkey = SIGNING_KEY_ID
```

where `SIGNING_KEY_ID` is the identifier of your signing key. Mine for
instance is `0246406D`.

#### Add a `node` as the default user when accessing `nodejs.org` via SSH

In `~/.ssh/config`, make sure that the following entry is present:

```
Host nodejs.org
 User node
```

## Make required changes to the source code repository

### Create a new entry for the new release in the changelog

```
git checkout -b v$(python tools/getnodeversion.py)-release
(git log --first-parent --graph --pretty=format:'%h%d %s (%an)' v$(head -n1 ChangeLog | awk '{ print $3 }')^..HEAD | sort -k 3 ; echo ""; echo ""; cat ChangeLog ) > ChangeLog.x; mv ChangeLog.x ChangeLog
vi ChangeLog
```

At that point, modify `ChangeLog` to keep only entries for api changes and bug
fixes for the public api. All documentation changes, build fixes or anything
that has no impact on end users must not be included in the ChangeLog.

The exception is for dependency upgrades: they are moved to the top of the
ChangeLog, and they don't include the author's name.

Also, make sure to add the sha1 for the commit that was used to create the
previous release.

### Update the AUTHORS file

It's now time to update the `AUTHORS` file:

```
./node tools/update-authors.js
```
and then make sure that new authors have been properly added (by using `git diff` for instance).

### Update the version number

Run the following command to turn your local code base into an official release:
```
perl -pi -e 's/define NODE_VERSION_IS_RELEASE 0/define NODE_VERSION_IS_RELEASE 1/' src/node_version.h
```

### Commit all these changes with the newly added changelog entry as commit message

```
git commit -am "$(bash tools/changelog-head.sh)"
```

After running this command, review the commit message with `git show` and make sure that it's properly formatted
and the content is correct.

### Push changes to your own fork of joyent/node so the CI platform can see the changes

```
git push git@github.com:GITHUB_USERNAME/node.git v$(python tools/getnodeversion.py)-release
```
and replace `GITHUB_USERNAME` by your GitHub username.

## Build release artefacts

### Build the binary package manually from a local OSX machine

The OSX binary package cannot be built automatically from the CI platform because
at some point the binary and package signature processes will need to display a prompt
to confirm the usage of the signature key and to ask for a passphrase, and there's an issue that
makes the build process stalls when that happens on our OSX Jenkins agent.

#### Add certificates and keys needed to sign the pkg file

You will need to install [manta](https://www.npmjs.com/package/manta) to be
able to download the keys and certificates used to sign the OSX application
and package.

```
mget -O /NodeCore/stor/joyent-dev-int.p12
mget -O /NodeCore/stor/joyent-dev-app.p12
```

Once downloaded, you will need to add these certificates and keys to your keychain. Simply double-click on these files and follow instructions.

#### Set following environment variables to generate the pkg file

`INT_SIGN='Developer ID Installer: Joyent, Inc (X4ETB2T5LK)' APP_SIGN='Developer ID Application: Joyent, Inc (X4ETB2T5LK)'`

#### Trigger the build

You will first need to install the PackageMaker OSX application, then run:
```
PACKAGEMAKER=/Applications/PackageMaker.app/Contents/MacOS/PackageMaker INT_SIGN="Developer ID Installer: Joyent, Inc (X4ETB2T5LK)" APP_SIGN="Developer ID Application: Joyent, Inc (X4ETB2T5LK)" make pkg -j 6
```

### Upload the OSX binary pkg

Upload the OSX pkg first before kicking off the nodejs-release job from
Jenkins so that it's accounted for when generating the checksums file:

```
scp out/node-v$(python tools/getnodeversion.py).pkg staging@nodejs.org:archive/node/tmp/v$(python tools/getnodeversion.py)
```

### Use the CI platform to trigger a build of all release files (except the OSX installer package)

#### Start the Jenkins nodejs-release job

Go to http://jenkins.nodejs.org/job/nodejs-release/ and click on "Build with parameters". Fill in the build parameters with the following values:

* `GIT_BRANCH` should be the result of running `echo v$(python tools/getnodeversion.py)-release`.
* `GIT_REPO` should be your own fork of joyent/node.
* `INT_SIGN` must be filled with `Developer ID Installer: Joyent, Inc (X4ETB2T5LK)`. Omitting to fill this field will make the release process do a nightly release.
* `APP_SIGN` must be filled with `Developer ID Application: Joyent, Inc (X4ETB2T5LK)`.

#### Confirm key signing for Windows builds

When running the nodejs-release job on Jenkins, connect to the windows box
(`node-build.cloudapp.net:50708`, see the Node.js CI administration guide for
more information) to authorize the signing, otherwise it stalls the whole
process.

#### Remove extra unneeded pdb, lib and exe files

The current build process generates unneeded `.pdb`, `.lib` and `.exe` files when doing a v0.12.x release.
To avoid distributing these files, make sure that you remove them from `/home/staging` when the nodejs-release Jenkins jobs has completed:

```
$ ssh staging@nodejs.org
$ cd node/tmp/archive/v0.12.x
$ rm icu* genrb* genccode*
$ cd x64 && rm icu* genrb* genccode*
```

#### Make sure SHASUMS files were generated properly

Sometimes, SHASUMS* files are not generated, but they are needed for the next step of the release process. Go to `/home/staging` and edit 'shasums.sh` to point to the proper directory where new release's files are located (e.g `/home/staging/v0.12.6`). Then run `sh /home/staging/shasums.sh`, all SHASUMS* files should now be present. 

## Publish release artefacts

```
./tools/node-release-post-build.sh
```

## Smoke test release artefacts

Login to nodejs.org as user `staging` and download newly created release files.
Smoke test them by doing the following for releases on all supported platforms:

1. Install node.
2. Run `node -p 'process.versions' and make sure that all version numbers are correct (especially dependencies if they've been upgraded with this release).
3. Install a module with binary add-ons as dependencies (like `ws` with `npm install ws`) to make sure that node-gyp, npm and node works correctly. Clean up ~/.node-gyp and run `npm cache clean` before doing so to force a fresh download/build.

## Push changes to joyent/node repository

It is now time to push the release branch, tag and merge commit of the release branch to the parent branch upstream (joyent/node):

`git push git@github.com:joyent/node.git v$(python tools/getnodeversion.py) v$(python tools/getnodeversion.py)-release v$(python tools/getnodeversion.py | sed -E 's#\.[0-9]+$##')`

## Publish changes to the website

### Publish API docs

API docs of the newly released version need to be published to the website. Go to the node repository (not node-website) and run:
```
make website-upload
```

### Write blog post

At that point in time, there should be a file with the `.md` extension in your `node-website` local clone.
Edit it and add any information you think would help users of Node.js understand better what this new release is about. Most of the time, the generated file is fine. In case of releases that fix security vulnerabilities, you will need to add some background around what these issues are and how they were fixed by Node.js.

Once the blog post is ready, run make test-blog and check that the blog post displays correctly.

### Publish blog post and website changes

Now go in the `node-website` directory and do:

```
make release
```

This will publish the blog post and changes to the `Downloads` section of the website.

### Commit and push the changes to joyent/node-website

Finally, commit and push changes to the website to the master branch.

## Advertise new release on Twitter in the Node.js Google group