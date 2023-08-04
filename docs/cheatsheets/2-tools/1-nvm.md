---
layout: default
title: nvm
parent: tools
grand_parent: cheatsheets
permalink: /docs/tools/nvm
nav_order: 1
---
# NVM
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Installing NVM (Node Version Manager)
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

### NVM On Ubuntu/Linux/macOS
1. Open Terminal.
2. Run the following command to install NVM:
   ```
   curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
   ```
3. Follow the instructions in the terminal to complete the installation.
4. Restart the Terminal or run `source ~/.bashrc` to start using NVM.

### Windows:
1. Download the latest installer from the NVM repository: [https://github.com/coreybutler/nvm-windows/releases](https://github.com/coreybutler/nvm-windows/releases)
2. Run the installer and follow the setup instructions.
3. Restart your computer to start using NVM.

## NVM Commands

## Install a specific Node.js version `nvm install <version>`
```bash
$ nvm install 14.17.0
```

- `nvm use <version>`: Use a specific Node.js version.
  Example: `nvm use 12.22.1`

- `nvm uninstall <version>`: Uninstall a specific Node.js version.
  Example: `nvm uninstall 10.15.3`

- `nvm ls`: List installed Node.js versions.

- `nvm ls-remote`: List available Node.js versions that can be installed.

- `nvm current`: Display the currently active Node.js version.

- `nvm alias <name> <version>`: Create an alias for a specific Node.js version.
  Example: `nvm alias default 14.17.0`

- `nvm use default`: Use the default Node.js version set by an alias.

- `nvm which <version>`: Display the path to a specific Node.js version.

- `nvm version`: Display the current NVM version.

- `nvm --version`: Display the current NVM version (alternative command).

Note: Replace `<version>` with the desired Node.js version (e.g., 14.17.0, 12.22.1).

That's it! You now have a cheat sheet for installing and using Node Version Manager (NVM) on different operating systems.

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
