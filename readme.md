# ParanoidAndroid #

## Working on translations ##

We're using [Crowdin](https://crowdin.net/project/aospa-framework) to accept translations so you
should join it if you are interested in working on translating a part of the project.

## Grabbing the source ##

[Repo](http://source.android.com/source/developing.html) is a tool provided by Google that
simplifies using [Git](http://git-scm.com/book) in the context of the Android source.

### Installing Repo ###

```bash
# Make a directory where Repo will be stored and add it to the path
$ mkdir ~/bin
$ PATH=~/bin:$PATH

# Download Repo itself
$ curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo

# Make Repo executable
$ chmod a+x ~/bin/repo
```

### Initializing Repo ###

```bash
# Create a directory for the source files
# This can be located anywhere (as long as the fs is case-sensitive)
$ mkdir WORKSPACE
$ cd WORKSPACE

# Install Repo in the created directory
# Use a real name/email combination, if you intend to submit patches
$ repo init -u https://github.com/AOSPA/manifest -b kitkat
```

### Downloading the source tree ###

This is what you will run each time you want to pull in upstream changes. Keep in mind that on your
first run, it is expected to take a while as it will download all the required Android source files
and their change histories.

```bash
# Let Repo take care of all the hard work
$ repo sync
```

#### Syncing specific projects ####

In case you are not interested in syncing all the projects, you can specify what projects you do
want to sync. This can help if, for example, you want to make a quick change and quickly push it
back for review. You should note that this can sometimes cause issues when building if there is
a large change that spans across multiple projects.

```bash
# Specify one or more projects by either name or path

# For example, enter AOSPA/android_frameworks_base or
# frameworks/base to sync the frameworks/base repository

$ repo sync PROJECT
```

## Building ##

The bundled builder tool `./rom-build.sh` handles all the building steps for the specified device
automatically. As the device value, you just feed it with the device codename (for example,
'hammerhead' for the Nexus 5).

```bash
# Go to the root of the source tree...
$ cd WORKSPACE
# ...and run the builder tool.
$ ./rom-build.sh DEVICE
```

## Submitting Patches ##

We're open source and patches are always welcome!

You can see the status of all patches at [Gerrit Code Review](https://gerrit.paranoidandroid.co/).

### Following the standard workflow ###

```bash
# Start by going to the root of the source tree
$ cd WORKSPACE

# Create a new branch on the specific project you are going to work on
# For example, `repo start fix-clock AOSPA/android_frameworks_base`
$ repo start BRANCH AOSPA/PROJECT

# Go inside the project you are working on
$ cd PROJECT

# Make your changes
...

# Commit all your changes
$ git add -A
$ git commit -a

# Upload your changes
$ cd WORKSPACE
$ repo upload AOSPA/PROJECT
```

### Making additional changes ###

If you are going to make more changes, you just have to repeat the steps (except for `repo start`
which you should not repeat) while using `git commit --amend` instead of `git commit -a` so that
you avoid having multiple commits for this single change. Gerrit will then recognize these changes
as a new patch set and figure out everything for you when you upload.

### Squashing multiple commits ###

Your patches should be single commits. If you have multiple commits laying around, squash them by
running `git rebase -i HEAD~<commit-count>` before uploading.

### Writing good commit messages ###

You will be asked a commit message when you run `git commit`. Writing a good commit message is
often hard, but it is also essential as these messages will stay around with your changes and
will be seen by others when looking back at the project history.

A few general pointers to keep in mind when writing the commit message are that you should use
imperative as it matches the style used by the `git merge` and `git revert` commands (that means
"Fix bug" is preferred over "Fixes bug", "Fixed bug" and others) and that you should write the
first line of the commit message as a summary of the commit. It should always be capitalized and
followed by an empty line. You might optionally include the project name at the start and try to
keep it to 50 characters when possible as it is used in various logs, including "one line" logs.
