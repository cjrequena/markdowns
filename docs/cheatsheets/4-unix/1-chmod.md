---
layout: default
title: chmod
parent: unix
grand_parent: cheatsheets
permalink: /docs/unix/chmod
nav_order: 1
---

# Chmod Cheat Sheet
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

`chmod` is a command used in Unix-based systems (like Linux and macOS) to modify the access permissions of files and directories. Permissions define who can read, write, or execute a file. This guideline provides an overview of how to use `chmod` effectively.

---

### 1. Understanding Permissions

Permissions on Unix-like systems are split into three categories:
- **User (u):** The owner of the file.
- **Group (g):** Users who are part of the file's group.
- **Others (o):** All other users.

Each file or directory has three types of permissions:
- **Read (r):** View the contents of the file.
- **Write (w):** Modify or delete the contents.
- **Execute (x):** Run the file (if it's an executable or script), or access a directory's contents.

#### Permission Symbols and Numeric Codes
Permissions are represented by symbols or octal (numeric) codes:

| Permission | Symbol | Octal |
|------------|--------|-------|
| Read       | `r`    | 4     |
| Write      | `w`    | 2     |
| Execute    | `x`    | 1     |
| No Access  | `-`    | 0     |

Each permission group (user, group, others) has an octal value. For example, `rwx` translates to 7 (4 + 2 + 1).

### 2. Basic Syntax of `chmod`

The `chmod` command follows this syntax:
```bash
chmod [options] mode file
```

- **mode**: Permissions to set (in symbolic or numeric format).
- **file**: The target file(s) or directory(ies).

Example:
```bash
chmod 755 filename
```

### 3. Using Symbolic Permissions

Symbolic permissions allow you to modify permissions by adding (`+`), removing (`-`), or setting (`=`) specific permissions for user categories.

Syntax:
```bash
chmod [user class][operator][permissions] filename
```

- **User Classes**:
   - `u`: User/Owner
   - `g`: Group
   - `o`: Others
   - `a`: All (user, group, and others)

- **Operators**:
   - `+`: Adds a permission
   - `-`: Removes a permission
   - `=`: Sets a permission

Examples:
```bash
chmod u+x filename     # Add execute permission for the user
chmod g-w filename      # Remove write permission for the group
chmod o=r filename      # Set read-only permission for others
chmod a+x filename      # Add execute permission for all
chmod u=rwx,g=rx,o=r filename # Set user to rwx, group to rx, others to r
```

### 4. Using Numeric (Octal) Permissions

Numeric (or octal) permissions use a three-digit number to represent permissions for user, group, and others.

| Permission | Octal |
|------------|-------|
| ---        | 0     |
| --x        | 1     |
| -w-        | 2     |
| -wx        | 3     |
| r--        | 4     |
| r-x        | 5     |
| rw-        | 6     |
| rwx        | 7     |

The format is:
```bash
chmod XYZ filename
```
Where:
- `X` = User permission
- `Y` = Group permission
- `Z` = Others permission

Examples:
```bash
chmod 644 filename     # rw-r--r-- (User: rw, Group: r, Others: r)
chmod 755 filename     # rwxr-xr-x (User: rwx, Group: rx, Others: rx)
chmod 700 filename     # rwx------ (User: rwx, no access for group and others)
```

### 5. Recursive Permissions

Use the `-R` option to apply permissions recursively to all files and directories within a directory.

Example:
```bash
chmod -R 755 /path/to/directory
```

### 6. Examples of Common `chmod` Commands

| Command             | Description                                                |
|---------------------|------------------------------------------------------------|
| `chmod 644 file`    | Sets file to be read/write for user, read-only for others. |
| `chmod 755 file`    | Allows everyone to read/execute, only user can write.      |
| `chmod u+x file`    | Adds execute permission for the user.                      |
| `chmod g-w file`    | Removes write permission from the group.                   |
| `chmod o=r file`    | Sets read-only permission for others.                      |
| `chmod -R 700 dir`  | Gives full access to user, no access for others, recursively. |

### 7. Special Permissions

Special permissions modify the default behavior of files and directories.

#### Setuid (Set User ID)
- Represented as `4` in front of regular permissions (e.g., `4755`).
- When applied to executables, it allows users to run the executable with the owner's privileges.

Example:
```bash
chmod 4755 file    # Sets the setuid bit on a file
```

#### Setgid (Set Group ID)
- Represented as `2` in front of regular permissions (e.g., `2755`).
- For executables, it allows users to run the executable with the group’s privileges.
- For directories, new files created within inherit the directory’s group.

Example:
```bash
chmod 2755 directory    # Sets the setgid bit on a directory
```

#### Sticky Bit
- Represented as `1` in front of regular permissions (e.g., `1755`).
- Applied to directories, it restricts file deletion within the directory: only the file's owner, the directory owner, or root can delete files.

Example:
```bash
chmod 1755 directory    # Sets the sticky bit on a directory
```

### Combined Special Permissions

Special permissions are combined by summing values (setuid=4, setgid=2, sticky=1).

| Command         | Meaning                    |
|-----------------|----------------------------|
| `chmod 4755 file` | Setuid on executable     |
| `chmod 2755 dir`  | Setgid on directory      |
| `chmod 1755 dir`  | Sticky bit on directory  |
| `chmod 7755 dir`  | Setuid + Setgid          |

---

This guide should help you understand and use `chmod` effectively to manage permissions on Unix-like systems. Proper use of `chmod` enhances both security and functionality.
