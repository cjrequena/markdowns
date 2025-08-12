---
layout: default
title: secure-secret-sharing-using-gpg
parent: software-engineering
nav_order: 30
---
# Secure secret sharing using gpg
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Abstract
This document outlines a complete workflow for securely exchanging sensitive information between a client and a server 
using [PGP](https://cjrequena.com/markdowns/docs/cryptography/gpg) encryption. It details the step-by-step process for clients to generate a PGP key pair, verify their email, 
and publish their public key on keys.openpgp.org via command-line tools, graphical interfaces, or dedicated applications 
across different operating systems. On the server side, it describes how to fetch, verify, and use the client’s public key 
to encrypt confidential data before delivery, ensuring only the intended recipient can decrypt it. Additional recommendations 
include optional features such as one-time downloads, expiration metadata, signed payloads, and audit trails to enhance 
security and traceability. A final summary table consolidates the protocol steps for quick reference.

## Use case
A secure protocol for delivering API credentials to a client using PGP ([GPG](https://cjrequena.com/markdowns/docs/cryptography/gpg)) encryption. The client generates a PGP key pair, 
verifies their email, and uploads their public key to keys.openpgp.org. The API provider retrieves and verifies the public key, 
then encrypts the API credentials so only the client’s private key can decrypt them. Step-by-step instructions cover key generation 
for multiple platforms, key publishing, secure encryption, and decryption. Optional enhancements, such as one-time downloads, 
key expiry checks, and signed payloads, further strengthen confidentiality and integrity. This process ensures that API secrets 
are exchanged over untrusted channels without risk of exposure.

## Client-Side (Your Consumer)

### Step 1: Client Generates a PGP Key Pair

This is the foundation of the whole protocol — the client needs to generate a PGP key pair and upload their **public key** to [`https://keys.openpgp.org`](https://keys.openpgp.org).

_**Required Info for All Methods:**_

- **Name**: Client name or company name
- **Email**: Must be real (used for verifying key with the server)
- **Key Type**: RSA 2048+ or ECC
- **Passphrase**: Strong and secure; protects the private key

_**Option A: Command Line (Linux & Windows WSL)**_

If comfortable with the terminal:

```bash
gpg --full-generate-key
```

- Choose RSA and RSA
- Key size: 2048 or 4096
- Set expiry: (e.g. 1 year)
- Enter name and verified email
- Choose a strong passphrase

After creation:

```bash
gpg --armor --export client@example.com > pubkey.asc
```

_**Option B: GPG Suite (macOS)**_

_Recommended for macOS users_

1. Download: <https://gpgtools.org>
2. Open **GPG Keychain**
3. Click ➕ to create a new key
4. Enter name, email, and passphrase
5. Click “Generate Key”

Export Public Key:

- Right-click your key → **Export…**
- Choose `.asc` format

_**Option C: Kleopatra (Windows GUI)**_

_Recommended for Windows users_

1. Install **Gpg4win**: <https://gpg4win.org>
    - Includes Kleopatra GUI
2. Launch **Kleopatra**
3. Menu → **File → New Key Pair**
4. Choose “Create a personal OpenPGP key pair”
5. Fill name and email
6. Set an expiration date and passphrase
7. Finish the wizard

Export Public Key:

- Right-click the key → **Export…**
- Save as `pubkey.asc`

### Step 2: Upload Public Key to keys.openpgp.org

Regardless of how the key was generated:

_**Option 1: Upload Manually**_

1. Go to: <https://keys.openpgp.org>
2. Click “Submit Key”
3. Upload your `pubkey.asc`
4. Click the verification link sent to your email

_**Option 2: From Terminal (if using GPG CLI)**_

```bash
gpg --send-keys --keyserver hkps://keys.openpgp.org <KEY_ID>
```

Replace `<KEY_ID>` with the key fingerprint or short ID:

```bash
gpg --list-keys
```

Then check your inbox and click the confirmation link to publish it.

**You now have a PGP public key tied to an email address, hosted on keys.openpgp.org.**

## Server-Side (Your System)

### Step 3: Fetch the Client’s Public Key

```bash
curl "https://keys.openpgp.org/vks/v1/by-email/client@example.com" > client_pubkey.asc
```

Or use your preferred HTTP client (Python, Go, etc.).

### Step 4: Verify the Key (Optional but Recommended)

You may want to verify:

- The UID contains the client’s expected email.
- The key is not expired or revoked.
- The key strength is sufficient (e.g., RSA 2048+).

Use GPG to import and inspect:

```bash
gpg --import client_pubkey.asc
gpg --list-keys client@example.com
```

### Step 5: Encrypt Secrets Using the Client's Public Key

Let’s say your secret is in a file:

```bash
echo "CLIENT_ID=abc123
CLIENT_SECRET=xyz789
API_KEY=api_key_456" > client_secrets.txt
```

Encrypt with GnuPG:

```bash
gpg --encrypt --armor --recipient client@example.com client_secrets.txt
```

This creates a file: `client_secrets.txt.asc` — an ASCII-armored, PGP-encrypted version.

You can now send this securely to the client:

- Email
- Dashboard download
- One-time HTTPS link

Only **the client with the private key** can decrypt it.

## Client-Side (Decrypting the Secrets)

When the client receives `client_secrets.txt.asc`, they run:

```bash
gpg --decrypt client_secrets.txt.asc
```

If their private key is loaded and passphrase is entered, it decrypts to:

```
CLIENT_ID=abc123
CLIENT_SECRET=xyz789
API_KEY=api_key_456
```

## Optional Enhancements

| Feature                 | Description                                                              |
| ----------------------- | ------------------------------------------------------------------------ |
| **One-Time Download**   | Host the `.asc` file on a secure server, destroy it after first access.  |
| **Expiration Metadata** | Include issue/expiration timestamps in the plaintext or as PGP notation. |
| **Key Expiry Checking** | Before encrypting, check if the client’s key is still valid.             |
| **Signed Payload**      | Optionally sign the secret payload with your server’s private PGP key.   |
| **Audit Trail**         | Log key fingerprints, timestamps, and delivery events.                   |

## Summary

| Step | Party  | Action                                            |
| ---- | ------ | ------------------------------------------------- |
| 1    | Client | Generates PGP key and verifies email              |
| 2    | Server | Fetches public key by email from keys.openpgp.org |
| 3    | Server | Encrypts secret payload with client’s key         |
| 4    | Server | Delivers encrypted blob to client                 |
| 5    | Client | Decrypts with their private key                   |

---
