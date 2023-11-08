---
layout: default
title: aws-ebs
parent: aws
grand_parent: cheatsheets
permalink: /docs/aws/aws-ebs
nav_order: 6
---
# AWS EBS
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## EBS
EBS is a block storage service that provides scalable, high-performance storage volumes for use with Amazon EC2 instances. 
Below, you'll find information on the various EBS storage classes and their respective quotas as of my last knowledge update 
in January 2022. Please note that AWS service offerings and quotas may have changed since then, so it's essential to check the 
AWS documentation for the most up-to-date information. [Amazon Elastic Block Store endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/ebs-service.html)

## Amazon EBS Storage Classes

### General Purpose SSD (gp2):

Amazon Elastic Block Store (EBS) offers General Purpose SSD (gp2) volumes designed for a wide range of workloads with balanced performance characteristics. Below is a detailed overview of the gp2 EBS volumes as of my last knowledge update in January 2022. Please verify the most up-to-date information in the AWS documentation, as offerings may have evolved since then.

- **Volume Size**: 1 GiB to 16 TiB

- **Throughput**: 128 MiB/s per GiB of volume size, with a minimum of 100 MiB/s

- **IOPS**: 3 IOPS per GiB of volume size, with a minimum of 100 IOPS and a maximum of 16,000 IOPS

- **Use Cases:** gp2 volumes are ideal for a broad spectrum of workloads, including web servers, development and testing environments, small to medium-sized databases, and other applications that require a balance of performance and cost-effectiveness.

- **Volume Size:** You can create gp2 volumes with sizes ranging from 1 GiB to 16 TiB.

- **Throughput:** The throughput of gp2 volumes is determined by the volume size. You get a baseline throughput of 128 MiB/s per GiB of volume size, with a minimum of 100 MiB/s. For example, a 100 GiB gp2 volume has a baseline throughput of 12,800 MiB/s.

- **IOPS Performance:** gp2 volumes provide a balanced IOPS-to-storage ratio. You get 3 IOPS per GiB of volume size, with a minimum of 100 IOPS. For example, a 100 GiB gp2 volume would offer 300 IOPS as its baseline performance.

- **Durability:** gp2 volumes provide a high level of data durability and availability for general-purpose workloads.

- **Burst Capability:** gp2 volumes have a burst capability, allowing them to burst up to 3,000 IOPS for a short duration. The burst duration depends on the accumulated I/O credits. Each volume earns I/O credits over time when not in use, which can be used during bursts.

- **I/O Credits:** To understand burst capabilities better, consider I/O credits. A gp2 volume earns credits when it is not using its maximum IOPS rate. These credits are then used during bursts when the volume's IOPS exceed its baseline. The more credits a volume has, the longer it can burst at a higher IOPS rate.

- **Baseline IOPS and Throughput:** The baseline IOPS and throughput can be sustained indefinitely, as long as you have credits to cover them. Once credits are exhausted, the volume operates at its baseline performance level.

- **Snapshot Performance:** Creating EBS snapshots from gp2 volumes is fully supported.

- **Multi-Attach Support:** gp2 volumes can be attached to only one EC2 instance at a time. Multi-Attach support is not available for gp2 volumes.

- **Data Encryption:** You can enable data encryption for gp2 volumes, ensuring data security at rest.

General Purpose SSD (gp2) volumes are versatile and cost-effective storage options suitable for a wide range of workloads. They offer a balance between performance and cost, making them a popular choice for many applications. It's essential to monitor I/O credits and adjust the volume size or type if your application consistently requires more performance than the baseline to avoid performance degradation.

Always consult the latest AWS documentation for any updates or changes to EBS offerings and performance characteristics.

### Provisioned IOPS SSD (io2 and io1):

Amazon Elastic Block Store (EBS) io classes, specifically io1 and io2, are designed for applications that require high levels of input/output operations per second (IOPS) and
low-latency storage performance. Below is a detailed overview of the EBS io classes as of my last knowledge update in January 2022. Please verify the most up-to-date information in
the AWS documentation, as offerings may have evolved since then.

- **Volume Size**: 4 GiB to 16 TiB

- **Throughput**: 256 KiB/s per provisioned IOPS, with a minimum of 100 MiB/s

- **IOPS**: Up to 64,000 IOPS (io2) or 32,000 IOPS (io1) per volume

- **Max IOPS-to-Volume Size Ratio**: 500 IOPS per GiB (io2) or 50 IOPS per GiB (io1)

#### **io1 (Provisioned IOPS SSD):**

- **Use Cases:** io1 volumes are suitable for applications with high I/O requirements, such as relational databases (e.g., MySQL, PostgreSQL), NoSQL databases (e.g., Cassandra), and transactional applications.

- **Volume Size:** Ranging from 4 GiB to 16 TiB.

- **IOPS Performance:** You can provision a specific number of IOPS per volume, ranging from 100 IOPS to a maximum of 32,000 IOPS.

- **Throughput:** The throughput of io1 volumes is directly related to the provisioned IOPS, with 256 KiB/s per provisioned IOPS as the baseline.

- **Durability:** io1 volumes are designed to provide high durability and availability for critical applications.

#### **io2 (Provisioned IOPS SSD):**

- **Use Cases:** io2 volumes are designed for critical production workloads that require consistently high IOPS performance, low-latency storage, and enhanced durability. They are suitable for applications like SAP HANA, high-performance databases, and financial applications.

- **Volume Size:** Ranging from 4 GiB to 16 TiB.

- **IOPS Performance:** io2 volumes support provisioned IOPS ranging from 100 IOPS to a maximum of 64,000 IOPS. They also offer Burst IOPS, which allows you to burst to 64,000 IOPS for volumes at or above 100 GiB in size.

- **Throughput:** The throughput is based on the provisioned IOPS, with 256 KiB/s per provisioned IOPS as the minimum throughput.

- **Durability:** io2 volumes are designed to deliver the highest level of data durability compared to other EBS volume types. They provide 99.999% durability.

- **Burst Capability:** For io2 volumes 100 GiB and larger, there is a burst capability that allows the volume to burst to 64,000 IOPS and 1,000 MiB/s for up to 30 minutes.

- **Multi-Attach Support:** io2 volumes support Multi-Attach, which enables attaching a single EBS volume to multiple EC2 instances simultaneously. This is useful for applications that require shared storage access.

- **Provisioned IOPS to Volume Size Ratio:** With io2 volumes, you can achieve higher IOPS per GiB, with a maximum ratio of 4,000 IOPS per GiB.

- **IO2 Block Express:** This is a sub-category of io2 volumes designed for ultra-high performance and lower-latency workloads. IO2 Block Express volumes offer high IOPS and throughput with a provisioned IOPS-to-volume-size ratio of 4,000 IOPS per GiB.

Remember that when choosing between io1 and io2 volumes, io2 is often preferred due to its enhanced durability, burst capability, and consistent 
high performance, making it suitable for a wide range of demanding applications. Additionally, io2 Block Express provides the highest levels of IOPS and throughput performance. Always consult the latest 
AWS documentation for any updates or changes to EBS offerings and performance characteristics.

### Throughput Optimized HDD (st1):

- *Volume Size*: 500 GiB to 16 TiB

- *Throughput*: A baseline of 40 MiB/s per TiB of volume size, with a burst rate of up to 250 MiB/s per TiB

### Cold HDD (sc1):

- *Volume Size*: 500 GiB to 16 TiB

- *Throughput*: A baseline of 12 MiB/s per TiB of volume size, with a burst rate of up to 80 MiB/s per TiB

- **Magnetic (standard):**
    
    - *Volume Size*: 1 GiB to 1 TiB
  
    - *Throughput*: About 40-90 MiB/s

- **Provisioned IOPS SSD (io2 Block Express):**

    - *Volume Size*: 4 GiB to 64 TiB
  
    - *Throughput*: 256 KiB/s per provisioned IOPS, with a minimum of 100 MiB/s
  
    - *IOPS*: Up to 256,000 IOPS per volume
  
    - *Max IOPS-to-Volume Size Ratio*: 4,000 IOPS per GiB

- **NVMe-based SSD (gp3):**

    - *Volume Size*: 1 GiB to 16 TiB
  
    - *Throughput*: 125 MiB/s per TiB of volume size, with a minimum of 1,000 MiB/s for volumes under 5.2 TiB
  
    - *IOPS*: 3,000 IOPS per TiB of volume size

- **NVMe-based SSD (io2 Block Express):**
    
    - *Volume Size*: 4 GiB to 64 TiB
  
    - *Throughput*: 256 KiB/s per provisioned IOPS, with a minimum of 1,000 MiB/s
  
    - *IOPS*: Up to 128,000 IOPS per volume
  
    - *Max IOPS-to-Volume Size Ratio*: 4,000 IOPS per GiB

**Additional Information:**

- Snapshots of EBS volumes are available for backup, replication, and disaster recovery.

- Encryption can be enabled on EBS volumes.

- You can attach and detach EBS volumes from EC2 instances.

- EBS volumes are specific to an Availability Zone.

## Mounting and configuring an Amazon Elastic Block Store (EBS) volume

> <p style="color: red">[!IMPORTANT]</p>  
> Replace `/dev/xvdX` with the proper device name.

Mounting and configuring an Amazon Elastic Block Store (EBS) volume on an Amazon Elastic Compute Cloud (EC2) instance involves several steps. I'll guide you through the process, assuming you 
have an EC2 instance and an EBS volume already created.

**Step 1: Attach the EBS Volume to Your EC2 Instance**

1. Log in to the AWS Management Console.

2. Navigate to the EC2 Dashboard.

3. Select "Volumes" from the left-hand navigation pane.

4. Locate the EBS volume you want to attach to your EC2 instance.

5. Select the volume and click "Actions."

6. Choose "Attach Volume."

7. Select the EC2 instance to which you want to attach the volume and specify the device name (e.g., /dev/sdf). The device name can vary depending on your setup.

8. Click "Attach."

**Step 2: Connect to Your EC2 Instance**

You need to connect to your EC2 instance to configure the attached EBS volume. Use SSH for Linux instances or RDP for Windows instances. You should have the appropriate key pair or password for this.

**Step 3: Check the Attached Volume**

Once you're connected to your EC2 instance, you need to check if the attached volume is available. Run the following command (for Linux):

```shell
$ lsblk
```
> **lsblk** lists information about all available or the specified block devices. The lsblk command reads the sysfs filesystem and udev db to gather information.

For Windows, you can check in "Disk Management."

You should see the attached volume listed, often as `/dev/xvdX` on Linux or as a new disk in Disk Management on Windows.

**Step 4: Check the Filesystem Volume**
```shell
sudo blkid -o value -s TYPE /dev/xvdX
sudo file -s /dev/xvdX
```

**Step 5: Format and Mount the EBS Volume (Linux)**

If you are using a Linux instance, follow these steps to format and mount the EBS volume:

Create a filesystem on the EBS volume. For example, to create an ext4 filesystem, you can use the following command:

For ext4:

```shell
sudo mkfs -t ext4 /dev/xvdX
```
For xfs:

```shell
sudo mkfs -t xfs /dev/xvdX
```

Create a directory where you want to mount the EBS volume. For example:

```shell
sudo mkdir /mnt/myebsvolume
```

Mount the EBS volume to the directory:

```shell
sudo mount /dev/xvdX /mnt/myebsvolume
```

Verify the EBS volume was mounted:

```shell
df -k
```

**To mount an attached volume automatically after reboot**

(Optional) Create a backup of your /etc/fstab file that you can use if you accidentally destroy or delete this file while editing it.

```shell
sudo cp /etc/fstab /etc/fstab.BACKUP
```

Use the blkid command to find the UUID of the device. Make a note of the UUID of the device that you want to mount after reboot. You'll need it in the following step.

For example, the following command shows that there are two devices mounted to the instance, and it shows the UUIDs for both devices.

```shell
sudo blkid
```
```shell
/dev/xvda1: LABEL="/" UUID="ca774df7-756d-4261-a3f1-76038323e572" TYPE="xfs" PARTLABEL="Linux" PARTUUID="02dcd367-e87c-4f2e-9a72-a3cf8f299c10"
/dev/xvdf: UUID="aebf131c-6957-451e-8d34-ec978d9581ae" TYPE="xfs"
```

Open the /etc/fstab file using any text editor, such as nano or vim.
```shell
sudo vim /etc/fstab
```

Add the following entry to /etc/fstab to mount the device at the specified mount point. The fields are the UUID value returned by blkid (or lsblk for Ubuntu 18.04), the mount point, 
the file system, and the recommended file system mount options. For more information about the required fields, run man fstab to open the fstab manual.

```text
UUID=aebf131c-6957-451e-8d34-ec978d9581ae  /mnt/myebsvolume  xfs  defaults,nofail  0  2
```

To verify that your entry works, run the following commands to unmount the device and then mount all file systems in /etc/fstab. If there are no errors, the /etc/fstab file is OK and your file system will mount automatically after it is rebooted.

```shell
sudo umount /data
sudo mount -a
```

**Step 6: Format and Assign a Drive Letter (Windows)**

If you are using a Windows instance, follow these steps to format and assign a drive letter to the EBS volume:

1. Open "Disk Management" by right-clicking on "Computer" or "This PC" and selecting "Manage." Then go to "Disk Management" in the left pane.

2. You should see the attached EBS volume. Right-click on it, initialize the disk, create a new simple volume, format it with a filesystem (e.g., NTFS), and assign a drive letter.

**Step 7: Verify the Mount**

Check that the EBS volume is mounted correctly by navigating to the directory where you mounted it. For Linux, use `cd /mnt/myebsvolume` 
(or your chosen directory), and for Windows, you should see the drive letter you assigned.

You've now successfully attached, formatted, and mounted your EBS volume to your EC2 instance. It's ready for use as additional storage for your 
application or data.

### Automating the process of attaching, formatting, and mounting Amazon Elastic Block Store (EBS)

Automating the process of attaching, formatting, and mounting Amazon Elastic Block Store (EBS) volumes to your EC2 instances can save time and ensure consistency across multiple instances. You can use tools like AWS CloudFormation, AWS Systems Manager (SSM), or configuration management tools like AWS OpsWorks, Ansible, Puppet, or Chef. Here, I'll provide an example of how to automate this process using AWS Systems Manager Automation Documents. This method is particularly suitable for ad-hoc automation tasks on AWS.

**Note:** Before starting, ensure that you have the necessary IAM permissions to perform these tasks. Additionally, you should have already created the EBS volumes and EC2 instances you want to automate.

**Step 1: Create an SSM Automation Document**

1. Open the AWS Management Console and navigate to AWS Systems Manager.

2. In the Systems Manager navigation pane, choose "Automation."

3. Choose "Create Automation."

4. In the "Name" field, provide a name for your Automation Document (e.g., "AttachEBSVolume").

5. In the "Document type" section, select "Automation document."

6. In the "Targets" section, choose the EC2 instances to which you want to attach the EBS volume.

7. In the "Document content" section, write a script that includes commands to attach, format, and mount the EBS volume. Here's an example:

```json
{
  "schemaVersion": "2.2",
  "description": "Attach and mount EBS volume",
  "mainSteps": [
    {
      "action": "aws:runShellScript",
      "name": "attachVolume",
      "inputs": {
        "runCommand": [
          "aws ec2 attach-volume --volume-id <your-volume-id> --instance-id <your-instance-id> --device /dev/xvdX",
          "mkfs -t ext4 /dev/xvdX",
          "mkdir /mnt/myebsvolume",
          "mount /dev/xvdX /mnt/myebsvolume",
          "echo '/dev/xvdX /mnt/myebsvolume ext4 defaults 0 0' >> /etc/fstab"
        ]
      }
    }
  ]
}
```

- Replace `<your-volume-id>` with the EBS volume ID you want to attach.
- Replace `<your-instance-id>` with the EC2 instance ID where you want to attach the volume.
- Replace `/dev/xvdX` and `/mnt/myebsvolume` with the appropriate device name and mount point.

8. Choose "Create Automation document."

**Step 2: Execute the Automation Document**

1. In the Systems Manager console, select the Automation document you created.

2. Choose "Execute automation."

3. Specify the targets (EC2 instances) where you want to perform the automation.

4. Choose "Execute automation."

AWS Systems Manager will execute the automation document on the specified instances, attaching, formatting, and mounting the EBS volume as defined in the document.

This automation document is a basic example, and you can expand on it to suit your specific needs, such as handling error scenarios or automating additional tasks. Automation documents can be versioned, allowing you to manage and update them over time.

Remember to review and adapt security settings and IAM roles to ensure proper access and control when automating tasks in your AWS environment.
