---
layout: default
title: sdkman
parent: tools
grand_parent: cheatsheets
permalink: /docs/tools/sdkman
nav_order: 8
---

# SDKMAN! Cheat Sheet (macOS & Linux)
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

> SDKMAN! = Software Development Kit Manager
> Great for managing parallel versions of **Java, Kotlin, Scala, Maven, Gradle, etc.**

---

## Installation

### 1. Prerequisites

* Bash or Zsh
* `curl`, `zip`, `unzip`
* Linux or macOS (Intel and Apple Silicon)

### 2. Install SDKMAN!

```bash
curl -s "https://get.sdkman.io" | bash
```

Then load it in the current shell (or reopen the terminal):

```bash
source "$HOME/.sdkman/bin/sdkman-init.sh"
sdk version
```

The installer appends this block to `~/.zshrc` / `~/.bashrc`. It **must stay at the end of the file**:

```bash
#THIS MUST BE AT THE END OF THE FILE FOR SDKMAN TO WORK!!!
export SDKMAN_DIR="$HOME/.sdkman"
[[ -s "$HOME/.sdkman/bin/sdkman-init.sh" ]] && source "$HOME/.sdkman/bin/sdkman-init.sh"
```

---

## Basic Commands

### List SDKs

```bash
sdk list          # all candidates
sdk list java     # versions of one candidate (installed ones are marked)
```

### Install an SDK

```bash
sdk install java 21-tem
sdk install gradle 8.5
sdk install java                       # latest default version
sdk install java my-jdk /path/to/jdk   # register a local install
```

### Default vs. current shell

| Goal                         | Command                    | Scope                 |
| ---------------------------- | -------------------------- | --------------------- |
| Make a version the default   | `sdk default java 21-tem`  | All new shells        |
| Try a version temporarily    | `sdk use java 17.0.8-tem`  | Current shell only    |

### Check Current Versions

```bash
sdk current         # all candidates in use
sdk current java    # one candidate
```

### Locate an SDK (e.g. for `JAVA_HOME`)

```bash
sdk home java 21-tem
```

### Uninstall an SDK

```bash
sdk uninstall java 17.0.8-tem
```

---

## Java Identifiers

Format: `<version>-<vendor>`. Pick from the **Identifier** column of `sdk list java`.

| Identifier  | Distribution            |
| ----------- | ----------------------- |
| `21-tem`    | Eclipse Temurin         |
| `21-zulu`   | Azul Zulu               |
| `21-amzn`   | Amazon Corretto         |
| `21-graalce`| GraalVM Community       |
| `21-open`   | OpenJDK (jdk.java.net)  |

---

## Per-Project SDK Version

Generate a `.sdkmanrc` in the project directory:

```bash
sdk env init
```

Or write it by hand:

```bash
java=17.0.8-tem
gradle=8.5
```

### Use SDKs from `.sdkmanrc`

```bash
sdk env install  # install missing versions
sdk env          # switch to versions defined in .sdkmanrc
sdk env clear    # restore defaults
```

To switch automatically on `cd`, set `sdkman_auto_env=true` (see [Configuration](#configuration-file-sdkmanetcconfig)).

---

## Supported SDKs (Common)

| SDK            | Command                    |
| -------------- | -------------------------- |
| Java           | `sdk install java`         |
| Kotlin         | `sdk install kotlin`       |
| Scala          | `sdk install scala`        |
| Groovy         | `sdk install groovy`       |
| Gradle         | `sdk install gradle`       |
| Maven          | `sdk install maven`        |
| Maven Daemon   | `sdk install mvnd`         |
| Ant            | `sdk install ant`          |
| sbt            | `sdk install sbt`          |
| Spring Boot    | `sdk install springboot`   |
| Micronaut      | `sdk install micronaut`    |
| Quarkus CLI    | `sdk install quarkus`      |
| JBang          | `sdk install jbang`        |

Check `sdk list` to explore more.

---

## SDKMAN! CLI Tips

| Task                              | Command           |
| --------------------------------- | ----------------- |
| Show help                         | `sdk help`        |
| Show installed versions           | `sdk list <sdk>`  |
| Show versions in use              | `sdk current`     |
| Upgrade outdated installed SDKs   | `sdk upgrade`     |
| Update SDKMAN! itself             | `sdk selfupdate`  |
| Refresh the candidate list        | `sdk update`      |
| Clear caches (stale candidates)   | `sdk flush`       |
| Work without network              | `sdk offline`     |

---

## Configuration File (`~/.sdkman/etc/config`)

Open it with `sdk config`. Common options:

* `sdkman_auto_env=true` – switch to `.sdkmanrc` versions on `cd`
* `sdkman_auto_answer=true` – never prompt (useful in scripts/CI)
* `sdkman_colour_enable=false` – disable coloured output
* `sdkman_auto_complete=true` – enable tab completion

---

## Troubleshooting

| Problem                          | Solution                                                          |
| -------------------------------- | ----------------------------------------------------------------- |
| SDK not available                | Run `sdk list <sdk>` to check name/version                        |
| `sdk: command not found`         | Add the init block at the **end** of `~/.zshrc` / `~/.bashrc`     |
| `zip`/`unzip` not found          | Install them (`brew install zip unzip` / `apt install zip unzip`) |
| Candidate or version list stale  | `sdk flush` then `sdk update`                                     |
| `.sdkmanrc` ignored              | Run `sdk env`, or set `sdkman_auto_env=true` via `sdk config`     |
| Conflicting SDK versions         | Use `sdk use` or `.sdkmanrc` locally                              |
| `JAVA_HOME` not set              | Open a new shell after `sdk default java <v>`; check `sdk home`   |

---

## Uninstall SDKMAN!

```bash
rm -rf ~/.sdkman
```

Then delete this block from `~/.zshrc` / `~/.bashrc`:

```bash
#THIS MUST BE AT THE END OF THE FILE FOR SDKMAN TO WORK!!!
export SDKMAN_DIR="$HOME/.sdkman"
[[ -s "$HOME/.sdkman/bin/sdkman-init.sh" ]] && source "$HOME/.sdkman/bin/sdkman-init.sh"
```

---

## Best Practices

* Commit a `.sdkmanrc` in every project and pin exact versions for reproducible builds.
* Run `sdk selfupdate` and `sdk upgrade` periodically.
* Favor **Temurin (tem)** or **Zulu** distributions for Java.

---
