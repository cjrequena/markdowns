---
layout: default
title: sns-to-sqs-different-accounts
parent: aws
grand_parent: cheatsheets
permalink: /docs/aws/sns-to-sqs-different-accounts
nav_order: 130
---
# How to send Amazon Simple Notification Service (SNS) messages to an Amazon Simple Queue Service (SQS) queue in a different AWS account
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Prerequisites
1. You should have an AWS account with access to both the SNS topic and SQS queue.

## Configuration Steps

### Configuring the SNS Topic

1. Go to the AWS Management Console and navigate to the Amazon SNS service.
2. Create a new SNS topic or select an existing topic that you want to use for sending messages to the SQS queue.
3. Note down the ARN (Amazon Resource Name) of the SNS topic, as it will be needed in the following steps.

### Configuring the SQS Queue in the Receiving Account

1. Sign in to the AWS Management Console using the account that owns the SQS queue.
2. Navigate to the Amazon SQS service.
3. Create a new SQS queue or select an existing queue where you want to receive the messages.
4. Note down the ARN of the SQS queue, as it will be used in the next step.

### Granting Permissions to the SNS Topic

1. Switch to the AWS account that owns the SNS topic.
2. Navigate to the IAM (Identity and Access Management) service in the AWS Management Console.
3. Create a new IAM policy or modify an existing policy to grant permissions to send messages to the SQS queue.
4. Here's an example policy that allows sending messages to the SQS queue:

   ```json
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": "sqs:SendMessage",
               "Resource": "arn:aws:sqs:<region>:<account-id>:<queue-name>"
           }
       ]
   }
   ```

   Replace `<region>` with the AWS region where the SQS queue is located, `<account-id>` with the AWS account ID that owns the SQS queue, and `<queue-name>` with the name of the SQS queue.

5. Attach the IAM policy to the IAM role or user that will be used to publish messages to the SNS topic.

### Configuring the SNS Topic Subscription

1. Switch back to the AWS account that owns the SNS topic.
2. Navigate to the Amazon SNS service.
3. Select the SNS topic that you want to send messages to the SQS queue.
4. Click on the "Create subscription" button.
5. Choose "Amazon SQS" as the protocol for the subscription.
6. Enter the ARN of the SQS queue in the "Endpoint" field.
7. Click on the "Create subscription" button to create the subscription.

### Confirming the Subscription

1. Switch to the AWS account that owns the SQS queue.
2. Navigate to the Amazon SQS service.
3. Select the SQS queue that should receive the messages.
4. In the "Details" section, copy the "Queue ARN".
5. Switch back to the AWS account that owns the SNS topic.
6. Navigate to the Amazon SNS service.
7. Select the SNS topic that sends messages to the SQS queue.
8. In the "Subscriptions" section, locate the subscription that corresponds to the SQS queue.
9. Click on the subscription's ARN to open the subscription details.
10. Click on the "Request confirmation" button.
11. Paste the SQS queue ARN and click on the "Confirm subscription" button.

Once the subscription is confirmed, you can start sending messages to the SNS topic
