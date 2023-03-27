---
layout: default
title: aws-cli
parent: cheatsheets
nav_order: 9
---
# aws-cli cheatsheet
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Installation
You can install the AWS CLI on various platforms such as Windows, macOS, and Linux. Refer to the official AWS CLI 
documentation for detailed instructions on how to install the CLI on your platform.

### On macOS    

By following these steps, you can install the AWS CLI on macOS using Homebrew. This is a simple and convenient 
way to install the AWS CLI, and it ensures that you have the latest version of the CLI and its dependencies.

**Step 1: Install Homebrew**    
If you don't have Homebrew installed, you can install it by running the following command in your terminal:
````bash
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
````
This command will download and run the Homebrew installation script.

**Step 2: Install the AWS CLI**     
Once Homebrew is installed, you can install the AWS CLI by running the following command:

```bash
$ brew install awscli
```
This command will download and install the AWS CLI and its dependencies.

**Step 3: Verify the installation**     
After the installation is complete, you can verify that the AWS CLI is installed by running the following command:

```bash
$ aws --version
```
This command should output the version number of the AWS CLI that you just installed.

## Configuration
After installing the AWS CLI, you need to configure it to access your AWS resources. The configuration process 
involves creating an IAM user, generating an access key and secret access key, and configuring the CLI with the 
access keys.

You can configure the AWS CLI by using the **aws configure** command. The command prompts you to enter your 
AWS access key ID, secret access key, default region, and default output format.

## Test 3
## Test 4
## Test 5
## Test 6
