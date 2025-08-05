---
layout: default
title: pip
parent: tools
grand_parent: cheatsheets
permalink: /docs/tools/pip
nav_order: 4
---
# pip Cheat Sheet
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
   {:toc}

---

## 🔹 What is pip?

**`pip`** is the **Python package installer**, used to install and manage packages from [PyPI](https://pypi.org) and other indexes.

---

## 🔹 pip Command Structure

```bash
pip [global-options] <command> [command-options] [arguments]
```

### Example:

```bash
pip install --upgrade numpy
```

---

## 🔹 Core pip Commands Overview

| Command     | Purpose                                      |
| ----------- | -------------------------------------------- |
| `install`   | Install packages                             |
| `uninstall` | Uninstall packages                           |
| `list`      | List installed packages                      |
| `show`      | Show info about a package                    |
| `freeze`    | Output packages in `requirements.txt` format |
| `check`     | Check for broken dependencies                |
| `download`  | Download packages without installing         |
| `wheel`     | Build wheels from source                     |
| `search`    | 🔴 Deprecated                                |
| `config`    | Manage pip config settings                   |
| `cache`     | Inspect pip’s cache                          |
| `debug`     | Show diagnostic info                         |
| `help`      | Help with pip or a command                   |

---

## 🔹 pip Install Syntax

### Basic Install

```bash
pip install <package-name>
```

### Version-Specific

```bash
pip install <package-name>==1.0.4
pip install <package-name>~=1.4.2
pip install '<package-name>>=1.2,<2.0'
```

### Editable Mode (for local development)

```bash
pip install -e .
```

### Install from URLs or VCS

```bash
pip install git+https://github.com/user/repo.git
pip install https://example.com/packages/package.whl
```

---

## 🔹 pip Requirements Files

### Create

```bash
pip freeze > requirements.txt
```

### Use

```bash
pip install -r requirements.txt
```

### Best Practices

* Pin versions for consistency:

  ```
  numpy==1.25.0
  pandas==2.2.0
  ```

---

## 🔹 pip Configuration Management

### Configuration Locations

| Scope      | Location                                                            |
| ---------- | ------------------------------------------------------------------- |
| Global     | `/etc/pip/pip.conf` (Unix) or `%PROGRAMDATA%\pip\pip.ini` (Windows) |
| User       | `~/.pip/pip.conf` (Unix) or `%APPDATA%\pip\pip.ini` (Windows)       |
| Virtualenv | `<venv>/pip.conf`                                                   |

### Commands

```bash
pip config list
pip config get global.index-url
pip config set global.index-url https://pypi.org/simple
pip config unset global.index-url
```

---

## 🔹 pip Caching and Performance

* pip uses a cache directory (typically `~/.cache/pip`) to avoid re-downloading packages.
* Use `--no-cache-dir` to avoid using the cache.

```bash
pip install <package> --no-cache-dir
```

---

## 🔹 pip Environment & Permissions

| Option     | Description                   |
| ---------- | ----------------------------- |
| `--user`   | Install to user site-packages |
| `--root`   | Install under custom root     |
| `--prefix` | Specify installation prefix   |
| `--target` | Install to custom directory   |

Example:

```bash
pip install --user requests
```

---

## 🔹 Common Errors & Fixes

| Issue                            | Fix                                                   |
| -------------------------------- | ----------------------------------------------------- |
| `Permission denied`              | Use `--user` or `sudo`                                |
| `SSL: CERTIFICATE_VERIFY_FAILED` | Use `--trusted-host`                                  |
| `No module named pip`            | Reinstall via `get-pip.py` or ensure pip is in `PATH` |

---

## 🔹 Best Practices

✅ Always use a **virtual environment** (e.g., `venv`, `virtualenv`)

✅ Pin versions in `requirements.txt`

✅ Use `pip install --upgrade pip` regularly

✅ Use `pip list --outdated` to monitor updates

✅ Avoid using `pip` and `conda` together in the same environment (unless you know what you're doing)

---

## 🔹 Useful Commands at a Glance

```bash
# Create virtual environment
python -m venv env
source env/bin/activate  # or .\env\Scripts\activate (Windows)

# Install a package
pip install requests

# Save dependencies
pip freeze > requirements.txt

# Install from file
pip install -r requirements.txt

# Check for outdated packages
pip list --outdated

# Upgrade pip itself
python -m pip install --upgrade pip
```

---

## 📌 **Basic pip Commands**

| Command                | Description                                              |
| ---------------------- | -------------------------------------------------------- |
| `pip --version`        | Show pip version                                         |
| `pip help`             | Display help for pip                                     |
| `pip <command> --help` | Help for a specific command (e.g., `pip install --help`) |

---

## 📦 **Installing Packages**

| Command                             | Description                                         |
| ----------------------------------- | --------------------------------------------------- |
| `pip install <package>`             | Install latest version of a package                 |
| `pip install <package>==1.2.3`      | Install specific version                            |
| `pip install '<package>>=1.2,<2.0'` | Install version range                               |
| `pip install -r requirements.txt`   | Install all from a requirements file                |
| `pip install .`                     | Install a local package (from current directory)    |
| `pip install -e .`                  | Install in "editable" mode (useful for development) |
| `pip install --upgrade <package>`   | Upgrade to latest version                           |
| `pip install --pre <package>`       | Include pre-release versions                        |

---

## ❌ **Uninstalling Packages**

| Command                             | Description           |
| ----------------------------------- | --------------------- |
| `pip uninstall <package>`           | Uninstall a package   |
| `pip uninstall -r requirements.txt` | Uninstall all in file |

---

## 🔍 **Searching & Showing Packages**

| Command                | Description                                          |
| ---------------------- | ---------------------------------------------------- |
| `pip search <package>` | 🔴 Deprecated in recent versions                     |
| `pip show <package>`   | Show details (version, dependencies, location, etc.) |
| `pip list`             | List all installed packages                          |
| `pip list --outdated`  | List outdated packages                               |
| `pip list --uptodate`  | List up-to-date packages                             |
| `pip check`            | Check for broken dependencies                        |

---

## 📁 **Requirements Files**

| Command                             | Description                                            |
| ----------------------------------- | ------------------------------------------------------ |
| `pip freeze`                        | Output installed packages in `requirements.txt` format |
| `pip freeze > requirements.txt`     | Save all installed packages to file                    |
| `pip install -r requirements.txt`   | Install all packages from file                         |
| `pip uninstall -r requirements.txt` | Uninstall all packages in file                         |

---

## 🌍 **Using Indexes and Sources**

| Command                                                      | Description                                              |
| ------------------------------------------------------------ | -------------------------------------------------------- |
| `pip install <package> -i <index_url>`                       | Use a custom package index                               |
| `pip install <package> --extra-index-url <url>`              | Add an extra index (e.g. private PyPI)                   |
| `pip install <package> --no-index --find-links <dir_or_url>` | Install from a local directory or URL without using PyPI |

---

## 🧪 **Installing from Other Sources**

| Command                                            | Description                                |
| -------------------------------------------------- | ------------------------------------------ |
| `pip install git+https://github.com/user/repo.git` | Install from a GitHub repo                 |
| `pip install https://example.com/pkg.whl`          | Install from a wheel file URL              |
| `pip install ./package.whl`                        | Install from a local wheel                 |
| `pip install <package>.tar.gz`                     | Install from a tarball source distribution |

---

## ⚙️ **Configuration & Environment**

| Command                        | Description                        |
| ------------------------------ | ---------------------------------- |
| `pip config list`              | Show current config                |
| `pip config get <key>`         | Get a config value                 |
| `pip config set <key> <value>` | Set config (e.g. proxy, index-url) |
| `pip config unset <key>`       | Remove a config setting            |

📝 Config scopes:

* `--global`: System-wide
* `--user`: Per-user
* `--site`: Per-virtualenv

---

## 🔒 **Proxy and Trusted Hosts**

| Command                                                     | Description                        |
| ----------------------------------------------------------- | ---------------------------------- |
| `pip install <package> --proxy http://user:pass@proxy:port` | Install using a proxy              |
| `pip install <package> --trusted-host <hostname>`           | Bypass SSL verification for a host |

---

## 🛠️ **Common Flags**

| Flag                       | Description                                      |
| -------------------------- | ------------------------------------------------ |
| `--quiet` or `-q`          | Reduce output verbosity                          |
| `--verbose` or `-v`        | Increase output verbosity                        |
| `--no-cache-dir`           | Don’t use cache                                  |
| `--user`                   | Install to user site directory (not system-wide) |
| `--upgrade-strategy eager` | Upgrade all dependencies eagerly                 |

---

## 🔄 **Upgrading pip**

```bash
python -m pip install --upgrade pip
```

---

## 📦 **Wheel Support**

| Command                     | Description                |
| --------------------------- | -------------------------- |
| `pip wheel <package>`       | Build a wheel              |
| `pip install <package>.whl` | Install a wheel file       |
| `pip install wheel`         | Install wheel tool support |

---
