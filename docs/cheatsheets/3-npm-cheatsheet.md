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

### NVM Usage
````bash
Usage:
  nvm --help                                  Show this message
    --no-colors                               Suppress colored output
  nvm --version                               Print out the installed version of nvm
  nvm install [<version>]                     Download and install a <version>. Uses .nvmrc if available and version is omitted.
   The following optional arguments, if provided, must appear directly after `nvm install`:
    -s                                        Skip binary download, install from source only.
    -b                                        Skip source download, install from binary only.
    --reinstall-packages-from=<version>       When installing, reinstall packages installed in <node|iojs|node version number>
    --lts                                     When installing, only select from LTS (long-term support) versions
    --lts=<LTS name>                          When installing, only select from versions for a specific LTS line
    --skip-default-packages                   When installing, skip the default-packages file if it exists
    --latest-npm                              After installing, attempt to upgrade to the latest working npm on the given node version
    --no-progress                             Disable the progress bar on any downloads
    --alias=<name>                            After installing, set the alias specified to the version specified. (same as: nvm alias <name> <version>)
    --default                                 After installing, set default alias to the version specified. (same as: nvm alias default <version>)
  nvm uninstall <version>                     Uninstall a version
  nvm uninstall --lts                         Uninstall using automatic LTS (long-term support) alias `lts/*`, if available.
  nvm uninstall --lts=<LTS name>              Uninstall using automatic alias for provided LTS line, if available.
  nvm use [<version>]                         Modify PATH to use <version>. Uses .nvmrc if available and version is omitted.
   The following optional arguments, if provided, must appear directly after `nvm use`:
    --silent                                  Silences stdout/stderr output
    --lts                                     Uses automatic LTS (long-term support) alias `lts/*`, if available.
    --lts=<LTS name>                          Uses automatic alias for provided LTS line, if available.
  nvm exec [<version>] [<command>]            Run <command> on <version>. Uses .nvmrc if available and version is omitted.
   The following optional arguments, if provided, must appear directly after `nvm exec`:
    --silent                                  Silences stdout/stderr output
    --lts                                     Uses automatic LTS (long-term support) alias `lts/*`, if available.
    --lts=<LTS name>                          Uses automatic alias for provided LTS line, if available.
  nvm run [<version>] [<args>]                Run `node` on <version> with <args> as arguments. Uses .nvmrc if available and version is omitted.
   The following optional arguments, if provided, must appear directly after `nvm run`:
    --silent                                  Silences stdout/stderr output
    --lts                                     Uses automatic LTS (long-term support) alias `lts/*`, if available.
    --lts=<LTS name>                          Uses automatic alias for provided LTS line, if available.
  nvm current                                 Display currently activated version of Node
  nvm ls [<version>]                          List installed versions, matching a given <version> if provided
    --no-colors                               Suppress colored output
    --no-alias                                Suppress `nvm alias` output
  nvm ls-remote [<version>]                   List remote versions available for install, matching a given <version> if provided
    --lts                                     When listing, only show LTS (long-term support) versions
    --lts=<LTS name>                          When listing, only show versions for a specific LTS line
    --no-colors                               Suppress colored output
  nvm version <version>                       Resolve the given description to a single local version
  nvm version-remote <version>                Resolve the given description to a single remote version
    --lts                                     When listing, only select from LTS (long-term support) versions
    --lts=<LTS name>                          When listing, only select from versions for a specific LTS line
  nvm deactivate                              Undo effects of `nvm` on current shell
    --silent                                  Silences stdout/stderr output
  nvm alias [<pattern>]                       Show all aliases beginning with <pattern>
    --no-colors                               Suppress colored output
  nvm alias <name> <version>                  Set an alias named <name> pointing to <version>
  nvm unalias <name>                          Deletes the alias named <name>
  nvm install-latest-npm                      Attempt to upgrade to the latest working `npm` on the current node version
  nvm reinstall-packages <version>            Reinstall global `npm` packages contained in <version> to current version
  nvm unload                                  Unload `nvm` from shell
  nvm which [current | <version>]             Display path to installed node version. Uses .nvmrc if available and version is omitted.
    --silent                                  Silences stdout/stderr output when a version is omitted
  nvm cache dir                               Display path to the cache directory for nvm
  nvm cache clear                             Empty cache directory for nvm
  nvm set-colors [<color codes>]              Set five text colors using format "yMeBg". Available when supported.
                                               Initial colors are:
                                                   b  y  g  r  e
                                               Color codes:
                                                 r/R = red / bold red
                                                 g/G = green / bold green
                                                 b/B = blue / bold blue
                                                 c/C = cyan / bold cyan
                                                 m/M = magenta / bold magenta
                                                 y/Y = yellow / bold yellow
                                                 k/K = black / bold black
                                                 e/W = light grey / white

Example:
  nvm install 8.0.0                     Install a specific version number
  nvm use 8.0                           Use the latest available 8.0.x release
  nvm run 6.10.3 app.js                 Run app.js using node 6.10.3
  nvm exec 4.8.3 node app.js            Run `node app.js` with the PATH pointing to node 4.8.3
  nvm alias default 8.1.0               Set default node version on a shell
  nvm alias default node                Always default to the latest available node version on a shell

  nvm install node                      Install the latest available version
  nvm use node                          Use the latest version
  nvm install --lts                     Install the latest LTS version
  nvm use --lts                         Use the latest LTS version

  nvm set-colors cgYmW                  Set text colors to cyan, green, bold yellow, magenta, and white
````

---
## NPM Ussage
````bash
Usage:

npm install        install all the dependencies in your project
npm install <foo>  add the <foo> dependency to your project
npm test           run this project's tests
npm run <foo>      run the script named <foo>
npm <command> -h   quick help on <command>
npm -l             display usage info for all commands
npm help <term>    search for help on <term>
npm help npm       more involved overview

All commands:

    access, adduser, audit, bugs, cache, ci, completion,
    config, dedupe, deprecate, diff, dist-tag, docs, doctor,
    edit, exec, explain, explore, find-dupes, fund, get, help,
    hook, init, install, install-ci-test, install-test, link,
    ll, login, logout, ls, org, outdated, owner, pack, ping,
    pkg, prefix, profile, prune, publish, query, rebuild, repo,
    restart, root, run-script, search, set, shrinkwrap, star,
    stars, start, stop, team, test, token, uninstall, unpublish,
    unstar, update, version, view, whoami

Specify configs in the ini-formatted file:
    /Users/cjrequena/.npmrc
or on the command line via: npm <command> --key=value

More configuration info: npm help config
Configuration fields: npm help 7 config
````
## Package management
## Listing
## Misc features
## Install names
## Updating
