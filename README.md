# Paranoid Android

## Set up your machine

You must run a 64-bit Linux distribution to build Paranoid Android.
Follow the system setup instructions on the [Android Open Source Project website](https://source.android.com/source/initializing.html#setting-up-a-linux-build-environment).
Google provides Ubuntu-specific setup packages and instructions.
Complete the environment setup before you proceed.

## Obtain the source

[Repo](https://source.android.com/source/developing.html) is a tool provided by Google that simplifies using [Git](https://git-scm.com/book) with Android source trees.

### Install Repo

Create a directory for `repo` and add it to your `PATH`:
```bash
mkdir -p ~/.local/bin
export PATH=~/.local/bin:$PATH
```

Download the `repo` binary:
```bash
curl https://storage.googleapis.com/git-repo-downloads/repo > ~/.local/bin/repo
```

Make the binary executable:
```bash
chmod a+x ~/.local/bin/repo
```

### Initialize Repo

Create a working directory on a case-sensitive filesystem and navigate into it.
Replace `WORKSPACE` with your chosen directory path.
```bash
mkdir WORKSPACE
cd WORKSPACE
```

Initialize the manifest repository:

> [!IMPORTANT]
> Configure your real name and email address in Git before you initialize Repo if you plan to submit patches.

```bash
repo init -u https://github.com/AOSPA/manifest -b calcite
```

### Download the source tree

Run `repo sync` to pull upstream source code.

> [!NOTE]
> Initial synchronization downloads the entire source history and takes significant time.

The `-j` option specifies the number of concurrent network jobs.
```bash
repo sync --current-branch --no-tags -j4
```

> [!TIP]
> A value of four jobs (`-j4`) works well for most internet connections.
> Adjust this value based on your connection speed.

#### Sync specific projects

You can synchronize individual projects instead of the entire source tree.
Specify projects by repository path or remote name.

> [!WARNING]
> Partial synchronization can cause build failures if changes span across projects.

For example, specify `frameworks/base` or `AOSPA/android_frameworks_base`:
```bash
repo sync PROJECT
```

## Build

The bundled builder script `./rom-build.sh` automates all build steps for a target device.
Provide the target device codename as the argument (for example, `phone2` for Nothing Phone (2)).

Navigate to your workspace root and execute the build script:
```bash
cd WORKSPACE
./rom-build.sh DEVICE
```

## Submit patches

Paranoid Android is open source and accepts patches from contributors.
Track patch review status on [Gerrit Code Review](https://gerrit.aospa.co/).

### Standard workflow with Repo

Navigate to your workspace root:
```bash
cd WORKSPACE
```

Create a topic branch for the project you want to modify.
Identify the project by repository name or local directory path:

| Identify by | Command |
| --- | --- |
| Repository name | `repo start BRANCH AOSPA/PROJECT` |
| Directory path | `repo start BRANCH PROJECT_DIR` |

For example, start a branch for `frameworks/base` (`AOSPA/android_frameworks_base`):
```bash
repo start BRANCH frameworks/base
```

Navigate to the project directory:
```bash
cd PROJECT_DIR
```

Make your code changes, then stage and commit them:
```bash
git add -A
git commit -a -s
```

Upload your changes to Gerrit for review:
```bash
cd WORKSPACE
repo upload PROJECT_DIR
```

### Workflow with plain Git

Navigate to the project directory:
```bash
cd PROJECT_DIR
```

Make your code changes, then stage and commit them:
```bash
git add -A
git commit -a -s
```

Push the commit directly to Gerrit.
Replace `USERNAME` with your Gerrit username and `PROJECT` with the repository name.
```bash
git push ssh://USERNAME@gerrit-ssh.aospa.co:29418/AOSPA/PROJECT HEAD:refs/for/calcite
```

### Gerrit push options

Append a suffix to the refspec to configure review options.
You can also toggle these options in the Gerrit web interface.

| Suffix | Effect |
| --- | --- |
| `%private` | Upload a change as private |
| `%wip` | Upload a change as work-in-progress |
| `%remove-private` | Remove the private status from a change |
| `%ready` | Mark a work-in-progress change as ready for review |

For example, upload a change as private:
```bash
git push ssh://USERNAME@gerrit-ssh.aospa.co:29418/AOSPA/PROJECT HEAD:refs/for/calcite%private
```

### Make additional changes

To update an existing patch set, make your changes and amend the previous commit.

> [!CAUTION]
> Do not run `repo start` again.
> Running `repo start` creates a new topic branch instead of updating the existing review.

```bash
git commit -a --amend
```
When you upload the amended commit, Gerrit attaches it as a new patch set to the existing review.

### Squash multiple commits

Each submitted patch must be a single commit.
Squash multiple commits before you upload:
```bash
git rebase -i HEAD~<commit-count>
```

### Write commit messages

Write clear and descriptive commit messages.

- Use the imperative mood in the subject line (for example, "Fix audio routing", not "Fixed audio routing").
- Keep the subject line near 50 characters and under 72 characters.
- Capitalize the first word of the subject line and omit trailing periods.
- Prefix the subject with the relevant project or component name when appropriate (for example, `manifest: Update default branch`).
- Separate the subject line from the message body with a blank line.
- Wrap message body text at 72 characters.

## Translations

Submit translations for Paranoid Android through Crowdin.
Access the translation portal at https://crowdin.aospa.co.

## Project assets and licensing

### Source code

The codebase uses the Apache License, Version 2.0 unless otherwise specified.
Retain all copyright and license notices when you use or modify the source code.
State any modifications you make to the code.
Read the full license text at https://www.apache.org/licenses/LICENSE-2.0.

### Images and branding assets

Unless otherwise specified, all project assets (including images and branding) use the Creative Commons Attribution-NonCommercial 4.0 International (CC BY-NC 4.0) license.
You may share and adapt these assets for non-commercial purposes.
You must provide attribution to the original author (Paranoid Android Project or AOSPA).
Include a reference to the license, note any modifications, and link to https://aospa.co.
Read the full license text at https://creativecommons.org/licenses/by-nc/4.0/.
