---
layout: default
title: shasum
parent: cryptography
grand_parent: cheatsheets
permalink: /docs/cryptography/shasum
nav_order: 1
---

# Shasum Cheat Sheet
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---
## Command Cheat Sheet

The `shasum` command is used to calculate and verify SHA (Secure Hash Algorithm) checksums of files. It computes the hash values of files using different SHA algorithms such as SHA-1, SHA-224, SHA-256, SHA-384, or SHA-512.

### Basic Usage:

```
shasum [OPTIONS] [FILES]
```

### Options:

- `-a`, `--algorithm <algorithm>`: Specifies the SHA algorithm to use. Supported options are `1`, `224`, `256`, `384`, and `512`. The default is SHA-256.
- `-c`, `--check`: Reads checksums from a file and verifies them against the given files.
- `-b`, `--binary`: Reads files in binary mode (useful for non-text files).
- `-p`, `--portable`: Produces output in a portable format that can be used across different platforms.
- `-s`, `--status`: Suppresses normal output and only displays the status of checksum verification.
- `-t`, `--text`: Reads files in text mode (default behavior).
- `-v`, `--verbose`: Enables verbose output, showing filenames and status for each file processed.
- `--version`: Displays the version information for `shasum`.
- `--help`: Displays the help message for `shasum`.

### Examples:

1. Calculate the SHA-256 checksum of a file:
   ```
   shasum myfile.txt
   ```

2. Calculate the SHA-512 checksum of multiple files:
   ```
   shasum -a 512 file1.txt file2.txt
   ```

3. Verify checksums stored in a file:
   ```
   shasum -c checksums.txt
   ```

4. Calculate the SHA-256 checksum of a file and display only the checksum value:
   ```
   shasum -a 256 -p myfile.txt
   ```

5. Verify checksums and display the status:
   ```
   shasum -s -c checksums.txt
   ```

### Output Format:

The default output format of `shasum` is:

```
<checksum>  <filename>
```

- `<checksum>`: The computed hash value of the file.
- `<filename>`: The name of the file.

If the `-p` (portable) option is used, the output format changes to:

```
<checksum> *<filename>
```

- `<checksum>`: The computed hash value of the file.
- `<filename>`: The name of the file, preceded by an asterisk.

### Security Considerations:

- Checksums are used to verify the integrity of files and detect any changes. They do not provide encryption or protection against malicious modifications.
- Always obtain checksum values from trusted sources to ensure their authenticity.
- Use a secure channel to transfer or share checksum values to prevent tampering.
- Verify checksums using a trusted tool and compare against multiple sources if possible.

Remember to consult the `shasum` command's documentation or `man` page for further information or specific details related to your system.

## How to check the hash of a file.
```sh
$ echo "{hash}  {filename}" | shasum -a 256 -c -
```
**or**

```sh
$ echo | shasum -a 256 {filename} | shasum -a 256 -c -
```
---


