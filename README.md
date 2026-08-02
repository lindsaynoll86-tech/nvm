<a href="https://github.com/nvm-sh/logos">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/nvm-sh/logos/HEAD/nvm-logo-white.svg" />
    <img src="https://raw.githubusercontent.com/nvm-sh/logos/HEAD/nvm-logo-color.svg" height="50" alt="nvm project logo" />
  </picture>
</a>


# Node Version Manager [![Tests](https://github.com/nvm-sh/nvm/actions/workflows/tests-fast.yml/badge.svg?branch=master)][3] [![nvm version](https://img.shields.io/badge/version-v0.40.6-yellow.svg)][4] [![CII Best Practices](https://bestpractices.dev/projects/684/badge)](https://bestpractices.dev/projects/684)

<!-- To update this table of contents, ensure you have run `npm install` then `npm run doctoc` -->
<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
## Table of Contents

- [Intro](#intro)
- [About](#about)
- [Installing and Updating](#installing-and-updating)
  - [Install & Update Script](#install--update-script)
    - [Additional Notes](#additional-notes)
    - [Installing in Docker](#installing-in-docker)
      - [Installing in Docker for CICD-Jobs](#installing-in-docker-for-cicd-jobs)
    - [Troubleshooting on Linux](#troubleshooting-on-linux)
    - [Troubleshooting on macOS](#troubleshooting-on-macos)
    - [Ansible](#ansible)
  - [Verify Installation](#verify-installation)
  - [Important Notes](#important-notes)
  - [Git Install](#git-install)
  - [Manual Install](#manual-install)
  - [Manual Upgrade](#manual-upgrade)
- [Usage](#usage)
  - [Long-term Support](#long-term-support)
  - [Migrating Global Packages While Installing](#migrating-global-packages-while-installing)
  - [Migrating Global Packages Between Installed Versions](#migrating-global-packages-between-installed-versions)
  - [Offline Install](#offline-install)
  - [Default Global Packages From File While Installing](#default-global-packages-from-file-while-installing)
  - [io.js](#iojs)
  - [System Version of Node](#system-version-of-node)
  - [Listing Versions](#listing-versions)
  - [Setting Custom Colors](#setting-custom-colors)
    - [Persisting custom colors](#persisting-custom-colors)
    - [Suppressing colorized output](#suppressing-colorized-output)
  - [Restoring PATH](#restoring-path)
  - [Set default node version](#set-default-node-version)
  - [Use a mirror of node binaries](#use-a-mirror-of-node-binaries)
    - [Pass Authorization header to mirror](#pass-authorization-header-to-mirror)
  - [.nvmrc](#nvmrc)
  - [Deeper Shell Integration](#deeper-shell-integration)
    - [Calling `nvm use` automatically in a directory with a `.nvmrc` file](#calling-nvm-use-automatically-in-a-directory-with-a-nvmrc-file)
      - [bash](#bash)
      - [zsh](#zsh)
      - [fish](#fish)
- [Running Tests](#running-tests)
- [Environment variables](#environment-variables)
- [Bash Completion](#bash-completion)
  - [Usage](#usage-1)
- [Compatibility Issues](#compatibility-issues)
- [Installing nvm on Alpine Linux](#installing-nvm-on-alpine-linux)
  - [Alpine Linux 3.13+](#alpine-linux-313)
  - [Alpine Linux 3.5 - 3.12](#alpine-linux-35---312)
- [Uninstalling / Removal](#uninstalling--removal)
  - [Manual Uninstall](#manual-uninstall)
- [Docker For Development Environment](#docker-for-development-environment)
- [Problems](#problems)
- [macOS Troubleshooting](#macos-troubleshooting)
- [WSL Troubleshooting](#wsl-troubleshooting)
- [Maintainers](#maintainers)
- [Project Support](#project-support)
- [Enterprise Support](#enterprise-support)
- [License](#license)
- [Copyright notice](#copyright-notice)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Intro

`nvm` allows you to quickly install and use different versions of [node](https://nodejs.org) via the command line.

**Example:**
```sh
$ nvm install 24
Now using node v24.14.0 (npm v11.9.0)
$ node -v
v24.14.0
$ nvm use 22
Now using node v22.22.1 (npm v10.9.4)
$ node -v
v22.22.1
$ nvm use 20
Now using node v20.20.1 (npm v10.8.2)
$ node -v
v20.20.1
```

Simple as that!


## About
nvm is a version manager for [node.js](https://nodejs.org/en/), designed to be installed per-user, and invoked per-shell. `nvm` works on any POSIX-compliant shell (sh, [dash](https://git.kernel.org/pub/scm/utils/dash/dash.git), [ksh](https://github.com/ksh93/ksh), [zsh](https://www.zsh.org), [bash](https://www.gnu.org/software/bash/)), in particular on these platforms: unix, [macOS](https://www.apple.com/macos/), and [Windows WSL](https://github.com/nvm-sh/nvm#important-notes).

<a id="installation-and-update"></a>
<a id="install-script"></a>
## Installing and Updating

### Install & Update Script

To **install** or **update** nvm, you should run the [install script][2]. To do that, you may either download and run the script manually, or use the following [cURL](https://curl.se) or [Wget](https://www.gnu.org/software/wget/) command:
```sh
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.6/install.sh | bash
```
```sh
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.6/install.sh | bash
```

Running either of the above commands downloads a script and runs it. The script clones the nvm repository to `~/.nvm`, and attempts to add the source lines from the snippet below to the correct profile file (`~/.bashrc`, `~/.bash_profile`, `~/.zshrc`, or `~/.profile`). If you find the install script is updating the wrong profile file, set the `$PROFILE` env var to the profile file’s path, and then rerun the installation script.

<a id="profile_snippet"></a>
```sh
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
```

#### Additional Notes

- If the environment variable `$XDG_CONFIG_HOME` is present, it will place the `nvm` files there.</sub>

- You can add `--no-use` to the end of the above script to postpone using `nvm` until you manually [`use`](#usage) it:

```sh
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" --no-use # This loads nvm, without auto-using the default version
```

- You can customize the install source, directory, profile, and version using the `NVM_SOURCE`, `NVM_DIR`, `PROFILE`, and `NODE_VERSION` variables.
Eg: `curl ... | NVM_DIR="path/to/nvm"`. Ensure that the `NVM_DIR` does not contain a trailing slash.

- The installer can use [`git`](https://git-scm.com/), `curl`, or `wget` to download `nvm`, whichever is available.

- You can instruct the installer to not edit your shell config (for example if you already get completions via a [zsh nvm plugin](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/nvm)) by setting `PROFILE=/dev/null` before running the `install.sh` script. Here's an example one-line command to do that: `PROFILE=/dev/null bash -c 'curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.6/install.sh | bash'`

#### Installing in Docker

When invoking bash as a non-interactive shell, like in a [Docker](https://www.docker.com) container, none of the regular profile files are sourced. In order to use `nvm`, `node`, and [`npm`](https://www.npmjs.com) like normal, you can instead specify the special `BASH_ENV` variable, which bash sources when invoked non-interactively.

```Dockerfile
# Use bash for the shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Create a script file sourced by both interactive and non-interactive bash shells
ENV BASH_ENV "${HOME}/.bash_env"
RUN touch "${BASH_ENV}"
RUN echo '. "${BASH_ENV}"' >> ~/.bashrc

# Download and install nvm
RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.6/install.sh | PROFILE="${BASH_ENV}" bash
RUN echo node > .nvmrc
RUN nvm install
```

##### Installing in Docker for CICD-Jobs

More robust, works in CI/CD-Jobs. Can be run in interactive and non-interactive containers.
See https://github.com/nvm-sh/nvm/issues/3531.

```Dockerfile
FROM ubuntu:latest
ARG NODE_VERSION=20

# install curl
RUN apt update && apt install curl -y

# install nvm
RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.6/install.sh | bash

# set env
ENV NVM_DIR=/root/.nvm

# install node
RUN bash -c "source $NVM_DIR/nvm.sh && nvm install $NODE_VERSION"

# set ENTRYPOINT for reloading nvm-environment
ENTRYPOINT ["bash", "-c", "source $NVM_DIR/nvm.sh && exec \"$@\"", "--"]

# set cmd to bash
CMD ["/bin/bash"]

```

This example defaults to installation of nodejs version 20.x.y. Optionally you can easily override the version with docker build args like:
```
docker build -t nvmimage --build-arg NODE_VERSION=19 .
```

After creation of the image you can start container interactively and run commands, for example:
```
docker run --rm -it nvmimage

root@0a6b5a237c14:/# nvm -v
0.40.6

root@0a6b5a237c14:/# node -v
v19.9.0

root@0a6b5a237c14:/# npm -v
9.6.3
```

Noninteractive example:
```
user@host:/tmp/test $ docker run --rm -it nvmimage node -v
v19.9.0
user@host:/tmp/test $ docker run --rm -it nvmimage npm -v
9.6.3
```

#### Troubleshooting on Linux

On [Linux](https://www.kernel.org/), after running the install script, if you get `nvm: command not found` or see no feedback from your terminal after you type `command -v nvm`, simply close your current terminal, open a new terminal, and try verifying again.
Alternatively, you can run the following commands for the different shells on the command line:

*bash*: `source ~/.bashrc`

*zsh*: `source ~/.zshrc`

*ksh*: `. ~/.profile`

These should pick up the `nvm` command.

#### Troubleshooting on macOS

Since OS X 10.9, `/usr/bin/git` has been preset by [Xcode](https://developer.apple.com/xcode/) command line tools, which means we can't properly detect if Git is installed or not. You need to manually install the Xcode command line tools before running the install script, otherwise, it'll fail. (see [#1782](https://github.com/nvm-sh/nvm/issues/1782))

If you get `nvm: command not found` after running the install script, one of the following might be the reason:

  - Since macOS 10.15, the default shell is `zsh` and nvm will look for `.zshrc` to update, none is installed by default. Create one with `touch ~/.zshrc` and run the install script again.

  - If you use bash, the previous default shell, your system may not have `.bash_profile` or `.bashrc` files where the command is set up. Create one of them with `touch ~/.bash_profile` or `touch ~/.bashrc` and run the install script again. Then, run `. ~/.bash_profile` or `. ~/.bashrc` to pick up the `nvm` command.

  - You have previously used `bash`, but you have `zsh` installed. You need to manually add [these lines](#manual-install) to `~/.zshrc` and run `. ~/.zshrc`.

  - You might need to restart your terminal instance or run `. ~/.nvm/nvm.sh`. Restarting your terminal/opening a new tab/window, or running the source command will load the command and the new configuration.

  - If the above didn't help, you might need to restart your terminal instance. Try opening a new tab/window in your terminal and retry.

If the above doesn't fix the problem, you may try the following:

  - If you use bash, it may be that your `.bash_profile` (or `~/.profile`) does not source your `~/.bashrc` properly. You could fix this by adding `source ~/<your_profile_file>` to it or following the next step below.

  - Try adding [the snippet from the install section](#profile_snippet), that finds the correct nvm directory and loads nvm, to your usual profile (`~/.bash_profile`, `~/.zshrc`, `~/.profile`, or `~/.bashrc`).

  - For more information about this issue and possible workarounds, please [refer here](https://github.com/nvm-sh/nvm/issues/576)

**Note** For Macs with the Apple Silicon chip, node started offering **arm64** arch Darwin packages since v16.0.0 and experimental **arm64** support when compiling from source since v14.17.0. If you are facing issues installing node using `nvm`, you may want to update to one of those versions or later.

#### Ansible

You can use a task:

```yaml
- name: Install nvm
  ansible.builtin.shell: >
    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.6/install.sh | bash
  args:
    creates: "{{ ansible_env.HOME }}/.nvm/nvm.sh"
```

### Verify Installation

To verify that nvm has been installed, do:

```sh
command -v nvm
```

which should output `nvm` if the installation was successful. Please note that `which nvm` will not work, since `nvm` is a sourced shell function, not an executable binary.

**Note:** On Linux, after running the install script, if you get `nvm: command not found` or see no feedback from your terminal after you type `command -v nvm`, simply close your current terminal, open a new terminal, and try verifying again.

### Important Notes

If you're running a system without prepackaged binary available, which means you're going to install node or [io.js](https://iojs.org) from its source code, you need to make sure your system has a C++ compiler. For OS X, Xcode will work, for [Debian](https://www.debian.org)/[Ubuntu](https://ubuntu.com) based GNU/Linux, the `build-essential` and `libssl-dev` packages work.

**Note:** `nvm` also supports Windows in some cases. It should work through WSL (Windows Subsystem for Linux) depending on the version of WSL. It should also work with [Git Bash](https://gitforwindows.org/) ([MSYS](https://www.msys2.org/)) or [Cygwin](https://cygwin.com). Otherwise, for Windows, a few alternatives exist, which are neither supported nor developed by us:

  - [nvm-windows](https://github.com/coreybutler/nvm-windows)
  - [nodist](https://github.com/marcelklehr/nodist)
  - [nvs](https://github.com/jasongin/nvs)

**Note:** `nvm` does not support [Fish] either (see [#303](https://github.com/nvm-sh/nvm/issues/303)). Alternatives exist, which are neither supported nor developed by us:

  - [bass](https://github.com/edc/bass) allows you to use utilities written for Bash in fish shell
  - [fast-nvm-fish](https://github.com/brigand/fast-nvm-fish) only works with version numbers (not aliases) but doesn't significantly slow your shell startup
  - [plugin-nvm](https://github.com/derekstavis/plugin-nvm) plugin for [Oh My Fish](https://github.com/oh-my-fish/oh-my-fish), which makes nvm and its completions available in fish shell
  - [nvm.fish](https://github.com/jorgebucaran/nvm.fish) - The Node.js version manager you'll adore, crafted just for Fish
  - [fish-nvm](https://github.com/FabioAntunes/fish-nvm) - Wrapper around nvm for fish, delays sourcing nvm until it's actually used.

**Note:** We still have some problems with [FreeBSD](https://www.freebsd.org), because there is no official pre-built binary for FreeBSD, and building from source may need [patches](https://www.freshports.org/www/node/files/patch-deps_v8_src_base_platform_platform-posix.cc); see the issue ticket:

  - [[#900] [Bug] node on FreeBSD may need to be patched](https://github.com/nvm-sh/nvm/issues/900)
  - [nodejs/node#3716](https://github.com/nodejs/node/issues/3716)

**Note:** On OS X, if you do not have Xcode installed and you do not wish to download the ~4.3GB file, you can install the `Command Line Tools`. You can check out this blog post on how to just that:

  - [How to Install Command Line Tools in OS X Mavericks & Yosemite (Without Xcode)](https://osxdaily.com/2014/02/12/install-command-line-tools-mac-os-x/)

**Note:** On OS X, if you have/had a "system" node installed and want to install modules globally, keep in mind that:

  - When using `nvm` you do not need `sudo` to globally install a module with `npm -g`, so instead of doing `sudo npm install -g grunt`, do instead `npm install -g grunt`
  - If you have an `~/.npmrc` file, make sure it does not contain any `prefix` settings (which is not compatible with `nvm`)
  - You can (but should not?) keep your previous "system" node install, but `nvm` will only be available to your user account (the one used to install nvm). This might cause version mismatches, as other users will be using `/usr/local/lib/node_modules/*` VS your user account using `~/.nvm/versions/node/vX.X.X/lib/node_modules/*`

[Homebrew](https://brew.sh) installation is not supported. If you have issues with homebrew-installed `nvm`, please `brew uninstall` it, and install it using the instructions below, before filing an issue.

**Note:** If you're using `zsh` you can easily install `nvm` as a zsh plugin. Install [`zsh-nvm`](https://github.com/lukechilds/zsh-nvm) and run `nvm upgrade` to upgrade ([you can set](https://github.com/lukechilds/zsh-nvm#auto-use) `NVM_AUTO_USE=true` to have it automatically detect and use `.nvmrc` files).

**Note:** Git versions before v1.7 may face a problem of cloning `nvm` source from [GitHub](https://github.com) via https protocol, and there is also different behavior of git before v1.6, and git prior to [v1.17.10](https://github.com/git/git/commit/5a7d5b683f869d3e3884a89775241afa515da9e7) can not clone tags, so the minimum required git version is v1.7.10. If you are interested in the problem we mentioned here, please refer to GitHub's [HTTPS cloning errors](https://help.github.com/articles/https-cloning-errors/) article.

### Git Install

If you have `git` installed (requires git v1.7.10+):

1. clone this repo in the root of your user profile
    - `cd ~/` from anywhere then `git clone https://github.com/nvm-sh/nvm.git .nvm`
1. `cd ~/.nvm` and check out the latest version with `git checkout v0.40.6`
1. activate `nvm` by sourcing it from your shell: `. ./nvm.sh`

Now add these lines to your `~/.bashrc`, `~/.profile`, or `~/.zshrc` file to have it automatically sourced upon login:
(you may have to add to more than one of the above files)

```sh
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

### Manual Install

For a fully manual install, execute the following lines to first clone the `nvm` repository into `$HOME/.nvm`, and then load `nvm`:

```sh
export NVM_DIR="$HOME/.nvm" && (
  git clone https://github.com/nvm-sh/nvm.git "$NVM_DIR"
  cd "$NVM_DIR"
  git checkout `git describe --abbrev=0 --tags --match "v[0-9]*" $(git rev-list --tags --max-count=1)`
) && \. "$NVM_DIR/nvm.sh"
```

Now add these lines to your `~/.bashrc`, `~/.profile`, or `~/.zshrc` file to have it automatically sourced upon login:
(you may have to add to more than one of the above files)

```sh
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

### Manual Upgrade

For manual upgrade with `git` (requires git v1.7.10+):

1. change to the `$NVM_DIR`
1. pull down the latest changes
1. check out the latest version
1. activate the new version

```sh
(
  cd "$NVM_DIR"
  git fetch --tags origin
  git checkout `git describe --abbrev=0 --tags --match "v[0-9]*" $(git rev-list --tags --max-count=1)`
) && \. "$NVM_DIR/nvm.sh"
```

## Usage

To download, compile, and install the latest release of node, do this:

```sh
nvm install node # "node" is an alias for the latest version
```

To install a specific version of node:

```sh
nvm install 14.7.0 # or 16.3.0, 12.22.1, etc
```

To set an alias:

```sh
nvm alias my_alias v14.4.0
```
Make sure that your alias does not contain any spaces or slashes.

The first version installed becomes the default. New shells will start with the default version of node (e.g., `nvm alias default`).

You can list available versions using `ls-remote`:

```sh
nvm ls-remote
```

And then in any new shell just use the installed version:

```sh
nvm use node
```

Or you can just run it:

```sh
nvm run node --version
```

Or, you can run any arbitrary command in a subshell with the desired version of node:

```sh
nvm exec 4.2 node --version
```

You can also get the path to the executable to where it was installed:

```sh
nvm which 12.22
```

In place of a version pointer like "14.7" or "16.3" or "12.22.1", you can use the following special default aliases with `nvm install`, `nvm use`, `nvm run`, `nvm exec`, `nvm which`, etc:

  - `node`: this installs the latest version of [`node`](https://nodejs.org/en/)
  - `iojs`: this installs the latest version of [`io.js`](https://iojs.org/en/)
  - `stable`: this alias is deprecated, and only truly applies to `node` `v0.12` and earlier. Currently, this is an alias for `node`.
  - `unstable`: this alias points to `node` `v0.11` - the last "unstable" node release, since post-1.0, all node versions are stable. (in [SemVer](https://semver.org), versions communicate breakage, not stability).
  - `current`: the version currently active in this shell (i.e. what `node` resolves to via `$PATH`). It is **not** affected by `.nvmrc`. Useful when you want to refer to the active version explicitly &mdash; e.g. `nvm which current` always prints the path to the active `node`, regardless of whether an `.nvmrc` file is present.

### Long-term Support

Node has a [schedule](https://github.com/nodejs/Release#release-schedule) for long-term support (LTS) You can reference LTS versions in aliases and `.nvmrc` files with the notation `lts/*` for the latest LTS, and `lts/argon` for LTS releases from the "argon" line, for example. In addition, the following commands support LTS arguments:

  - `nvm install --lts` / `nvm install --lts=argon` / `nvm install 'lts/*'` / `nvm install lts/argon`
  - `nvm uninstall --lts` / `nvm uninstall --lts=argon` / `nvm uninstall 'lts/*'` / `nvm uninstall lts/argon`
  - `nvm use --lts` / `nvm use --lts=argon` / `nvm use 'lts/*'` / `nvm use lts/argon`
  - `nvm exec --lts` / `nvm exec --lts=argon` / `nvm exec 'lts/*'` / `nvm exec lts/argon`
  - `nvm run --lts` / `nvm run --lts=argon` / `nvm run 'lts/*'` / `nvm run lts/argon`
  - `nvm ls-remote --lts` / `nvm ls-remote --lts=argon` `nvm ls-remote 'lts/*'` / `nvm ls-remote lts/argon`
  - `nvm version-remote --lts` / `nvm version-remote --lts=argon` / `nvm version-remote 'lts/*'` / `nvm version-remote lts/argon`

Any time your local copy of `nvm` connects to https://nodejs.org, it will re-create the appropriate local aliases for all available LTS lines. These aliases (stored under `$NVM_DIR/alias/lts`), are managed by `nvm`, and you should not modify, remove, or create these files - expect your changes to be undone, and expect meddling with these files to cause bugs that will likely not be supported.

To get the latest LTS version of node and migrate your existing installed packages, use:

```sh
nvm install --reinstall-packages-from=current 'lts/*'
```

### Migrating Global Packages While Installing

If you want to install a new version of Node.js and migrate npm packages from a previous version:

```sh
nvm install --reinstall-packages-from=node node
```

This will first use "nvm version node" to identify the current version you're migrating packages from. Then it resolves the new version to install from the remote server and installs it. Lastly, it runs "nvm reinstall-packages" to reinstall the npm packages from your prior version of Node to the new one.

You can also install and migrate npm packages from specific versions of Node like this:

```sh
nvm install --reinstall-packages-from=5 6
nvm install --reinstall-packages-from=iojs v4.2
```

Note that reinstalling packages _explicitly does not update the npm version_ — this is to ensure that npm isn't accidentally upgraded to a broken version for the new node version.

To update npm at the same time add the `--latest-npm` flag, like this:

```sh
nvm install --reinstall-packages-from=default --latest-npm 'lts/*'
```

or, you can at any time run the following command to get the latest supported npm version on the current node version:
```sh
nvm install-latest-npm
```

If you've already gotten an error to the effect of "npm does not support Node.js", you'll need to (1) revert to a previous node version (`nvm ls` & `nvm use <your latest _working_ version from the ls>`), (2) delete the newly created node version (`nvm uninstall <your _broken_ version of node from the ls>`), then (3) rerun your `nvm install` with the `--latest-npm` flag.

### Migrating Global Packages Between Installed Versions

`--reinstall-packages-from` is tied to `nvm install`. To migrate global npm packages between versions you _already_ have installed, without (re)installing anything, `nvm use` the destination and run `nvm reinstall-packages` as a standalone command, pointing at the version you want to copy _from_:

```sh
nvm use 22.22.2
nvm reinstall-packages 22.20.0
```

This reinstalls all global packages from `22.20.0` into the currently-active version (`22.22.2`). As with `--reinstall-packages-from`, the npm version itself is not changed.


### Offline Install

If you've previously downloaded a node version (or it's still in the cache), you can install it without any network access using the `--offline` flag:

```sh
nvm install --offline 14.7.0
```

This resolves versions using only locally installed versions and cached downloads. It will not attempt to download anything. This is useful in air-gapped environments, on planes, or when you want to avoid network latency.

You can combine `--offline` with `--lts` to install the latest cached LTS version (as long as LTS aliases have been populated by a prior `nvm ls-remote --lts`):

```sh
nvm install --offline --lts
```

### Default Global Packages From File While Installing

If you have a list of default packages you want installed every time you install a new version, we support that too -- just add the package names, one per line, to the file `$NVM_DIR/default-packages`. You can add anything npm would accept as a package argument on the command line.

```sh
# $NVM_DIR/default-packages

rimraf
object-inspect@1.0.2
stevemao/left-pad
```

### io.js

> [!WARNING]
> io.js was a [fork of Node.js](https://en.wikipedia.org/wiki/Node.js#History), created in 2014 and merged back in 2015. io.js shipped v1, v2, and v3 release lines; post-merge, node.js began releasing with v4.

If you want to install io.js:

```sh
nvm install iojs
```

If you want to install a new version of io.js and migrate npm packages from a previous version:

```sh
nvm install --reinstall-packages-from=iojs iojs
```

The same guidelines mentioned for migrating npm packages in node are applicable to io.js.

### System Version of Node

If you want to use the system-installed version of node, you can use the special default alias "system":

```sh
nvm use system
nvm run system --version
```

### Listing Versions

If you want to see what versions are installed:

```sh
nvm ls
```

If you want to see what versions are available to install:

```sh
nvm ls-remote
```

### Setting Custom Colors

You can set five colors that will be used to display version and alias information. These colors replace the default colors.
  Initial colors are: g b y r e

  Color codes:

    r/R = red / bold red

    g/G = green / bold green

    b/B = blue / bold blue

    c/C = cyan / bold cyan

    m/M = magenta / bold magenta

    y/Y = yellow / bold yellow

    k/K = black / bold black

    e/W = light grey / white

```sh
nvm set-colors rgBcm
```

#### Persisting custom colors

If you want the custom colors to persist after terminating the shell, export the `NVM_COLORS` variable in your shell profile. For example, if you want to use cyan, magenta, green, bold red and bold yellow, add the following line:

```sh
export NVM_COLORS='cmgRY'
```

#### Suppressing colorized output

`nvm help (or -h or --help)`, `nvm ls`, `nvm ls-remote` and `nvm alias` usually produce colorized output. You can disable colors with the `--no-colors` option (or by setting the environment variable `TERM=dumb`):

```sh
nvm ls --no-colors
nvm help --no-colors
TERM=dumb nvm ls
```

### Restoring PATH
To restore your PATH, you can deactivate it:

```sh
nvm deactivate
```

### Set default node version
To set a default Node version to be used in any new shell, use the alias 'default':

```sh
nvm alias default node # this refers to the latest installed version of node
nvm alias default 18 # this refers to the latest installed v18.x version of node
nvm alias default 18.12  # this refers to the latest installed v18.12.x version of node
```

### Use a mirror of node binaries
To use a mirror of the node binaries, set `$NVM_NODEJS_ORG_MIRROR`:

```sh
export NVM_NODEJS_ORG_MIRROR=https://nodejs.org/dist
nvm install node

NVM_NODEJS_ORG_MIRROR=https://nodejs.org/dist nvm install 4.2
```

To use a mirror of the io.js binaries, set `$NVM_IOJS_ORG_MIRROR`:

```sh
export NVM_IOJS_ORG_MIRROR=https://iojs.org/dist
nvm install iojs-v1.0.3

NVM_IOJS_ORG_MIRROR=https://iojs.org/dist nvm install iojs-v1.0.3
```

`nvm use` will not, by default, create a "current" symlink. Set `$NVM_SYMLINK_CURRENT` to "true" to enable this behavior, which is sometimes useful for IDEs. Note that using `nvm` in multiple shell tabs with this environment variable enabled can cause race conditions.

#### Pass Authorization header to mirror
To pass an Authorization header through to the mirror url, set `$NVM_AUTH_HEADER`

```sh
NVM_AUTH_HEADER="Bearer secret-token" nvm install node
```

### .nvmrc

You can create a `.nvmrc` file containing a node version number (or any other string that `nvm` understands; see `nvm --help` for details) in the project root directory (or any parent directory).
Afterwards, `nvm use`, `nvm install`, and `nvm which` will use the version specified in the `.nvmrc` file if no version is supplied on the command line; if no `.nvmrc` is found either, they exit with status `127`. (`nvm exec` and `nvm run` follow the same `.nvmrc` lookup, but currently fall back to the active node if neither resolves &mdash; treat that fallback as undefined behavior; pass an explicit version if you need predictable scripting.) If you want the currently active version, pass `current` explicitly (e.g. `nvm which current`) &mdash; `current` is not affected by `.nvmrc`.

For example, to make nvm default to the latest 5.9 release, the latest LTS version, or the latest node version for the current directory:

```sh
$ echo "5.9" > .nvmrc

$ echo "lts/*" > .nvmrc # to default to the latest LTS version

$ echo "node" > .nvmrc # to default to the latest version
```

[NB these examples assume a POSIX-compliant shell version of `echo`. If you use a Windows `cmd` development environment, eg the `.nvmrc` file is used to configure a remote Linux deployment, then keep in mind the `"`s will be copied leading to an invalid file. Remove them.]

Then when you run nvm use:

```sh
$ nvm use
Found '/path/to/project/.nvmrc' with version <5.9>
Now using node v5.9.1 (npm v3.7.3)
```

Running nvm install will also switch over to the correct version, but if the correct node version isn't already installed, it will install it for you.

```sh
$ nvm install
Found '/path/to/project/.nvmrc' with version <5.9>
Downloading and installing node v5.9.1...
Downloading https://nodejs.org/dist/v5.9.1/node-v5.9.1-linux-x64.tar.xz...
#################################################################################### 100.0%
Computing checksum with sha256sum
Checksums matched!
Now using node v5.9.1 (npm v3.7.3)
```

`nvm use` et. al. will traverse directory structure upwards from the current directory looking for the `.nvmrc` file. In other words, running `nvm use` et. al. in any subdirectory of a directory with an `.nvmrc` will result in that `.nvmrc` being utilized.

The contents of a `.nvmrc` file **must** contain precisely one `<version>` (as described by `nvm --help`) followed by a newline. `.nvmrc` files may also have comments. The comment delimiter is `#`, and it and any text after it, as well as blank lines, and leading and trailing white space, will be ignored when parsing.

Key/value pairs using `=` are also allowed and ignored, but are reserved for future use, and may cause validation errors in the future.

Run [`npx nvmrc`](https://npmjs.com/nvmrc) to validate an `.nvmrc` file. If that tool’s results do not agree with nvm, one or the other has a bug - please file an issue.

### Deeper Shell Integration

You can use [`nvshim`](https://github.com/iamogbz/nvshim) to shim the `node`, `npm`, and `npx` bins to automatically use the `nvm` config in the current directory. `nvshim` is **not** supported by the `nvm` maintainers. Please [report issues to the `nvshim` team](https://github.com/iamogbz/nvshim/issues/new).

If you prefer a lighter-weight solution, the recipes below have been contributed by `nvm` users. They are **not** supported by the `nvm` maintainers. We are, however, accepting pull requests for more examples.

#### Calling `nvm use` automatically in a directory with a `.nvmrc` file

In your profile (`~/.bash_profile`, `~/.zshrc`, `~/.profile`, or `~/.bashrc`), add the following to `nvm use` whenever you enter a new directory:

##### bash

Put the following at the end of your `$HOME/.bashrc`:

```bash
cdnvm() {
    command cd "$@" || return $?
    nvm_path="$(nvm_find_up .nvmrc | command tr -d '\n')"

    # If there are no .nvmrc file, use the default nvm version
    if [[ ! $nvm_path = *[^[:space:]]* ]]; then

        declare default_version
        default_version="$(nvm version default)"

        # If there is no default version, set it to `node`
        # This will use the latest version on your machine
        if [ $default_version = 'N/A' ]; then
            nvm alias default node
            default_version=$(nvm version default)
        fi

        # If the current version is not the default version, set it to use the default version
        if [ "$(nvm current)" != "${default_version}" ]; then
            nvm use default
        
        fi
    fi
}

alias cd='cdnvm'
cdnvm "$PWD" || exit
```

This alias would search 'up' from your current directory in order to detect a `.nvmrc` file. If it finds it, it will switch to that version; if not, it will use the default version.

##### zsh

This shell function will install (if needed) and `nvm use` the specified Node version when an `.nvmrc` is found, and `nvm use default` otherwise.

Put this into your `$HOME/.zshrc` to call `nvm use` automatically whenever you enter a directory that contains an
`.nvmrc` file with a string telling nvm which node to `use`:

```zsh
# place this after nvm initialization!
autoload -U add-zsh-hook

load-nvmrc() {
  local nvmrc_path
  nvmrc_path="$(nvm_find_nvmrc)"

  if [ -n "$nvmrc_path" ]; then
    local nvmrc_node_version
    nvmrc_node_version=$(nvm version "$(cat "${nvmrc_path}")")

    if [ "$nvmrc_node_version" = "N/A" ]; then
      nvm install
    elif [ "$nvmrc_node_version" != "$(nvm version)" ]; then
      nvm use
    fi
  elif [ -n "$(PWD=$OLDPWD nvm_find_nvmrc)" ] && [ "$(nvm version)" != "$(nvm version default)" ]; then
    echo "Reverting to nvm default version"
    nvm use default
  fi
}

add-zsh-hook chpwd load-nvmrc
load-nvmrc
```

After saving the file, run `source ~/.zshrc` to reload the configuration with the latest changes made.

##### fish

This requires that you have [bass](https://github.com/edc/bass) installed.
```fish
# ~/.config/fish/functions/nvm.fish
function nvm
  bass source ~/.nvm/nvm.sh --no-use ';' nvm $argv
end

# ~/.config/fish/functions/nvm_find_nvmrc.fish
function nvm_find_nvmrc
  bass source ~/.nvm/nvm.sh --no-use ';' nvm_find_nvmrc
end

# ~/.config/fish/functions/load_nvm.fish
function load_nvm --on-variable="PWD"
  set -l default_node_version (nvm version default)
  set -l node_version (nvm version)
  set -l nvmrc_path (nvm_find_nvmrc)
  if test -n "$nvmrc_path"
    set -l nvmrc_node_version (nvm version (cat $nvmrc_path))
    if test "$nvmrc_node_version" = "N/A"
      nvm install (cat $nvmrc_path)
    else if test "$nvmrc_node_version" != "$node_version"
      nvm use $nvmrc_node_version
    end
  else if test "$node_version" != "$default_node_version"
    echo "Reverting to default Node version"
    nvm use default
  end
end

# ~/.config/fish/config.fish
# You must call it on initialization or listening to directory switching won't work
load_nvm > /dev/stderr
```

## Running Tests

Tests are written in [Urchin]. Install Urchin (and other dependencies) like so:

    npm install

There are slow tests and fast tests. The slow tests do things like install node
and check that the right versions are used. The fast tests fake this to test
things like aliases and uninstalling. From the root of the nvm git repository,
run the fast tests like this:

    npm run test/fast

Run the slow tests like this:

    npm run test/slow

Run all of the tests like this:

    npm test

Nota bene: Avoid running nvm while the tests are running.

## Environment variables

nvm exposes the following environment variables:

- `NVM_DIR` - nvm's installation directory.
- `NVM_BIN` - where node, npm, and global packages for the active version of node are installed.
- `NVM_INC` - node's include file directory (useful for building C/C++ addons for node).
- `NVM_CD_FLAGS` - used to maintain compatibility with zsh.
- `NVM_RC_VERSION` - version from .nvmrc file if being used.

Additionally, nvm modifies `PATH`, and, if present, `MANPATH` and `NODE_PATH` when changing versions.

The following environment variables can be set to configure `nvm install`:

- `NVM_NO_SOURCE_FALLBACK` - when `1`, a failed binary download aborts instead of silently falling back to a (much slower) from-source compile; the persistent equivalent of the `-b` flag, and mutually exclusive with `-s`.
- `NVM_INSTALL_LOCK_TIMEOUT` - seconds to wait for a concurrent install of the same version to finish before giving up (default `600`). On timeout, nvm prints the lock path so a lock left behind by a killed install can be removed.
- `NVM_INSTALL_LOCK_STALE` - minutes after which an install lock is assumed abandoned and stolen automatically; `0` (the default) never steals.

`nvm install <version>` takes a per-version advisory lock (a directory under `$NVM_DIR/.cache/locks`), so two shells installing the same version 
```

  - If setting the `default` alias does not establish the node version in new shells (i.e. `nvm current` yields `system`), ensure that the system's node `PATH` is set before the `nvm.sh` source line in your shell profile (see [#658](https://github.com/nvm-sh/nvm/issues/658))

## macOS Troubleshooting

**nvm node version not found in [vim](https://www.vim.org) shell**

If you set node version to a version other than your system node version `nvm use 6.2.1` and open vim and run `:!node -v` you should see `v6.2.1` if you see your system version `v0.12.7`. You need to run:

```shell
sudo chmod ugo-x /usr/libexec/path_helper
```

More on this issue in [dotphiles/dotzsh](https://github.com/dotphiles/dotzsh#mac-os-x).

**nvm is not compatible with the npm config "prefix" option**

Some solutions for this issue can be found [here](https://github.com/nvm-sh/nvm/issues/1245)

There is one more edge case causing this issue, and that's a **mismatch between the `$HOME` path and the user's home directory's actual name**.

You have to make sure that the user directory name in `$HOME` and the user directory name you'd see from running `ls /Users/` **are capitalized the same way** ([See this issue](https://github.com/nvm-sh/nvm/issues/2261)).

To change the user directory and/or account name follow the instructions [here](https://support.apple.com/en-us/HT201548)

[1]: https://github.com/nvm-sh/nvm.git
[2]: https://github.com/nvm-sh/nvm/blob/v0.40.6/install.sh
[3]: https://github.com/nvm-sh/nvm/actions/workflows/tests-fast.yml
[4]: https://github.com/nvm-sh/nvm/releases/tag/v0.40.6
[Urchin]: https://git.sdf.org/tlevine/urchin
[Fish]: https://fishshell.com

**Homebrew makes zsh directories insecure**

```shell
zsh compinit: insecure directories, run compaudit for list.
Ignore insecure directories and continue [y] or abort compinit [n]? y
```

Homebrew causes insecure directories like `/usr/local/share/zsh/site-functions` and `/usr/local/share/zsh`. This is **not** an `nvm` problem - it is a homebrew problem. Refer [here](https://github.com/zsh-users/zsh-completions/issues/680) for some solutions related to the issue.

**Macs with Apple Silicon chips**

Experimental support for the Apple Silicon chip architecture was added in node.js v15.3 and full support was added in v16.0.
Because of this, if you try to install older versions of node as usual, you will probably experience either compilation errors when installing node or out-of-memory errors while running your code.

So, if you want to run a version prior to v16.0 on an Apple Silicon Mac, it may be best to compile node targeting the `x86_64` Intel architecture so that [Rosetta 2](https://support.apple.com/en-us/HT211861) can translate the `x86_64` processor instructions to ARM-based Apple Silicon instructions.
Here's what you will need to do:

- Install Rosetta, if you haven't already done so

  ```sh
  $ softwareupdate --install-rosetta
  ```

  You might wonder, "how will my Apple Silicon Mac know to use Rosetta for a version of node compiled for an Intel chip?".
  If an executable contains only Intel instructions, macOS will automatically use Rosetta to translate the instructions.

- Open a shell that's running using Rosetta

  ```sh
  $ arch -x86_64 zsh
  ```

  Note: This same thing can also be accomplished by finding the Terminal or [iTerm](https://iterm2.com) App in Finder, right clicking, selecting "Get Info", and then checking the box labeled "Open using Rosetta".

  Note: This terminal session is now running in `zsh`.
  If `zsh` is not the shell you typically use, `nvm` may not be `source`'d automatically like it probably is for your usual shell through your dotfiles.
  If that's the case, make sure to source `nvm`.

  ```sh
  $ source "${NVM_DIR}/nvm.sh"
  ```

- Install whatever older version of node you are interested in. Let's use 12.22.1 as an example.
  This will fetch the node source code and compile it, which will take several minutes.

  ```sh
  $ nvm install v12.22.1 --shared-zlib
  ```

  Note: You're probably curious why `--shared-zlib` is included.
  There's a bug in recent versions of Apple's system [`clang`](https://clang.llvm.org/) compiler.
  If one of these broken versions is installed on your system, the above step will likely still succeed even if you didn't include the `--shared-zlib` flag.
  However, later, when you attempt to `npm install` something using your old version of node.js, you will see `incorrect data check` errors.
  If you want to avoid the possible hassle of dealing with this, include that flag.
  For more details, see [this issue](https://github.com/nodejs/node/issues/39313) and [this comment](https://github.com/nodejs/node/issues/39313#issuecomment-90.40.676)

- Exit back to your native shell.

  ```sh
  $ exit
  $ arch
  arm64
  ```

  Note: If you selected the box labeled "Open using Rosetta" rather than running the CLI command in the second step, you will see `i386` here.
  Unless you have another reason to have that box selected, you can deselect it now.

- Check to make sure the architecture is correct. `x64` is the abbreviation for `x86_64`, which is what you want to see.

  ```sh
  $ node -p process.arch
  x64
  ```

Now you should be able to use node as usual.

## WSL Troubleshooting

If you've encountered this error on WSL-2:

  ```sh
  curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.6/install.sh | bash
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                  Dload  Upload  Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:--  0:00:09 --:--:--     0curl: (6) Could not resolve host: raw.githubusercontent.com
  
