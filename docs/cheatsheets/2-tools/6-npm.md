---
layout: default
title: npm
parent: tools
grand_parent: cheatsheets
permalink: /docs/tools/npm
nav_order: 6
---

# NPM cheatsheet
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---
## see [NPM cheatsheet](https://devhints.io/npm)

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
`npm i`	Alias for npm install
`npm install`	Install everything in package.json
`npm install` --production	Install everything in package.json, except devDependecies
`npm install lodash`	Install a package
`npm install --save-dev lodash`	Install as devDependency
`npm install --save-exact lodash`	Install with exact
`npm version 1.2.3`	Bump the package version to 1.2.3
`npm version major`	Bump the major package version by 1 (1.2.3 → 2.0.0)
`npm version minor`	Bump the minor package version by 1 (1.2.3 → 1.3.0)
`npm version patch`	Bump the patch package version by 1 (1.2.3 → 1.2.4)

--save is the default as of npm@5. Previously, using npm install without --save doesn’t update package.json.

## Listing
## Misc features
## Install names
## Updating
