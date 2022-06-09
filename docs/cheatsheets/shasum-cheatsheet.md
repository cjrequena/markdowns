---
layout: default
title: Shasum cheat sheet
parent: Cheat sheets
nav_order: 4
---
# Shasum cheat sheet
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---
Running shasum is often the quickest way to compute SHA message digests. The user simply feeds data to the script
through files or standard input, and then collects the results from standard output.

For verifying the integrity **(but not authenticity of data, i.e., who authored it or the origin of the file)** of a file,
it is necessary to run a checksum function on the file which will output a value and compare it to a previously stored
checksum value; if it matches we can be relatively confident that the file hasn't been tampered with or altered.

You might be asked to verify a file's sha1sum or sha2sum–all this means is calculating and verifying the cryptographic
sha1 or sha2 hash value or digest included in the file.
---

## SHA256
The program sha256sum is designed to verify data integrity using the SHA-256 (SHA-2 family with a digest length of
256 bits). SHA-256 hashes used properly can confirm both file integrity and authenticity. SHA-256 serves a similar
purpose to a prior algorithm recommended by Ubuntu, MD5, but is less vulnerable to attack.

Comparing hashes makes it possible to detect changes in files that would cause errors. The possibility of changes
(errors) is proportional to the size of the file; the possibility of errors increase as the file becomes larger.
It is a very good idea to run an SHA-256 hash comparison check when you have a file like an operating system install
CD that has to be 100% correct.

## Hash
{hash} is the string of characters. **A hash function** is any function that can be used to map data of arbitrary size to
fixed-size values. The values returned by a hash function are called hash values, hash codes, digests, or simply hashes.

The values are used to index a fixed-size table called a hash table. Use of a hash function to index a hash table
is called hashing or scatter storage addressing

## How to get the hash value of a file using sha1:
```sh
$ shasum -a 1 filename/path
```

## How to get a hash value of a file using sha2:
```sh
$ shasum -a 256 filename/path
```

## SHASUM check
```sh
$ echo "{hash}  {filename}" | shasum -a 256 -c -
```
**or**

```sh
$ echo | shasum -a 256 {filename} | shasum -a 256 -c -
```
---


