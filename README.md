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

![Project Screenshot](/images/instance.jpg)

## Step 2: Create EBS Volume

1. Go to **EC2 Dashboard**
2. In the left menu, click on **Volumes** under **Elastic Block Store**
3. Click on **Create Volume**
4. Set the following:
    - **Size** → `8 GiB`
    - **Availability Zone** → same as your EC2 instance (e.g., `us-east-1a`)
5. Leave other settings as default (volume type: `gp3` or `gp2`)
6. Click on **Create Volume**
![Project Screenshot](/images/create_volume.jpg)
![Project Screenshot](/images/volume-done.jpg)

## **Step 3: Attach EBS Volume to EC2 Instance**

1. After volume is created, select it from the list
2. Click on **Actions** → **Attach Volume**
3. Choose your running **EC2 instance** from the dropdown
4. Device name → leave default (`/dev/sdf`)
5. Click **Attach**
![Project Screenshot](/images/attacting-volume.jpg)
![Project Screenshot](/images/attach-volume.jpg)

## Step 4: Format and Mount the Volume

1. Connect instance via SSH

```bash
ssh -i "pem-server-key.pem" ec2-user@ec2-100-26-31-151.compute-1.amazonaws.com
```
![Project Screenshot](/images/connect-instance.jpg)

2. Check the volume is attached to your instance or check partition 

```bash
lsblk
```
![Project Screenshot](/images/lsblk.jpg)

3. Make the partition to new Volume 

```bash
sudo fdisk /dev/xvdf
			n -> n → new partition 
			+2G -> for make 2GB partition 
			w -> for the save all changes and Exit 
lablk   -> recheck 
```
![Project Screenshot](/images/partition.jpg)

4. Format Partitions with XFS

```bash
sudo mkfs.xfs /dev/xvdf1
sudo mkfs.xfs /dev/xvdf2
```
![Project Screenshot](/images/format-volume.jpg)

5. Create mount points

```bash
sudo mkdir /mnt
sudo mkdir /opt
```

6. Mount the partitions

```bash
sudo mount /dev/xvdf1 /mnt
sudo mount /dev/xvdf2 /opt
```

7. Verify mount points

```bash
lsblk
```
![Project Screenshot](/images/mount.jpg)

## Step 5: **Terminating Your instance**

1. Your use are done then got to AWS console
2. Click on EC2 → instance
3. Select instance You want to terminated
4. Click on Instance state
5. Choose **Terminate (delete) instance**
6. Now click delete
![Project Screenshot](/images/delete-instance.jpg)

## Step 6: Delete your Volume

1. Your use are done then got to AWS console
2. Click on EC2 → volume 
3. Select instance You want to deleted
4. Click on Action
5. Choose **delete Volume** 
6. Now click delete
![Project Screenshot](/images/delete-volume.jpg)
