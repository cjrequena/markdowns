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

## Help
```sh=
Supported algorithms:
Pubkey: RSA, ELG, DSA, ECDH, ECDSA, EDDSA
Cipher: IDEA, 3DES, CAST5, BLOWFISH, AES, AES192, AES256, TWOFISH,
        CAMELLIA128, CAMELLIA192, CAMELLIA256
Hash: SHA1, RIPEMD160, SHA256, SHA384, SHA512, SHA224
Compression: Uncompressed, ZIP, ZLIB, BZIP2

Syntax: gpg [options] [files]
Sign, check, encrypt or decrypt
Default operation depends on the input data

Commands:

 -s, --sign                         make a signature
     --clear-sign                   make a clear text signature
 -b, --detach-sign                  make a detached signature
 -e, --encrypt                      encrypt data
 -c, --symmetric                    encryption only with symmetric cipher
 -d, --decrypt                      decrypt data (default)
     --verify                       verify a signature
 -k, --list-keys                    list keys
     --list-signatures              list keys and signatures
     --check-signatures             list and check key signatures
     --fingerprint                  list keys and fingerprints
 -K, --list-secret-keys             list secret keys
     --generate-key                 generate a new key pair
     --quick-generate-key           quickly generate a new key pair
     --quick-add-uid                quickly add a new user-id
     --quick-revoke-uid             quickly revoke a user-id
     --quick-set-expire             quickly set a new expiration date
     --full-generate-key            full featured key pair generation
     --generate-revocation          generate a revocation certificate
     --delete-keys                  remove keys from the public keyring
     --delete-secret-keys           remove keys from the secret keyring
     --quick-sign-key               quickly sign a key
     --quick-lsign-key              quickly sign a key locally
     --quick-revoke-sig             quickly revoke a key signature
     --sign-key                     sign a key
     --lsign-key                    sign a key locally
     --edit-key                     sign or edit a key
     --change-passphrase            change a passphrase
     --export                       export keys
     --send-keys                    export keys to a keyserver
     --receive-keys                 import keys from a keyserver
     --search-keys                  search for keys on a keyserver
     --refresh-keys                 update all keys from a keyserver
     --import                       import/merge keys
     --card-status                  print the card status
     --edit-card                    change data on a card
     --change-pin                   change a card's PIN
     --update-trustdb               update the trust database
     --print-md                     print message digests
     --server                       run in server mode
     --tofu-policy VALUE            set the TOFU policy for a key

Options controlling the diagnostic output:
 -v, --verbose                      verbose
 -q, --quiet                        be somewhat more quiet
     --options FILE                 read options from FILE
     --log-file FILE                write server mode logs to FILE

Options controlling the configuration:
     --default-key NAME             use NAME as default secret key
     --encrypt-to NAME              encrypt to user ID NAME as well
     --group SPEC                   set up email aliases
     --openpgp                      use strict OpenPGP behavior
 -n, --dry-run                      do not make any changes
 -i, --interactive                  prompt before overwriting

Options controlling the output:
 -a, --armor                        create ascii armored output
 -o, --output FILE                  write output to FILE
     --textmode                     use canonical text mode
 -z N                               set compress level to N (0 disables)

Options controlling key import and export:
     --auto-key-locate MECHANISMS   use MECHANISMS to locate keys by mail address
     --auto-key-import              import missing key from a signature
     --include-key-block            include the public key in signatures
     --disable-dirmngr              disable all access to the dirmngr

Options to specify keys:
 -r, --recipient USER-ID            encrypt for USER-ID
 -u, --local-user USER-ID           use USER-ID to sign or decrypt

(See the man page for a complete listing of all commands and options)

Examples:

 -se -r Bob [file]          sign and encrypt for user Bob
 --clear-sign [file]        make a clear text signature
 --detach-sign [file]       make a detached signature
 --list-keys [names]        show keys
 --fingerprint [names]      show fingerprints
```

---

## Shell Script: Symmetric → Asymmetric Encryption :: Asymmetric --> Symmetric Decryption

```shell
#!/bin/bash

# Filename: gpg_encrypt_symmetric_then_asymmetric.sh
# Description: Encrypts a file symmetrically, then asymmetrically.
#              Can also decrypt the result by reversing the steps.

# Encrypt: ./gpg_encrypt_symmetric_then_asymmetric.sh encrypt input.txt recipient@example.com
# Decrypt: ./gpg_encrypt_symmetric_then_asymmetric.sh decrypt input.txt.asymmetric.gpg

encrypt_file() {
    INPUT="$1"
    RECIPIENT="$2"
    SYM_FILE="${INPUT}.symmetric.gpg"
    FINAL_FILE="${INPUT}.asymmetric.gpg"

    echo "🔐 [1/2] Symmetric encryption of '$INPUT'..."
    gpg --batch --yes --symmetric --cipher-algo AES256 --output "$SYM_FILE" "$INPUT"

    echo "🔐 [2/2] Asymmetric encryption of '$SYM_FILE' for $RECIPIENT..."
    gpg --batch --yes --encrypt --recipient "$RECIPIENT" --output "$FINAL_FILE" "$SYM_FILE"

    rm -f "$SYM_FILE"
    echo "✅ Encrypted: $FINAL_FILE"
}

decrypt_file() {
    ENCRYPTED_FILE="$1"
    INTERMEDIATE_FILE="${ENCRYPTED_FILE%.asymmetric.gpg}.symmetric.gpg"
    OUTPUT_FILE="${ENCRYPTED_FILE%.asymmetric.gpg}.decrypted"

    echo "🔓 [1/2] Asymmetric decryption of '$ENCRYPTED_FILE'..."
    gpg --batch --yes --output "$INTERMEDIATE_FILE" --decrypt "$ENCRYPTED_FILE"

    echo "🔓 [2/2] Symmetric decryption of '$INTERMEDIATE_FILE'..."
    gpg --batch --yes --output "$OUTPUT_FILE" --decrypt "$INTERMEDIATE_FILE"

    rm -f "$INTERMEDIATE_FILE"
    echo "✅ Decrypted: $OUTPUT_FILE"
}

# Main
if [[ "$1" == "encrypt" && -n "$2" && -n "$3" ]]; then
    encrypt_file "$2" "$3"
elif [[ "$1" == "decrypt" && -n "$2" ]]; then
    decrypt_file "$2"
else
    echo "Usage:"
    echo "  Encrypt: $0 encrypt <input_file> <recipient_email>"
    echo "  Decrypt: $0 decrypt <encrypted_file>"
    exit 1
fi

```


---

## Conclusion

GPG is a vital tool for secure communication and data protection. With the right understanding and best practices, you can confidently manage your cryptographic keys, sign and verify data, and encrypt/decrypt files.

For advanced usage, consult the official GnuPG documentation: [https://gnupg.org/documentation/](https://gnupg.org/documentation/)
