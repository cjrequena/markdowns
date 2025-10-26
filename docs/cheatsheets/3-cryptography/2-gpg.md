---
layout: default
title: gpg
parent: cryptography
grand_parent: cheatsheets
permalink: /docs/cryptography/gpg
nav_order: 2
---

# GPG (GNU Privacy Guard) Cheat Sheet
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---
## Introduction

GPG (GNU Privacy Guard) is a powerful open-source implementation of the OpenPGP standard. It enables users to encrypt and sign data and communications, ensuring privacy and authenticity. This guide covers key concepts, installation, usage, key management, and best practices.

---

## Key Concepts

### Public-Key Cryptography

GPG uses asymmetric cryptography:

* **Public Key**: Used to encrypt data or verify signatures.
* **Private Key**: Used to decrypt data or create signatures.

### Key Types

* **Primary Key**: Your main identity. Often used for signing and certification.
* **Subkeys**: Created for specific uses (e.g., encryption, authentication).

### User ID (UID)

Each key is associated with one or more UIDs, typically including your name and email address.

---

## Installation

### Linux (Debian/Ubuntu)

```bash
sudo apt install gnupg
```

### macOS

```bash
brew install gnupg
```

Use [GPG Suite](https://gpgtools.org/), which includes GPG Keychain, GPG Mail, and more.

```bash
brew install --cask gpg-suite
```

### Windows

* Use [Gpg4win](https://gpg4win.org/), which includes Kleopatra, a key management tool.

---

## Generating Keys

```bash
gpg --full-generate-key
```

View your keys:

```bash
gpg --list-keys
```

---

## Exporting and Importing Keys

### Export Public Key

```bash
gpg --armor --export your@email.com > publickey.asc
```

### Export Private Key

```bash
gpg --armor --export-secret-keys your@email.com > privatekey.asc
```

### Import a Key

```bash
gpg --import publickey.asc
```

---

## Encrypting and Decrypting Files

### Encrypt to a Recipient

```bash
gpg --encrypt --recipient recipient@email.com file.txt
```

### Encrypt with Password (Symmetric)

```bash
gpg --symmetric file.txt
```

### Decrypt

```bash
gpg --output file.txt --decrypt file.txt.gpg
```

---

## Signing and Verifying

### Sign a File

```bash
gpg --sign file.txt
```

### Create a Detached Signature

```bash
gpg --armor --output file.sig --detach-sig file.txt
```

### Verify a Signature

```bash
gpg --verify file.sig file.txt
```

---

## Key Management

### List All Keys

```bash
gpg --list-keys
```

### Show Fingerprint

```bash
gpg --fingerprint user@email.com
```

### Edit a Key

```bash
gpg --edit-key user@email.com
```

Useful commands within key edit:

* `addkey` - Add a new subkey
* `revkey` - Revoke a key
* `trust` - Set trust level
* `quit` - Exit editor

---

## Best Practices

* Use strong passphrases for your private key.
* Keep your private key offline for sensitive uses.
* Regularly update and revoke unused subkeys.
* Sign and verify keys from trusted sources.
* Backup your keys securely (e.g., encrypted USB drive).

---

## Useful Commands

### View Packets in an Encrypted File

```bash
gpg --list-packets file.gpg
```

### Encrypt Verbosely (Show Recipient Info)

```bash
gpg -vv --encrypt --recipient user@email.com file.txt
```

### Show Recipient Fingerprint Before Encrypting

```bash
gpg --fingerprint user@email.com
```

---

## Tools and Interfaces

* **GPG Keychain (macOS)**: GUI for key management
* **Kleopatra (Windows)**: GUI for managing keys and encrypting files
* **Enigmail** (Legacy Thunderbird plugin): Email encryption
* **Mailvelope**: Browser extension for OpenPGP email encryption


To change the **GPG passphrase cache timeout**, you need to configure `gpg-agent`, which handles caching for both asymmetric (private key) and symmetric (password-based) decryption.

---

## gpg-agent

### Edit `gpg-agent.conf`

Create or edit the GPG agent configuration file:

```bash
nano ~/.gnupg/gpg-agent.conf
```

Then add or modify the following lines:

```ini
default-cache-ttl 600
max-cache-ttl 7200
```

* `default-cache-ttl`: Time (in seconds) the passphrase is cached after first use (e.g. 600 seconds = 10 minutes).
* `max-cache-ttl`: Maximum time a cached passphrase is kept alive (even if used repeatedly).

You can set them to something like:

```ini
default-cache-ttl 60       # 1 minute
max-cache-ttl 300          # 5 minutes
```

---

### Reload gpg-agent

After saving the file, run:

```bash
gpgconf --kill gpg-agent
```

This will stop the agent, and it will restart automatically with the new settings the next time it's needed.

---

## gpgfy.sh

This script, `gpgfy.sh`, is a **Bash utility to securely encrypt and decrypt files using GPG (GNU Privacy Guard)**. It combines **symmetric** and **asymmetric** encryption methods for added security.


```shell
#!/bin/bash

# Filename: gpgfy.sh
# Description: Encrypts a file symmetrically, then asymmetrically with enhanced security and error handling.
#              Can also decrypt the result by reversing the steps.
# Version: 2.0

# Usage:
# Encrypt: ./gpgfy.sh encrypt input.txt recipient@example.com
# Decrypt: ./gpgfy.sh decrypt input.txt.asymmetric.gpg

set -euo pipefail  # Exit on error, undefined vars, pipe failures

# Configuration
readonly CIPHER_ALGO="AES256"
readonly DIGEST_ALGO="SHA512"
readonly COMPRESS_ALGO="2"  # ZLIB compression
readonly S2K_MODE="3"       # Iterated and salted S2K
readonly S2K_COUNT="65011712"  # High iteration count for key derivation

# Color codes for output
readonly RED='\033[0;31m'
readonly GREEN='\033[0;32m'
readonly YELLOW='\033[1;33m'
readonly BLUE='\033[0;34m'
readonly NC='\033[0m' # No Color

# Logging functions
log_info() {
    echo -e "${BLUE}ℹ️  $1${NC}" >&2
}

log_success() {
    echo -e "${GREEN}✅ $1${NC}" >&2
}

log_warning() {
    echo -e "${YELLOW}⚠️  $1${NC}" >&2
}

log_error() {
    echo -e "${RED}❌ $1${NC}" >&2
}

# Cleanup function for temporary files
cleanup() {
    local temp_files=("$@")
    for file in "${temp_files[@]}"; do
        if [[ -f "$file" ]]; then
            log_info "Cleaning up temporary file: $file"
            shred -vfz -n 3 "$file" 2>/dev/null || rm -f "$file"
        fi
    done
}

# Validate GPG installation and configuration
validate_gpg() {
    if ! command -v gpg >/dev/null 2>&1; then
        log_error "GPG is not installed or not in PATH"
        exit 1
    fi

    local gpg_version
    gpg_version=$(gpg --version | head -n1 | cut -d' ' -f3)
    log_info "Using GPG version: $gpg_version"
}

# Check if recipient key exists
check_recipient_key() {
    local recipient="$1"

    log_info "Checking for recipient key: $recipient"
    if ! gpg --list-keys "$recipient" >/dev/null 2>&1; then
        log_error "No public key found for recipient: $recipient"
        log_info "Import the recipient's public key first:"
        log_info "  gpg --import recipient_pubkey.asc"
        exit 1
    fi

    # Check key validity
    local key_validity
    key_validity=$(gpg --list-keys --with-colons "$recipient" 2>/dev/null | awk -F: '/^pub:/ {print $2}')
    if [[ "$key_validity" == "r" ]]; then
        log_warning "Recipient key is revoked: $recipient"
    elif [[ "$key_validity" == "e" ]]; then
        log_warning "Recipient key is expired: $recipient"
    fi
}

# Validate file paths and permissions
validate_file() {
    local file="$1"
    local operation="$2"

    if [[ ! -f "$file" ]]; then
        log_error "File not found: $file"
        exit 1
    fi

    if [[ "$operation" == "read" && ! -r "$file" ]]; then
        log_error "Cannot read file: $file"
        exit 1
    fi

    if [[ "$operation" == "write" ]]; then
        local dir
        dir=$(dirname "$file")
        if [[ ! -w "$dir" ]]; then
            log_error "Cannot write to directory: $dir"
            exit 1
        fi
    fi
}

# Generate secure temporary filename
generate_temp_file() {
    local base="$1"
    local suffix="$2"
    echo "${base}.$(date +%s).${RANDOM}.${suffix}"
}

encrypt_file() {
    local input="$1"
    local recipient="$2"

    # Validation
    validate_file "$input" "read"
    check_recipient_key "$recipient"

    # Generate output filenames
    local sym_file final_file
    sym_file=$(generate_temp_file "$input" "symmetric.gpg")
    final_file="${input}.asymmetric.gpg"

    # Check if output file already exists
    if [[ -f "$final_file" ]]; then
        log_warning "Output file already exists: $final_file"
        read -p "Overwrite? (y/N): " -n 1 -r
        echo
        if [[ ! $REPLY =~ ^[Yy]$ ]]; then
            log_info "Operation cancelled"
            exit 0
        fi
    fi

    # Setup cleanup trap
    trap "cleanup '$sym_file'" EXIT INT TERM

    log_info "[1/2] Symmetric encryption of '$input' with $CIPHER_ALGO..."

    # Symmetric encryption with enhanced security parameters
    if ! gpg \
        --batch \
        --yes \
        --quiet \
        --symmetric \
        --cipher-algo "$CIPHER_ALGO" \
        --digest-algo "$DIGEST_ALGO" \
        --compress-algo "$COMPRESS_ALGO" \
        --s2k-mode "$S2K_MODE" \
        --s2k-count "$S2K_COUNT" \
        --output "$sym_file" \
        "$input"; then
        log_error "Symmetric encryption failed"
        exit 1
    fi

    log_info "[2/2] Asymmetric encryption for recipient: $recipient..."

    # Asymmetric encryption with trust model override
    if ! gpg \
        --batch \
        --yes \
        --quiet \
        --trust-model always \
        --encrypt \
        --recipient "$recipient" \
        --cipher-algo "$CIPHER_ALGO" \
        --digest-algo "$DIGEST_ALGO" \
        --compress-algo "$COMPRESS_ALGO" \
        --output "$final_file" \
        "$sym_file"; then
        log_error "Asymmetric encryption failed"
        exit 1
    fi

    # Verify the encrypted file was created and has content
    if [[ ! -s "$final_file" ]]; then
        log_error "Encryption failed: output file is empty or missing"
        exit 1
    fi

    log_success "File encrypted successfully: $final_file"
    log_info "Original file size: $(wc -c < "$input") bytes"
    log_info "Encrypted file size: $(wc -c < "$final_file") bytes"
}

decrypt_file() {
    local encrypted_file="$1"

    # Validation
    validate_file "$encrypted_file" "read"

    # Derive filenames
    if [[ ! "$encrypted_file" =~ \.asymmetric\.gpg$ ]]; then
        log_error "File doesn't appear to be encrypted with this script: $encrypted_file"
        log_info "Expected filename pattern: *.asymmetric.gpg"
        exit 1
    fi

    local base_name intermediate_file output_file
    base_name="${encrypted_file%.asymmetric.gpg}"
    intermediate_file=$(generate_temp_file "$base_name" "symmetric.gpg")
    output_file="${base_name}.decrypted"

    # Check if output file already exists
    if [[ -f "$output_file" ]]; then
        log_warning "Output file already exists: $output_file"
        read -p "Overwrite? (y/N): " -n 1 -r
        echo
        if [[ ! $REPLY =~ ^[Yy]$ ]]; then
            log_info "Operation cancelled"
            exit 0
        fi
    fi

    # Setup cleanup trap
    trap "cleanup '$intermediate_file'" EXIT INT TERM

    log_info "[1/2] Asymmetric decryption of '$encrypted_file'..."

    # Asymmetric decryption
    if ! gpg \
        --batch \
        --yes \
        --quiet \
        --output "$intermediate_file" \
        --decrypt "$encrypted_file"; then
        log_error "Asymmetric decryption failed"
        log_info "Possible reasons:"
        log_info "  - Wrong private key"
        log_info "  - Private key not available"
        log_info "  - File is corrupted"
        exit 1
    fi

    log_info "[2/2] Symmetric decryption of intermediate file..."

    # Symmetric decryption
    if ! gpg \
        --batch \
        --yes \
        --quiet \
        --output "$output_file" \
        --decrypt "$intermediate_file"; then
        log_error "Symmetric decryption failed"
        log_info "Possible reasons:"
        log_info "  - Wrong passphrase"
        log_info "  - File is corrupted"
        exit 1
    fi

    # Verify the decrypted file was created and has content
    if [[ ! -s "$output_file" ]]; then
        log_error "Decryption failed: output file is empty or missing"
        exit 1
    fi

    log_success "File decrypted successfully: $output_file"
    log_info "Decrypted file size: $(wc -c < "$output_file") bytes"
}

# Verify the integrity of an encrypted file
verify_file() {
    local encrypted_file="$1"

    validate_file "$encrypted_file" "read"

    log_info "Verifying encrypted file: $encrypted_file"

    # Try to list the packets without decrypting
    if gpg --list-packets "$encrypted_file" >/dev/null 2>&1; then
        log_success "File structure appears valid"
        gpg --list-packets "$encrypted_file" | head -10
    else
        log_error "File structure appears corrupted"
        exit 1
    fi
}

# Display comprehensive help
help() {
    cat <<EOF
🔐 gpgfy - Enhanced GPG Encryption Tool v2.0

DESCRIPTION:
    Encrypts files using a two-layer approach: symmetric encryption with AES256
    followed by asymmetric encryption. This provides both the security of public
    key cryptography and the efficiency of symmetric encryption for large files.

USAGE:
    gpgfy encrypt <input_file> <recipient_email>
        Encrypt the input file for the specified recipient.

    gpgfy decrypt <encrypted_file>
        Decrypt a file encrypted by this script.

    gpgfy verify <encrypted_file>
        Verify the structure of an encrypted file.

    gpgfy help
        Show this help message.

EXAMPLES:
    gpgfy encrypt secret.txt alice@example.com
    gpgfy decrypt secret.txt.asymmetric.gpg
    gpgfy verify secret.txt.asymmetric.gpg

SECURITY FEATURES:
    - AES256 symmetric encryption with SHA512 digest
    - High iteration count for key derivation (65M+ iterations)
    - Secure temporary file handling with shredding
    - Key validity checking
    - File integrity verification

REQUIREMENTS:
    - GPG (GNU Privacy Guard) must be installed
    - For encryption: recipient's public key in your keyring
    - For decryption: your private key and the symmetric passphrase

NOTES:
    - Encrypted files use the extension: .asymmetric.gpg
    - Decrypted files use the extension: .decrypted
    - Temporary files are securely deleted after use
    - The script will prompt before overwriting existing files

TROUBLESHOOTING:
    - Ensure GPG is properly configured: gpg --list-keys
    - Import recipient's key: gpg --import pubkey.asc
    - Check key validity: gpg --check-sigs recipient@example.com

For more information, see: https://gnupg.org/documentation/
EOF
}

# Main execution
main() {
    # Validate GPG availability
    validate_gpg

    case "${1:-}" in
        "encrypt")
            if [[ -z "${2:-}" || -z "${3:-}" ]]; then
                log_error "Missing arguments for encrypt command"
                echo
                help
                exit 1
            fi
            encrypt_file "$2" "$3"
            ;;
        "decrypt")
            if [[ -z "${2:-}" ]]; then
                log_error "Missing argument for decrypt command"
                echo
                help
                exit 1
            fi
            decrypt_file "$2"
            ;;
        "verify")
            if [[ -z "${2:-}" ]]; then
                log_error "Missing argument for verify command"
                echo
                help
                exit 1
            fi
            verify_file "$2"
            ;;
        "help"|"--help"|"-h"|"")
            help
            ;;
        *)
            log_error "Invalid command: ${1:-}"
            echo
            help
            exit 1
            ;;
    esac
}

# Execute main function with all arguments
main "$@"
```

---

### Purpose

* **Encrypt mode**:
  Encrypts a file *first symmetrically* with a passphrase (AES256), then *asymmetrically* with a recipient's public key (GPG).
* **Decrypt mode**:
  Reverses the process: decrypts the outer asymmetric layer, then decrypts the inner symmetric layer.

---

### How It Works

####  `encrypt_file()`

1. **Input Parameters**:

    * `$1`: input file (e.g., `input.txt`)
    * `$2`: GPG recipient email (e.g., `recipient@example.com`)
2. **Step 1: Symmetric Encryption**

    * Encrypts `input.txt` into `input.txt.symmetric.gpg` using a passphrase (you'll be prompted to enter one).
    * Algorithm: AES256.
3. **Step 2: Asymmetric Encryption**

    * Encrypts the symmetric file using GPG public key encryption for the recipient.
    * Output: `input.txt.asymmetric.gpg`
4. **Cleanup**: Deletes the intermediate symmetric file for security.

####  `decrypt_file()`

1. **Input Parameter**:

    * `$1`: Encrypted file (e.g., `input.txt.asymmetric.gpg`)
2. **Step 1: Asymmetric Decryption**

    * Decrypts the `.asymmetric.gpg` file using your private key, revealing the intermediate `.symmetric.gpg`.
3. **Step 2: Symmetric Decryption**

    * Prompts you for the original passphrase to decrypt the inner file.
    * Outputs `input.txt.decrypted`
4. **Cleanup**: Deletes the intermediate symmetric file.

---

### Usage

**Encrypt:**

```bash
./gpgfy.sh encrypt input.txt recipient@example.com
```

**Decrypt:**

```bash
./gpgfy.sh decrypt input.txt.asymmetric.gpg
```

---

### Why Use Both Symmetric and Asymmetric?

* **Symmetric (AES256)**: Fast and strong encryption, but requires securely sharing the password.
* **Asymmetric (GPG)**: Solves key sharing by encrypting with the recipient's public key.
* Combining both:

    * Protects the actual content with a symmetric key.
    * Protects the symmetric key with the recipient’s public key.

| Mode      | Action                                                     |
| --------- | ---------------------------------------------------------- |
| `encrypt` | Encrypt file with AES256, then encrypt result with GPG key |
| `decrypt` | Decrypt GPG layer, then decrypt AES256 layer               |

---

### Step-by-Step: Install `gpgfy.sh` as a system command

#### 1. **Make the Script Executable**

```bash
chmod +x gpgfy.sh
```

#### 2. **Move It to a Directory in Your PATH**

A typical location is `/usr/local/bin`, which is meant for custom scripts:

```bash
sudo mv gpgfy.sh /usr/local/bin/gpgfy
```

> You’re renaming it to `gpgfy` (dropping the `.sh`) so you can run it like: `gpgfy encrypt ...`

#### 3. **Verify It's in Your PATH**

Run:

```bash
which gpgfy
```

You should see:

```bash
/usr/local/bin/gpgfy
```

#### 4. **Test It**

Try:

```bash
gpgfy
gpgfy encrypt secrets.txt recipient@example.com
gpgfy decrypt secrets.txt.asymmetric.gpg
```

You should see the usage output.

---

## Conclusion

GPG is a vital tool for secure communication and data protection. With the right understanding and best practices, you can confidently manage your cryptographic keys, sign and verify data, and encrypt/decrypt files.

For advanced usage, consult the official GnuPG documentation: [https://gnupg.org/documentation/](https://gnupg.org/documentation/)
