---
layout: default
title: git
parent: tools
grand_parent: cheatsheets
permalink: /docs/tools/git
nav_order: 7
---

# Git Cheat Sheet
{: .no_toc }
This cheat sheet features the most important and commonly used git commands for easy reference.
Git Tips and Tricks (A collection of useful tips and tricks for Git)

## External Resources
- [Git Cheat Sheet PDF](https://education.github.com/git-cheat-sheet-education.pdf)

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## **1. Configurations**

### Setup & Configuration
```bash
git config --global user.name "Your Name"           # Set your name
git config --global user.email "you@example.com"   # Set your email
git config --list                                  # List all settings
git config --global core.editor "code --wait"      # Set default editor (e.g., VS Code)
git config --global core.autocrlf true             # Handle line endings
```

---

## **2. Creating and Managing Repositories**

### Initialize a Repository
```bash
git init                        # Create a new local repository
git clone <url>                 # Clone a remote repository
```

### Check Repository Status
```bash
git status                      # Check status of files
```

---

## **3. Working with Files**

### Adding and Removing Files
```bash
git add <file>                  # Stage a specific file
git add .                       # Stage all changes in the current directory
git rm <file>                   # Remove a file from working directory and stage it
git rm --cached <file>          # Remove file from staging but keep in working directory
```

### Viewing Changes
```bash
git diff                        # View unstaged changes
git diff --staged               # View staged changes
```

---

## **4. Committing Changes**

### Basic Commit
```bash
git commit -m "Your commit message"        # Commit staged changes
git commit --amend                        # Amend the last commit
```

### Skipping Staging Area
```bash
git commit -a -m "Your commit message"    # Add all tracked files and commit
```

---

## **5. Branching**

### Basic Branch Commands
```bash
git branch                              # List branches
git branch <branch_name>                # Create a new branch
git checkout <branch_name>              # Switch to a branch
git checkout -b <branch_name>           # Create and switch to a new branch
git branch -d <branch_name>             # Delete a branch
git branch -D <branch_name>             # Force delete a branch
```

---

## **6. Merging and Rebasing**

### Merging
```bash
git merge <branch_name>                  # Merge a branch into the current branch
```

### Resolving Merge Conflicts
```bash
# Edit the conflicting files, then:
git add <conflicted_file>
git commit
```

### Rebasing
```bash
git rebase <branch_name>                 # Rebase onto another branch
git rebase --continue                    # Continue rebase after conflict
git rebase --abort                       # Abort rebase
```

---

## **7. Stashing**

### Saving Temporary Changes
```bash
git stash                                # Stash current changes
git stash list                           # List stashes
git stash apply                          # Apply the most recent stash
git stash pop                            # Apply the stash and delete it
git stash drop                           # Delete the stash
```

---

## **8. Viewing History**

### Log Commands
```bash
git log                                  # View commit history
git log --oneline                        # Compact commit history
git log --graph --decorate               # Visualize branch history
git log -p                               # Show changes in each commit
```

---

## **9. Undoing Changes**

### Discard Changes
```bash
git restore <file>                       # Discard changes in working directory
git restore --staged <file>              # Unstage a file
```

### Resetting Commits
```bash
git reset --soft <commit_hash>           # Reset to a previous commit (keep changes staged)
git reset --mixed <commit_hash>          # Reset to a previous commit (unstage changes)
git reset --hard <commit_hash>           # Reset to a previous commit (discard changes)
```

---

## **10. Synchronizing with Remote**

### Managing Remotes
```bash
git remote add origin <url>              # Add a remote repository
git remote -v                            # List remotes
git remote remove <name>                 # Remove a remote
```

### Fetch, Pull, and Push
```bash
git fetch                                # Download changes from the remote
git pull                                 # Fetch and merge changes from remote
git push                                 # Push changes to remote repository
git push -u origin <branch_name>         # Push branch and set upstream
```

---

## **11. Tags**

### Working with Tags
```bash
git tag                                  # List all tags
git tag <tag_name>                       # Create a new tag
git tag -a <tag_name> -m "Message"       # Create an annotated tag
git push origin <tag_name>               # Push a tag to remote
git push origin --tags                   # Push all tags to remote
```

---

## **12. Collaboration**

### Viewing and Working on Changes
```bash
git blame <file>                         # Show who changed each line of a file
git show <commit_hash>                   # Show details of a commit
```

### Resolving Conflicts
```bash
# After resolving conflicts:
git add <file>
git commit
```

---

## **13. Advanced Commands**

### Clean Untracked Files
```bash
git clean -f                             # Remove untracked files
git clean -fd                            # Remove untracked files and directories
```

### Cherry Picking
```bash
git cherry-pick <commit_hash>            # Apply a specific commit to the current branch
```

### Squashing Commits
```bash
git rebase -i HEAD~<number_of_commits>   # Squash commits interactively
```

---

## **14. Hooks**

### Common Hooks
```bash
# Located in .git/hooks/
pre-commit                               # Script that runs before a commit
pre-push                                 # Script that runs before a push
```

---

## **15. Troubleshooting**

### Unfinished Operations
```bash
git merge --abort                        # Abort a merge
git rebase --abort                       # Abort a rebase
git cherry-pick --abort                  # Abort a cherry-pick
```

### Fix a Broken Push
```bash
git push --force                         # Force push changes
```

---

Here’s an **updated Git cheat sheet**, now including **rewriting history** and a deeper dive into **stashing**.

---

# **Git Cheat Sheet with Advanced Topics**

---

## **16. Rewriting History**

### Modifying the Most Recent Commit
```bash
git commit --amend                           # Amend the last commit (e.g., update message or files)
git commit --amend --no-edit                 # Amend without changing the commit message
```

### Changing the Message of an Older Commit
1. Start an interactive rebase:
   ```bash
   git rebase -i HEAD~<number_of_commits>
   ```
2. Mark the commit to edit by replacing `pick` with `edit`.
3. Modify the commit:
   ```bash
   git commit --amend
   ```
4. Continue the rebase:
   ```bash
   git rebase --continue
   ```

### Squashing Commits (Combining Commits)
1. Start an interactive rebase:
   ```bash
   git rebase -i HEAD~<number_of_commits>
   ```
2. Change `pick` to `squash` (or `s`) for the commits you want to merge.
3. Edit the commit message when prompted.

### Removing a Commit from History
```bash
git rebase -i HEAD~<number_of_commits>       # Mark the commit with `drop` to remove it
```

### Reverting Changes in a Commit
```bash
git revert <commit_hash>                     # Create a new commit that undoes a specific commit
```

### Rewriting History Across the Entire Repository
Use with caution!
```bash
git filter-branch --tree-filter 'rm -rf <file_or_directory>' HEAD
```

### Force Push to Rewrite Remote History
```bash
git push --force                             # Push rewritten history to remote
```

---

## **17. Advanced Stashing**

### Save Changes to Stash
```bash
git stash                                    # Save changes to stash
git stash save "Message"                     # Save with a message
```

### List Stashes
```bash
git stash list                               # Show all stashes
```

### Apply or Pop Stashes
```bash
git stash apply                              # Apply the latest stash (without removing it)
git stash apply stash@{2}                    # Apply a specific stash
git stash pop                                # Apply and remove the latest stash
git stash pop stash@{2}                      # Apply and remove a specific stash
```

### Stashing Specific Files
```bash
git stash push -m "Message" <file>           # Stash specific files
```

### View Stash Contents
```bash
git stash show -p                            # Show changes in the most recent stash
git stash show -p stash@{2}                  # Show changes in a specific stash
```

### Create a Branch from Stash
```bash
git stash branch <branch_name>               # Create a new branch with the stash applied
```

### Drop or Clear Stashes
```bash
git stash drop                               # Remove the latest stash
git stash drop stash@{2}                     # Remove a specific stash
git stash clear                              # Remove all stashes
```

---

## **Key Differences: Apply vs. Pop**
- **`git stash apply`:** Applies the stash but keeps it in the stash list.
- **`git stash pop`:** Applies the stash and removes it from the stash list.

---

## **18. Additional Tips for History Rewriting**

### Remove Sensitive Data from History
If a file with sensitive data (e.g., `passwords.txt`) was committed:
```bash
git filter-branch --force --index-filter \
  'git rm --cached --ignore-unmatch passwords.txt' \
  --prune-empty --tag-name-filter cat -- --all
```

Then force-push to update the remote:
```bash
git push --force --all
```

---

These advanced topics are vital for working with complex Git repositories and maintaining a clean history. If you'd like further explanations or examples for any section, let me know!

This guide should provide you with everything you need for daily Git operations. If you need additional details on any command, use:
```bash
git help <command>
```

