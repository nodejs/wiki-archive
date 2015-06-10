# Node.js release guide

## Prerequisites

In order to go through the whole release process, you will first need to do
some required setup on your local machine.

### Local machine

#### Setup the joyent/node and joyent/node-website

Currently, the release process is made for a setup where the `joyent/node` and `joyent/node-website` repositories sit side by side.
So you'll need to have both projects checked out in locations similar to:

```
~/foo/bar/node/v0.12
~/foo/bar/node/node-website
```
where `/foo/bar` is a common prefix.

#### Setup your signing identity with Git

Add the key ID to Git config to be able to sign tags by adding the following content to
your `~/.gitconfig` file:

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

It's not time to update the `AUTHORS` file:

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

#### Download the certificates and keys needed to sign the pkg file

You will need to install [manta](https://www.npmjs.com/package/manta) to be
able to download the keys and certificates used to sign the OSX application
and package.

```
mget -O /NodeCore/stor/joyent-dev-int.p12
mget -O /NodeCore/stor/joyent-dev-app.p12
```

#### Set following environment variables to generate the pkg file

`INT_SIGN='Developer ID Installer: Joyent, Inc (X4ETB2T5LK)' APP_SIGN='Developer ID Application: Joyent, Inc (X4ETB2T5LK)'`

#### Trigger the build

You will first need to install the PackageMaker OSX application, then run:
```
PACKAGEMAKER=/Applications/PackageMaker.app/Contents/MacOS/PackageMaker INT_SIGN="Developer ID Installer: Joyent, Inc (X4ETB2T5LK)" APP_SIGN="Developer ID Application: Joyent, Inc (X4ETB2T5LK)" make pkg -j 6
```

### Upload the OSX binary pkg

Upload the OSX pkg first before kicking off the nodejs-release job from
Jenkins so that it's accounted for when generating the checksums file.

### Use the CI platform to trigger a build of all release files (except the OSX installer package)

#### Start the Jenkins nodejs-release job

#### Confirm key signing for Windows builds

When running the nodejs-release job on Jenkins, connect to the windows box
(`node-build.cloudapp.net:50708`, see the Node.js CI administration guide for
more information) to authorize the signing, otherwise it stalls the whole
process.

#### Remove extra unneeded pdb, lib and exe files

#### Make sure SHASUMS files were generated properly

## Publish release artefacts

```
./tools/node-release-post-build.sh
```

## Smoke test release artefacts

## Push changes to joyent/node repository

## Publish changes to the website

Email:
After release-post-build.sh is run, email.md is in the current directory.

Then go in the node-website directory and do:

```
make release
```

### Test changes made to the website

### Publish them online

### Commit and push the changes to joyent/node-website
