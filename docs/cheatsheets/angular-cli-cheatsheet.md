---
layout: default
title: Angular-CLI cheatsheet
parent: Cheatsheets
nav_order: 5
---
# Angular-CLI cheatsheet
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---
## [CLI Overview and Command Reference](https://angular.io/cli)

## [Cheatsheet: Angular CLI Reference](https://www.digitalocean.com/community/tutorials/angular-angular-cli-reference?utm_source=pocket_mylist)

## Installing Angular CLI
```sh
$ npm install -g @angular/cli
```

## Checking the Version
```sh
$ ng --version
```

## Updating the Version
```sh
$ npm uninstall -g @angular/cli cache clean
$ npm install -g @angular/cli@latest
```

## Help
```sh
$ ng help # Get general help:
$ ng help generate # Or get help for a specific command:
```

## New Project
```sh
$ ng new my-app
$ ng new my-app --prefix yo --style scss --skip-tests --verbose # An example with a few flags
```
And here are a few flags you can use:
* --dry-run: See which files would be created, but don’t actually do anything.
* --verbose: Be more chatty.
* --skip-install: Don’t npm install, useful when offline or with slow internet.
* --skip-tests: Don’t create spec files.
* --skip-git: Don’t initialize a git repo.
* --source-dir: Name of the source directory
* --routing: Add routing to the app.
* --prefix: Specify the prefix to use for components selectors.
* --style: Defaults to css, but can be set to scss.
* --inline-style: Use inline styles for components instead of separate files.
* --inline-template: Use inline templates for components instead of separate files.

## Generate a component
```sh
$ ng g c unicorn-component
```

## Generate a service
```sh
$ ng g s everything-service
```

## Generate a pipe
```sh
$ ng g pipe my-pipe
```

## Generate a directive
```sh
$ ng g directive my-directive
```

## Generate an enum
```sh
$ ng g enum some-enum
```

## Generate a module
```sh
$ ng g module fancy-module
```

## Generate a class
```sh
$ ng g cl my-class
```

## Generate an interface
```sh
$ ng g interface my-interface
```

## Generate a route guard
```sh
$ ng g guard my-guard
```

*The --dry-run and --verbose flags can be used with any generate command.*

## Serving
```sh
$ ng s # Serve your project
$ ng s -o # Serve and open in your default browser automatically
$ ng s --port 5555 # Serve to a different port
```
