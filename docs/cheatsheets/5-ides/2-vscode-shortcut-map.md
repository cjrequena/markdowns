---
layout: default
title: vscode-shortcut-map
parent: ides
grand_parent: cheatsheets
permalink: /docs/ides/vscode-shortcut-map
nav_order: 2
---

# Visual Studio Code Shortcut Mappping
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## 🧭 Core Philosophy

- **IntelliJ**: full-featured IDE with built-in intelligence  
- **VS Code**: lightweight editor + extensions

Tip: Install **“IntelliJ IDEA Keybindings”** extension if you want familiar shortcuts.

---

## Global Shortcut Mapping (MacOS)

| Action | IntelliJ | VS Code (Mac) |
|--------|----------|---------------|
| Search Everywhere | `Shift + Shift` | `Cmd + Shift + P` |
| Find File | `Cmd + Shift + O` | `Cmd + P` |
| Find in Files | `Cmd + Shift + F` | `Cmd + Shift + F` |
| Go to Symbol | `Cmd + Alt + O` | `Cmd + T` |
| Go to Definition | `Cmd + B` | `F12` / `Cmd + Click` |
| Quick Fix | `Option + Enter` | `Cmd + .` |
| Rename | `Shift + F6` | `F2` |
| Format Code | `Cmd + Option + L` | `Shift + Option + F` |
| Optimize Imports | `Cmd + Option + O` | `Shift + Option + O` |
| Command Palette | — | `Cmd + Shift + P` |
| Terminal | — | `` Ctrl + ` `` |

---

## Java

### Extensions
- Extension Pack for Java
- Spring Boot Extension Pack (optional)

### Shortcuts

| Action | IntelliJ | VS Code |
|--------|----------|---------|
| Generate code | `Cmd + N` | `Cmd + .` |
| Run | `Shift + F10` | `Ctrl + F5` |
| Debug | `Shift + F9` | `F5` |
| Go to implementation | `Cmd + Option + B` | `Cmd + Click` |
| Type hierarchy | `Ctrl + H` | Command Palette |

### Notes
- Refactoring is more limited than IntelliJ
- Spring support is good but less integrated

---

## Python

### Extensions
- Python (Microsoft)
- Pylance

### Shortcuts

| Action | IntelliJ (PyCharm) | VS Code |
|--------|-------------------|---------|
| Run file | `Shift + F10` | `Ctrl + F5` |
| Debug | `Shift + F9` | `F5` |
| Quick fix | `Option + Enter` | `Cmd + .` |
| Rename | `Shift + F6` | `F2` |
| Show docs | `F1` | `Cmd + K Cmd + I` |
| Go to definition | `Cmd + B` | `F12` |

### Notes
- Pylance gives strong IntelliSense
- Virtualenv setup is manual but flexible

---

## JavaScript

### Extensions
- ESLint
- Prettier

### Shortcuts

| Action | IntelliJ | VS Code |
|--------|----------|---------|
| Auto import | `Option + Enter` | `Cmd + .` |
| Rename | `Shift + F6` | `F2` |
| Extract function | `Cmd + Option + M` | `Cmd + .` |
| Format | `Cmd + Option + L` | `Shift + Option + F` |
| Find usages | `Option + F7` | `Shift + F12` |

### Notes
- VS Code is often better for JS
- Native TypeScript engine powers IntelliSense

---

## TypeScript

### Built-in support
No extension needed

### Shortcuts

| Action | IntelliJ | VS Code |
|--------|----------|---------|
| Go to definition | `Cmd + B` | `F12` |
| Peek definition | `Cmd + Shift + I` | `Option + F12` |
| Rename | `Shift + F6` | `F2` |
| Find references | `Option + F7` | `Shift + F12` |
| Organize imports | `Cmd + Option + O` | `Shift + Option + O` |

### Notes
- Best-in-class experience in VS Code
- Fast and reliable refactoring

---

## Multi-cursor & Editing

| Action | IntelliJ | VS Code |
|--------|----------|---------|
| Multi-cursor | `Option + Click` | `Option + Click` |
| Select next occurrence | `Ctrl + G` | `Cmd + D` |
| Select all occurrences | `Cmd + Ctrl + G` | `Cmd + Shift + L` |
| Duplicate line | `Cmd + D` | `Shift + Option + ↓` |
| Delete line | `Cmd + Delete` | `Cmd + Shift + K` |

---

## Recommended Extensions (All Languages)

- IntelliJ IDEA Keybindings
- Prettier
- ESLint
- GitLens

---

## Final Takeaway

| Language | Winner |
|----------|--------|
| Java | IntelliJ |
| Python | Tie |
| JavaScript | VS Code |
| TypeScript | VS Code |

---

## Pro Tip

If you're switching full-time:

1. Install IntelliJ keybindings
2. Learn `Cmd + Shift + P` (Command Palette)
3. Use `Cmd + .` for EVERYTHING (fixes, refactors, imports)

---
