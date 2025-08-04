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

* Bash, Zsh, or similar shell
* curl, unzip
* Unix-like OS (Linux/macOS)

### 2. Install SDKMAN!

```bash
curl -s "https://get.sdkman.io" | bash
```

Then:

```bash
source "$HOME/.sdkman/bin/sdkman-init.sh"
```

Or reopen the terminal.

---

## Basic Commands

### 🔍 List All SDKs

```bash
sdk list
```

or for a specific SDK:

```bash
sdk list java
```

### Install an SDK

```bash
sdk install java 21-tem
sdk install gradle 8.5
```

### Set Default SDK

```bash
sdk default java 21-tem
```

### Use Specific Version (current shell only)

```bash
sdk use java 17.0.8-tem
```

### 🔬 Check Current Version

```bash
sdk current
```

### Uninstall an SDK

```bash
sdk uninstall java 17.0.8-tem
```

---

## Per-Project SDK Version

Create a `.sdkmanrc` file in your project directory:

```bash
java=17.0.8-tem
gradle=8.5
```

### Use SDKs from `.sdkmanrc`

```bash
sdk env install  # install missing versions
sdk env          # switch to versions defined in .sdkmanrc
```

You can also auto-enable this:

```bash
sdk config
# Enable auto-env feature
```

---

## Supported SDKs (Common)

| SDK         | Command                  |
| ----------- | ------------------------ |
| Java        | `sdk install java`       |
| Kotlin      | `sdk install kotlin`     |
| Scala       | `sdk install scala`      |
| Groovy      | `sdk install groovy`     |
| Gradle      | `sdk install gradle`     |
| Maven       | `sdk install maven`      |
| Ant         | `sdk install ant`        |
| Spring Boot | `sdk install springboot` |

Check `sdk list` to explore more.

---

## SDKMAN! CLI Tips

| Task                            | Command                                   |
| ------------------------------- | ----------------------------------------- |
| Show help                       | `sdk help`                                |
| Show installed SDKs             | `sdk current`                             |
| Show available versions         | `sdk list <sdk>`                          |
| Update SDKMAN! itself           | `sdk selfupdate`                          |
| Update available SDKs           | `sdk update`                              |
| Auto-complete (enable in shell) | `source "$SDKMAN_DIR/bin/sdkman-init.sh"` |

---

## Configuration File (`~/.sdkman/etc/config`)

Common options:

* `sdkman_auto_env=true` – Auto use `.sdkmanrc`
* `sdkman_auto_selfupdate=true` – Update SDKMAN! automatically

---

## Troubleshooting

| Problem                  | Solution                                   |
| ------------------------ | ------------------------------------------ |
| SDK not available        | Run `sdk list <sdk>` to check name/version |
| `sdk: command not found` | Source SDKMAN in shell config              |
| `.sdkmanrc` ignored      | Enable auto-env: `sdk config`              |
| Conflicting SDK versions | Use `sdk use` or `.sdkmanrc` locally       |

---

## Uninstall SDKMAN!

```bash
rm -rf ~/.sdkman
```

Also remove from your `.bashrc` / `.zshrc`:

```bash
# Remove SDKMAN entries
```

---

## Best Practices

* Use `.sdkmanrc` in all your projects for reproducible environments.
* Regularly update SDKMAN! and your SDKs.
* Favor **Temurin (tem)** or **Zulu** distributions for Java.

---

