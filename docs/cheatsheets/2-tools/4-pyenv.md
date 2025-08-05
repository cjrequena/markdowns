---
layout: default
title: pyenv
parent: tools
grand_parent: cheatsheets
permalink: /docs/tools/pyenv
nav_order: 4
---
# pyenv Cheat Sheet (Mac & Linux)
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## What is pyenv?

`pyenv` is a tool that lets you easily switch between multiple versions of Python. It works by manipulating the `PATH` to point to the desired Python version.

---

## Installation

### Prerequisites

**MacOS:**

```bash
brew update
brew install openssl readline sqlite3 xz zlib
```

**Linux (Debian/Ubuntu):**

```bash
sudo apt update
sudo apt install -y make build-essential libssl-dev \
libreadline-dev zlib1g-dev libsqlite3-dev curl git
```

---

### Install pyenv

#### Using Homebrew (MacOS/Linux)

```bash
brew install pyenv
```

#### Manual (Universal)

```bash
curl https://pyenv.run | bash
```

---

### Shell Configuration

Add to your shell config (`~/.bashrc`, `~/.zshrc`, etc.):

```bash
export PYENV_ROOT="$HOME/.pyenv"
export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init --path)"
eval "$(pyenv init -)"
```

> Then restart your shell:

```bash
exec "$SHELL"
```

---

## Install Python Versions

### List available versions

```bash
pyenv install --list
```

### Install a version

```bash
pyenv install 3.11.9
```

### Uninstall a version

```bash
pyenv uninstall 3.11.9
```

---

## pyenv Usage

### Show current Python version

```bash
pyenv version
```

### List all installed versions

```bash
pyenv versions
```

### Set global Python version

```bash
pyenv global 3.11.9
```

### Set local Python version (for project dir)

```bash
pyenv local 3.10.12
```

Creates a `.python-version` file in current directory.

### Set shell-specific version

```bash
pyenv shell 3.8.18
```

---

## pyenv-virtualenv (manage virtual environments)

**Install:**

```bash
brew install pyenv-virtualenv
```

Add to shell config:

```bash
eval "$(pyenv virtualenv-init -)"
```

**Usage:**

* Create virtualenv:

  ```bash
  pyenv virtualenv 3.10.6 myenv
  ```

* Activate virtualenv:

  ```bash
  pyenv activate myenv
  ```

* Deactivate:

  ```bash
  pyenv deactivate
  ```

* Auto-activate per-project:

  ```bash
  echo "myenv" > .python-version
  ```

---

## Upgrading Python in an Existing Project
If a project needs a new Python version:

```bash
pyenv install 3.13.0
pyenv virtualenv 3.13.0 newenv
pyenv local newenv
pip install -r requirements.txt
```

---
## Troubleshooting

### pyenv not found?

Check if installed and shell configured properly.

```bash
command -v pyenv
```

Should return something like `/home/youruser/.pyenv/bin/pyenv`

### Build failed?

Install missing dependencies. Check for errors related to:

* `zlib`
* `openssl`
* `readline`

---

## Common Paths

* Pyenv root: `~/.pyenv`
* Installed Python: `~/.pyenv/versions/`
* Virtualenvs: `~/.pyenv/versions/<env-name>`

---

## Cleanup

### Remove pyenv

```bash
rm -rf ~/.pyenv
```

Also remove lines from your shell config file.

---

## Tips

* Use `pyenv local` in each project.
* Combine with `pipx` or `poetry` for isolated environments.
* For global Python packages per version: use `~/.pyenv/versions/3.x.y/lib/python3.x/site-packages/`

---
