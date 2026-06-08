---
layout: default
title: ssh-keys-github-gitlab
parent: unix
grand_parent: cheatsheets
nav_order: 2
---
# SSH Keys — macOS + GitHub + GitLab
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## 1. Generate SSH keys

Create separate keys for GitHub and GitLab (recommended for isolation).

```bash
# GitHub key
ssh-keygen -t ed25519 -C "cjrequena001+claudeai@gmail.com" -f ~/.ssh/id_ed25519_github

# GitLab key
ssh-keygen -t ed25519 -C "cjrequena001+claudeai@gmail.com" -f ~/.ssh/id_ed25519_gitlab
```

> Use `-t rsa -b 4096` if the remote requires RSA.

---

## 2. Add keys to the macOS keychain

```bash
ssh-add --apple-use-keychain ~/.ssh/id_ed25519_github
ssh-add --apple-use-keychain ~/.ssh/id_ed25519_gitlab
```

Verify loaded keys:

```bash
ssh-add -l
```

---

## 3. Configure ~/.ssh/config

```
# GitHub
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_github
  AddKeysToAgent yes
  UseKeychain yes

# GitLab
Host gitlab.com
  HostName gitlab.com
  User git
  IdentityFile ~/.ssh/id_ed25519_gitlab
  AddKeysToAgent yes
  UseKeychain yes
```

Set correct permissions:

```bash
chmod 600 ~/.ssh/config
```

---

## 4. Add public keys to GitHub and GitLab

Copy each public key to the clipboard:

```bash
# GitHub
pbcopy < ~/.ssh/id_ed25519_github.pub

# GitLab
pbcopy < ~/.ssh/id_ed25519_gitlab.pub
```

**GitHub:** Settings → SSH and GPG keys → New SSH key → paste → Save

**GitLab:** Preferences → SSH Keys → Add new key → paste → Save

---

## 5. Test connections

```bash
ssh -T git@github.com
# Hi <username>! You've successfully authenticated...

ssh -T git@gitlab.com
# Welcome to GitLab, @<username>!
```

---

## 6. Clone / set remote URLs

SSH URLs use the `git@` scheme — not `https://`.

```bash
# Clone
git clone git@github.com:user/repo.git
git clone git@gitlab.com:user/repo.git

# Update existing remote from HTTPS to SSH
git remote set-url origin git@github.com:user/repo.git
git remote set-url origin git@gitlab.com:user/repo.git

# Verify
git remote -v
```

---

## 7. Multiple accounts on the same host

To use two GitHub accounts, define aliases in `~/.ssh/config`:

```
Host github-personal
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_github_personal
  AddKeysToAgent yes
  UseKeychain yes

Host github-work
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_github_work
  AddKeysToAgent yes
  UseKeychain yes
```

Clone using the alias:

```bash
git clone git@github-personal:user/repo.git
git clone git@github-work:org/repo.git
```

---

## 8. Troubleshooting

| Problem | Fix |
|---|---|
| `Permission denied (publickey)` | Key not loaded — run `ssh-add --apple-use-keychain ~/.ssh/id_ed25519_github` |
| Key not persisting after reboot | Confirm `UseKeychain yes` and `AddKeysToAgent yes` are in `~/.ssh/config` |
| Multiple keys, wrong one used | Check `IdentityFile` path in config; test with `ssh -vT git@github.com` |
| `Bad owner or permissions on ~/.ssh/config` | Run `chmod 600 ~/.ssh/config` |

Verbose connection debug:

```bash
ssh -vT git@github.com
ssh -vT git@gitlab.com
```
