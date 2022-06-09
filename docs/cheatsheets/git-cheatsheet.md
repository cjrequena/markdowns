---
layout: default
title: Git cheat sheet
parent: Cheat sheets
nav_order: 2
---
# Git cheat sheet
{: .no_toc }
This cheat sheet features the most important and commonly used git commands for easy reference.
Git Tips and Tricks (A collection of useful tips and tricks for Git)


## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## [Github Git Cheat Sheet](https://education.github.com/git-cheat-sheet-education.pdf)

## SETUP
### Configuring user information used across all local repositories

**set a name that is identifiable for credit when review version history**
```sh
$ git config --global user.name "[firstname lastname]"
```
**set an email address that will be associated with each history marker**
```sh
$ git config --global user.email "[valid-email]"
```
**set automatic command line coloring for Git for easy reviewing**
```sh
$ git config --global color.ui auto
```

## BRANCH & MERGE
### Isolating work in branches, changing context, and integrating changes

**list your branches. a * will appear next to the currently active branch**
```sh
$ git branch
```
**create a new branch at the current commit**
```sh
$ git branch [branch-name]
```
**switch to another branch and check it out into your working directory**
```sh
$ git checkout
```
**merge the specified branch’s history into the current one**
```sh
$ git merge [branch]
```
**show all commits in the current branch’s history**
```sh
$ git log
```
**delete remote branch**
```sh
$ git branch -d [branch]
$ git push origin :[branch]
```

## Delete last commit (Rewriting the history)
```sh
$ git reset --hard HEAD^
$ git push -f
```

## Delete last two commit (Rewriting the history)
```sh
$ git reset --hard HEAD~2
$ git push -f
```
