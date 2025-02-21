---
layout: default
title: vscode
parent: ides
grand_parent: cheatsheets
permalink: /docs/unix/vscode
nav_order: 1
---

# Visual Studio Code Cheat Sheet
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## **Install Visual Studio Code**
Download and install **VS Code** from the official website:  
👉 [https://code.visualstudio.com/](https://code.visualstudio.com/)

---

## **Install Visual Studio Code on macOS using Homebrew**

### **Ensure Homebrew is Installed**
Open **Terminal** (`Cmd + Space` → Type **Terminal** → Press **Enter**) and run:
```sh
brew --version
```
If Homebrew is not installed, install it with:
```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
Then, restart your terminal.

---

### **Install Visual Studio Code**
Run:
```sh
brew install --cask visual-studio-code
```

---

### **Verify Installation**
Check if VS Code is installed:
```sh
code --version
```
If VS Code doesn’t open, restart your terminal and try again.

---

### **(Optional) Add `code` Command to PATH**
If the `code` command doesn’t work, add it manually:
```sh
ln -s "/Applications/Visual Studio Code.app/Contents/Resources/app/bin/code" /usr/local/bin/code
```
Now, you can open VS Code from the terminal using:
```sh
code .
```

---

### **(Optional) Install Useful Extensions via Homebrew**
You can install VS Code extensions directly from Homebrew:
```sh
code --install-extension ms-python.python
code --install-extension esbenp.prettier-vscode
code --install-extension dbaeumer.vscode-eslint
```

---
