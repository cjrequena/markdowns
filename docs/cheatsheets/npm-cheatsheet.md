---
layout: default
title: NPM cheatsheet
parent: Cheatsheets
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

## Install NVM (Node Version Manager) on MacOS

Node version manager is a script to manage multiple active node.js versions. 
see [Installing Node.js with `nvm` to Linux & macOS & WSL](https://gist.github.com/d2s/372b5943bce17b964a79)

Install NVM on MacOS using brew
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

**Create two user environment variables `NVM_DIR` and `NVM_HOMEBREW`.**
````bash
export NVM_DIR="$HOME/.nvm"
NVM_HOMEBREW="/usr/local/opt/nvm/nvm.sh"
[ -s "$NVM_HOMEBREW" ] && \. "$NVM_HOMEBREW"
````

**The `NODE_PATH` variable can also be useful for some applications, for example, VScode, this is the code you need to put after the previous variables.**
````bash
[ -x "$(command -v npm)" ] && export NODE_PATH=$NODE_PATH:`npm root -g`
````
---
## Package management
## Listing
## Misc features
## Install names
## Updating
