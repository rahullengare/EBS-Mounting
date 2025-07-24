## **About the Project**

This project demonstrates how to **attach and mount an Amazon EBS (Elastic Block Store) volume** to an **EC2 (Elastic Compute Cloud) instance** using AWS. The goal is to expand the storage capacity of a running EC2 instance by adding a separate volume that can be used to store files, databases, or application data.

The process involves:

- Creating a new EBS volume
- Attaching it to an EC2 instance
- Formatting and mounting it on the Linux system

## **Tools and Technologies:**

- **Amazon Web Services (AWS)** – Cloud platform to host and manage infrastructure
- **Amazon EC2** – Service to launch and manage virtual servers in the cloud
- **SSH:** Secure terminal access to EC2 instance
- **Amazon Linux**: The operating system used on the EC2 instance.

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

## Step 2: Create EBS Volume

1. Go to **EC2 Dashboard**
2. In the left menu, click on **Volumes** under **Elastic Block Store**
3. Click on **Create Volume**
4. Set the following:
    - **Size** → `8 GiB`
    - **Availability Zone** → same as your EC2 instance (e.g., `us-east-1a`)
5. Leave other settings as default (volume type: `gp3` or `gp2`)
6. Click on **Create Volume**

## **Step 3: Attach EBS Volume to EC2 Instance**

1. After volume is created, select it from the list
2. Click on **Actions** → **Attach Volume**
3. Choose your running **EC2 instance** from the dropdown
4. Device name → leave default (`/dev/sdf`)
5. Click **Attach**

## Step 4: Format and Mount the Volume

1. Connect instance via SSH

```bash
ssh -i "pem-server-key.pem" ec2-user@ec2-100-26-31-151.compute-1.amazonaws.com
```

1. Check the volume is attached to your instance or check partition 

```bash
lsblk
```

1. Make the partition to new Volume 

```bash
sudo fdisk /dev/xvdf
			n -> n → new partition 
			+2G -> for make 2GB partition 
			w -> for the save all changes and Exit 
lablk   -> recheck 
```

1. Format Partitions with XFS

```bash
sudo mkfs.xfs /dev/xvdf1
sudo mkfs.xfs /dev/xvdf2
```

1. Create mount points

```bash
sudo mkdir /mnt
sudo mkdir /opt
```

1. Mount the partitions

```bash
sudo mount /dev/xvdf1 /mnt
sudo mount /dev/xvdf2 /opt
```

1. Verify mount points

```bash
lsblk
```

## Step 5: **Terminating Your instance**

1. Your use are done then got to AWS console
2. Click on EC2 → instance
3. Select instance You want to terminated
4. Click on Instance state
5. Choose **Terminate (delete) instance**
6. Now click delete

## Step 6: Delete your Volume

1. Your use are done then got to AWS console
2. Click on EC2 → volume 
3. Select instance You want to deleted
4. Click on Action
5. Choose **delete Volume** 
6. Now click delete
