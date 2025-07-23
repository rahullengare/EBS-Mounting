# Attach and Mount an EBS Volume on EC2
## **About the Project**

This project demonstrates how to **attach and mount an Amazon EBS (Elastic Block Store) volume** to an **EC2 (Elastic Compute Cloud) instance** using AWS. The goal is to expand the storage capacity of a running EC2 instance by adding a separate volume that can be used to store files, databases, or application data.

The process involves:

- Creating a new EBS volume
- Attaching it to an EC2 instance
- Formatting and mounting it on the Linux system

## **Tools and Technologies:**

- **Amazon Web Services (AWS)** – Cloud platform to host and manage infrastructure
- **Amazon EC2** – Service to launch and manage virtual servers in the cloud
- **Amazon EBS (Elastic Block Store)** – Provides persistent block storage for EC2 instances
- **Linux (Amazon Linux/Ubuntu)** – Operating system installed on the EC2 instance
- **SSH (Secure Shell)** – Secure protocol to access the EC2 instance remotely
- **Key Pair** – Authentication method used to securely log in to EC2
- **Security Group** – Virtual firewall to manage network access rules
- **AWS Management Console / AWS CLI** – Web interface and command-line tool for managing AWS resources

## What is EBS?

> **Amazon EBS (Elastic Block Store)** is a storage service used with **EC2 instances** on AWS. It provides **persistent block-level storage**, like a virtual hard drive, that keeps data even if the instance is stopped. EBS is commonly used for **databases, applications, and file systems**. It is **scalable, reliable, and supports backups** through snapshots.
> 

## Step 1: Launch an EC2 Instance

1. Go to AWS Console → EC2
2. Launch Instance.
    - Name → EBS_Mounting
    - Choose AMI → Amazon Linux.
    - Instance type → t2.micro
    - Key pair → pem_server_key
    - security group → launch-wizard-1

## Step 2: Connect to EC2 via SSH

1. Connect to EC2 via SSH

```bash
ssh -i "pem-server-key.pem" ec2-user@ec2-54-164-193-230.compute-1.amazonaws.com
```

## Step 3: Create EBS Volume

1. Go to **EC2 Dashboard**
2. In the left menu, click on **Volumes** under **Elastic Block Store**
3. Click on **Create Volume**
4. Set the following:
    - **Size** → `8 GiB`
    - **Availability Zone** → same as your EC2 instance (e.g., `us-east-1a`)
5. Leave other settings as default (volume type: `gp3` or `gp2`)
6. Click on **Create Volume**

## **Step 4: Attach EBS Volume to EC2 Instance**

1. After volume is created, select it from the list
2. Click on **Actions** → **Attach Volume**
3. Choose your running **EC2 instance** from the dropdown
4. Device name → leave default (`/dev/sdf`)
5. Click **Attach**

## Step 5: Format and Mount the Volume

1. On your EC2 terminal, run to check devices:

```bash
lsblk
```

1. Format the EBS volume (only if new):

```bash
sudo mkfs -t ext4 /dev/xvdf
```

1. Create mount point:

```bash
sudo mkdir /mnt/ebs
```

1. Mount the volume:

```bash
sudo mount /dev/xvdf /mnt/ebs
```

1. Verify:

```bash
df -h
```

## Step 6: Terminate the Instance (When Done)

1. Go to **EC2 → Instances**
2. Select the instance you want to delete
3. Click on **Instance State**
4. Choose **Terminate Instance**
5. Confirm termination
