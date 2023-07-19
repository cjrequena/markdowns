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
   ```shell
   shasum myfile.txt
   ```
2. Calculate the SHA-512 checksum of multiple files:
   ```shell
   shasum -a 512 file1.txt file2.txt
   ```
3. Verify checksums stored in a file:
   ```shell
   shasum -c checksums.txt
   ```
4. Calculate the SHA-256 checksum of a file and display only the checksum value:
   ```shell
   shasum -a 256 -p myfile.txt
   ```
5. Verify checksums and display the status:
   ```shell
   shasum -s -c checksums.txt
   ```
6. If you have a specific checksum value (e.g., a1b2c3d4e5f6) and want to verify a single file, use the following command:
   ```shell
   shasum -c <<< "a1b2c3d4e5f6  filename.txt"
   ```
7. Check the hash of a file
   ```shell
   $ echo "{hash}  {filename}" | shasum -a 256 -c -
   $ echo | shasum -a 256 {filename} | shasum -a 256 -c -
   $ shasum -c <<< "{hash}  {filename}"
   $ shasum -c <<< "a4bffc71f406e7c381b3a9692e980d233c244ce26b3f7ea7170c447f8fe3f1e5  BlockstreamGreen_MacOS_x86_64.zip"
   $ shasum -c SHA256SUMS.asc
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


