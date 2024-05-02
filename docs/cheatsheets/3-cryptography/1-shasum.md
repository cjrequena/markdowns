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

**Calculating SHA (Secure Hash Algorithm):**

1. **Calculate SHA-256 Hash:**
   ```shell
   shasum -a 256 filename
   ```

2. **Calculate SHA-512 Hash (Verbose Output):**
   ```
   shasum -a 512 -v filename
   ```

3. **Calculate SHA-256 Hash for Multiple Files:**
   ```
   shasum -a 256 file1.txt file2.txt
   ```

4. **Calculate SHA-256 Hash in Binary Mode:**
   ```
   shasum -a 256 -b filename
   ```

5. **Calculate SHA-256 Hash in Portable Mode:**
   ```
   shasum -a 256 -p filename
   ```

**Verifying and checking SHA (Secure Hash Algorithm):**

1. **Check Single File Integrity:**
   ```shell
   shasum --check checksums.txt --ignore-missing
   ```
   ```shell
   shasum -a 256 -c checksums.txt
   ```
   ```shell
   shasum -a 256 -c checksums.txt --ignore-missing
   ```
   ```shell
   shasum -a 512 -c checksums.txt --ignore-missing
   ```
   - This commands reads the checksums from the `checksums.txt` file and verifies the integrity of the files listed.

2. **Check Multiple File Integrity:**
   ```
   shasum -a 512 -c *.sha512
   ```
   - This command reads the checksums from all `.sha512` files in the current directory and verifies the integrity of the corresponding files using SHA-512 hash algorithm.

3. **Check Integrity with Verbose Output:**
   ```
   shasum -a 256 -c -v checksums.txt
   ```
   - The `-v` option provides verbose output, displaying the filename along with the result of the integrity check for each file.

4. **Check Integrity of Binary Files:**
   ```
   shasum -a 256 -c -b checksums.bin
   ```
   - Use the `-b` option to read binary files and verify their integrity against the checksums provided in `checksums.bin`.

5. **Check Integrity of a Single File:**
   ```
   shasum -a 1 -c file1.txt.sha1
   ```
   - This command verifies the integrity of `file1.txt` using the SHA-1 hash algorithm and the checksum provided in `file1.txt.sha1`.

6. **Check Integrity and Suppress Output:**
   ```
   shasum -a 256 -s -c checksums.txt
   ```
   - The `-s` option suppresses normal output, useful for scripting. It only shows a summary of the integrity check status.

7. **Check Integrity of Files Listed in a Directory:**
   ```
   shasum -a 256 -c /path/to/directory/checksums.txt
   ```
   - This command reads the checksums from `checksums.txt` located in the specified directory and verifies the integrity of the files listed within it using SHA-256 hash algorithm.

8. **Check Integrity Using Portable Mode:**
   ```
   shasum -a 256 -c -p checksums.txt
   ```
   - The `-p` option produces a portable result, which can be useful when comparing checksums across different platforms or systems.

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


