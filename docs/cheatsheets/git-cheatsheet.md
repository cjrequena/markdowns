---
layout: default
title: Git cheatsheet
parent: Cheatsheets
nav_order: 2
---
# Git cheatsheet
{: .no_toc }
This cheat sheet features the most important and commonly used git commands for easy reference.
Git Tips and Tricks (A collection of useful tips and tricks for Git)


## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## [Git Cheat Sheet PDF](https://education.github.com/git-cheat-sheet-education.pdf)

## Configuring user information used across all local repositories

### Set a name that is identifiable for credit when review version history
```sh
$ git config --global user.name "[firstname lastname]"
```
### Set an email address that will be associated with each history marker
```sh
$ git config --global user.email "[valid-email]"
```
### Set automatic command line coloring for Git for easy reviewing
```sh
$ git config --global color.ui auto
```

## Branch

### List your branches
```sh
$ git branch
```
### Create a new branch at the current commit
```sh
$ git branch [branch-name]
```
### Switch to another branch and check it out into your working directory
```sh
$ git checkout
```
### Merge the specified branch’s history into the current one
```sh
$ git merge [branch]
```
### Show all commits in the current branch’s history
```sh
$ git log
```
### Delete remote branch
```sh
$ git branch -d [branch]
$ git push origin :[branch]
```

## Rewriting the history
### Delete last commit
```sh
$ git reset --hard HEAD^
$ git push -f
```

### Delete last two commit
```sh
$ git reset --hard HEAD~2
$ git push -f
```
