---
layout: default
title: jenv
parent: tools
grand_parent: cheatsheets
permalink: /docs/tools/jenv
nav_order: 6
---
# jenv cheatsheet
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## What is jenv?

`jenv` manages multiple installed JDKs by putting lightweight **shims** (`java`, `javac`, `mvn`, ...) at the front of `PATH`. It picks the JDK per shell, per directory or globally.

* jenv does **not** download or install JDKs. Install them with Homebrew, SDKMAN!, the OS package manager, etc., then register them with `jenv add`.
* Version resolution order: `JENV_VERSION` (shell) > `.java-version` (local, searched upward) > `~/.jenv/version` (global).

---

## Installation

### macOS (Homebrew)

```bash
brew install jenv
```

### Linux / manual (git)

```bash
git clone https://github.com/jenv/jenv.git ~/.jenv
```

---

## Shell Configuration

### zsh (`~/.zshrc`)

```bash
export PATH="$HOME/.jenv/bin:$PATH"   # manual install only; Homebrew already puts jenv on PATH
eval "$(jenv init -)"
```

### bash (`~/.bash_profile` or `~/.bashrc`)

```bash
export PATH="$HOME/.jenv/bin:$PATH"
eval "$(jenv init -)"
```

Reload and verify:

```bash
source ~/.zshrc
jenv doctor
```

---

## Add JDKs

### List the JDKs the OS knows about (macOS)

```bash
/usr/libexec/java_home -V
```

### Register a JDK

```bash
jenv add /Library/Java/JavaVirtualMachines/temurin-21.jdk/Contents/Home
jenv add $(/usr/libexec/java_home -v 17)
```

Homebrew JDK locations:

```bash
jenv add /opt/homebrew/opt/openjdk@17      # Apple Silicon
jenv add /usr/local/opt/openjdk@17         # Intel
```

### List registered versions

```bash
jenv versions
```

### Remove a version from jenv (does not delete the JDK)

```bash
jenv remove 17.0.10
```

---

## Select a Version

### Global (default for all shells)

```bash
jenv global 21
```

### Local (per directory, writes `.java-version`)

```bash
jenv local 17
```

### Shell (current session only)

```bash
jenv shell 11
jenv shell --unset
```

### Inspect

```bash
jenv version          # active version and where it was set
jenv which java       # real binary behind the shim
jenv prefix           # JAVA_HOME of the active version
jenv prefix 17        # JAVA_HOME of a specific version
```

---

## Plugins

### Set `JAVA_HOME` automatically (recommended)

```bash
jenv enable-plugin export
exec $SHELL -l
echo $JAVA_HOME
```

### Make build tools use the jenv JDK

```bash
jenv enable-plugin maven
jenv enable-plugin gradle
```

Others: `ant`, `sbt`, `scala`, `groovy`, `kotlin`, `springboot`, `lein`, `grails`, `asciidoctor`, `vertx`, `scalac`.

### Manage plugins

```bash
jenv plugins                    # list available/enabled
jenv disable-plugin maven
jenv rehash                     # rebuild shims after changes
```

---

## Command Reference

| Command | Description |
|---|---|
| `jenv add <path>` | Register a JDK |
| `jenv remove <version>` | Unregister a JDK |
| `jenv versions` | List registered JDKs |
| `jenv version` | Show active version |
| `jenv global <v>` | Set global version |
| `jenv local <v>` | Set per-directory version (`.java-version`) |
| `jenv local --unset` | Remove local version |
| `jenv shell <v>` | Set version for current shell |
| `jenv which <cmd>` | Path of the real command |
| `jenv prefix [v]` | JAVA_HOME of a version |
| `jenv exec <cmd>` | Run a command with jenv's environment |
| `jenv rehash` | Rebuild shims |
| `jenv enable-plugin <p>` / `disable-plugin <p>` | Toggle plugin |
| `jenv plugins` | List plugins |
| `jenv doctor` | Diagnose setup |
| `jenv options` / `jenv commands` | Show config / commands |

---

## Using jenv with SDKMAN!

Division of labor:

| Tool | Role |
|---|---|
| SDKMAN! | **Installs** JDKs (and Maven, Gradle, ...) |
| jenv | **Selects** which JDK is active, per project |

### 1. Install JDKs with SDKMAN!

```bash
sdk list java
sdk install java 21.0.5-tem
sdk install java 17.0.13-tem
```

They live in `~/.sdkman/candidates/java/<identifier>`.

### 2. Register them in jenv

```bash
jenv add ~/.sdkman/candidates/java/21.0.5-tem
jenv add ~/.sdkman/candidates/java/17.0.13-tem
jenv versions
```

> Use the real directories, not the `current` symlink; `current` changes when you run `sdk default`.

### 3. Shell init order (`~/.zshrc`)

SDKMAN! must be last, jenv before it:

```bash
export PATH="$HOME/.jenv/bin:$PATH"
eval "$(jenv init -)"

#THIS MUST BE AT THE END OF THE FILE FOR SDKMAN TO WORK!!!
export SDKMAN_DIR="$HOME/.sdkman"
[[ -s "$HOME/.sdkman/bin/sdkman-init.sh" ]] && source "$HOME/.sdkman/bin/sdkman-init.sh"
```

### 4. Fix PATH shadowing

SDKMAN! prepends `~/.sdkman/candidates/java/current/bin` to `PATH`, which comes **before** the jenv shims, so `java` ignores jenv. Re-assert the shims after SDKMAN! init, at the very end of `~/.zshrc`:

```bash
export PATH="$HOME/.jenv/shims:$PATH"
```

Check:

```bash
which java            # expect ~/.jenv/shims/java
jenv which java       # real JDK
```

### 5. Keep `JAVA_HOME` in sync with jenv

```bash
jenv enable-plugin export
jenv enable-plugin maven
jenv enable-plugin gradle
```

SDKMAN!-installed Maven/Gradle then use the jenv-selected JDK through `JAVA_HOME`.

### 6. Per project

```bash
cd my-project
jenv local 17          # writes .java-version
java -version
```

Use `.java-version` (jenv) **or** `.sdkmanrc` (SDKMAN!) for Java in a project, not both, to avoid conflicting sources of truth. `.sdkmanrc` can still pin non-Java tools (Maven, Gradle, Kotlin).

### 7. Sync script: register every SDKMAN! JDK

```bash
for jdk in ~/.sdkman/candidates/java/*/; do
  [ -L "${jdk%/}" ] && continue          # skip the `current` symlink
  jenv add "$jdk"
done
```

### 8. After `sdk uninstall java <id>`

```bash
jenv remove <version-shown-in-jenv-versions>
jenv rehash
```

### When to use which

| Goal | Use |
|---|---|
| Download / update / remove a JDK | `sdk install` / `sdk upgrade` / `sdk uninstall` |
| Switch JDK per project automatically | `jenv local` |
| Switch JDK in this terminal only | `jenv shell` |
| Default JDK for new shells | `jenv global` (not `sdk default`) |
| Install Maven, Gradle, Kotlin, Spring Boot CLI | SDKMAN! |

---

## Troubleshooting

### `jenv: command not found`

Ensure `~/.jenv/bin` is on `PATH` (manual install) or `brew install jenv` succeeded, then reload the shell.

### `java` ignores jenv

```bash
jenv doctor
which -a java
```

Shims must come first in `PATH`. With SDKMAN!, apply step 4 above.

### `JAVA_HOME` empty or wrong

```bash
jenv enable-plugin export
exec $SHELL -l
```

### `version 'X' is not installed`

```bash
jenv versions         # use the exact name listed
jenv add <jdk-path>
```

### Shims missing after installing a tool

```bash
jenv rehash
```

### IDE (IntelliJ, VS Code) ignores jenv

IDEs don't use shims. Point the project SDK to the real path:

```bash
jenv prefix 17
```

---

## Uninstall

```bash
brew uninstall jenv            # Homebrew
rm -rf ~/.jenv                 # manual / leftover data
```

Then remove the `jenv` lines from `~/.zshrc` / `~/.bashrc`.

---

## Best Practices

* Commit `.java-version` to the repository so the team shares the JDK.
* Set a LTS JDK as `jenv global`.
* Enable the `export` plugin; avoid hard-coding `JAVA_HOME`.
* With SDKMAN!: install there, select with jenv, keep jenv shims first in `PATH`.

---

## References

- [jenv on GitHub](https://github.com/jenv/jenv)
- [SDKMAN! usage](https://sdkman.io/usage)
