---
layout: default
title: npm cheatsheet
parent: cheatsheets
nav_order: 3
---
# NPM cheatsheet
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---
## see [NPM cheatsheet](https://devhints.io/npm)

## Install NVM (Node Version Manager)
Node version manager is a script to manage multiple active node.js versions.
see [Installing Node.js with `nvm` to Linux & macOS & WSL](https://gist.github.com/d2s/372b5943bce17b964a79)

### NVM On macOS

Install NVM on macOS using brew
````bash
$ brew install nvm
````

Create a folder for the current user `.nvm` where the files will reside.
````bash
$ mkdir ~/.nvm
````

Install the last LTS version
````bash
$ nvm install --lts
````

You can select Node.js version by running `nvm use v16.14.0` (or another version number). Another alternative: create a small Bash shell script to enable the right environment variables for your project.
````bash
$ nvm use node
````

Check if everything is working as expected
````bash
$ nvm run node --version
````

Create two user environment variables `NVM_DIR` and `NVM_HOMEBREW`
````bash
export NVM_DIR="$HOME/.nvm"
NVM_HOMEBREW="/usr/local/opt/nvm/nvm.sh"
[ -s "$NVM_HOMEBREW" ] && \. "$NVM_HOMEBREW"
````

The `NODE_PATH` variable can also be useful for some applications, for example, VScode, this is the code you need to put after the previous variables.
````bash
[ -x "$(command -v npm)" ] && export NODE_PATH=$NODE_PATH:`npm root -g`
````

### NVM On Ubuntu or macOS
````bash
$ curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.2/install.sh | bash
````

### NVM Tips And Tricks
````bash
$ nvm ls-remote                 # lists all of the available versions of NodeJs & iojs
$ nvm ls                        # list locally installed version
$ nvm install 0.12.3            # install the version 0.12.3 (see ls-remote for available options)
$ nvm use 0.12.3                # switch to and use the installed 0.12.3 version
$ nvm which 0.12.2              # the path to the installed node version
$ nvm current                   # what is the current installed nvm version
$ nvm alias default 0.10.32     # set the default node to the installed 0.10.32 version
$ nvm --help                    # the help documents
````

---
## Package management
## Listing
## Misc features
## Install names
## Updating
