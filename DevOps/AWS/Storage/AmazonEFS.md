# Introduction

### Agenda

- What is Amazon Elastic File System?
- Storage Classes and Performance Options
- Creating an EFS File System
- Mounting your EFS File System
- Managing EFS Security
- Importing Data
- Course Summary

---

## Understanding EFS

### What is the Amazon Elastic File System?

Where does EFS fit in the storage scene?
- Amazon S3 - Object storage solution
	- Object storage stores everything as a single object, not in small chunks or blocks. With this type of storage, you upload a file and if the file changes to replace it, the entire file will be replaced.
	- This type of storage is best for situations where files are written once and then accessed many times.
	- It's not optimal for situations that require both heavy read and write access at the same time.
	- So Amazon S3 is usually used for storage of large files such as video files, images, static websites, and backup archives.
	- For example, Netflix uses S3 for their data streaming service. They upload large movie files once and then subscribers access and play the movies many, many times. 
- Amazon Elastic Block Store or EBS - Block level storage
	- Files are not stored as single objects. They're stored in small chunks of blocks so that only the portion of the file that is changed will be updated.
	- This type of storage is optimized for low latency access and when fast, concurrent read and write operations are needed.
	- EBS provides persistent block storage volumes for use with a single EC2 instance. As described, EBS is persistent, meaning that even if you stop or terminate an EC2 instance that's using EBS, the data on the EBS volume remains intact.
	- You should use this type of storage like a computer hard drive where you store operating system files, applications and other files you wish to obtain for use with your EC2 instance.
- Amazon Elastic File Storage or EFS - File level storage
	- also optimized for low latency access, but unlike EBS, it supports access by multiple EC2 instances at once.
	- It appears to users like a file manager interface and uses standard file system semantics such as locking files, renaming files, updating files and uses a hierarchy structure. This is just like what we're used to on standard premise-based systems.
	- This type of storage allows you to store files that are accessible to network resources.

### How are people traditionally used to access network files and resources?

- users access files by browsing network resources that connect to a server, perhaps via a mapped drive that has been configured for them, and once they connect, they will see a tree view of available folders and files.
- This functionality is generally provided by various local area network systems such as file servers or storage area network, a SAN, or network-attached storage, a NAS.

### Cloud based, specifically within AWS and EFS
- EFS provides simple, scalable file storage for use with Amazon EC2 instances.
- Much like traditional file servers, or a SAN or a NAS, Amazon EFS provides the ability for users to browse cloud network resources.
- EC2 instances can be configured to access Amazon EFS instances using configured mount points.
- Now, mount points can be created in multiple availability zones that attach to multiple EC2 instances.
- So, much like your traditional land servers, EC2 instances are connected to a network file system, Amazon EFS.
- So from a user standpoint, the result is the same. The user accesses network resources just as they always have done except for now, it's done using cloud resources.

### Properties of EFS

- EFS is a fully managed, highly available and durable service that allows you to create shared file systems that can easily scale to petabytes in size with low latency access.
- EFS has been designed to maintain a high level of throughput in addition to low latency access response, and these performance factors make EFS a desirable storage solution for a wide variety of workloads, and use cases and can meet the demands of tens, hundreds or even thousands of EC2 instances concurrently.
- Being a managed service, there is no need for you to provision any file servers to manage the storage elements or provide any maintenance of those servers. This makes it a very simple option to provide file-level storage within your environment.
- It uses standard operating system APIs, so any application that is designed to work with standard operating system APIs will work with EFS. It supports both NFS versions 4.1 and 4.0, and uses standard file system semantics such as strong consistency and file locking.
- It's replicated across availability zones in a single region making EFS a highly reliable storage service.

### Summary

- As the file system can be accessed by multiple instances, it makes it a very good storage option for applications that scale across multiple instances allowing for parallel access of data.
- The EFS file system is also regional, and so any application deployments that span across multiple availability zones can all access the same file systems providing a level of high availability of your application storage layer.

---

## Storage Classes and Performance Options

### Storage Classes

Amazon EFS offers 2 different storage classes
1. Standard (Default)
	1. Access: Anytime
	2. Cost: Standard cost
	3. Performance: Standard latency
2. Infrequent Access (IA) (Cheaper)
	1. Access: Infrequent
	2. Cost: Reduced
	3. Performance: Higher first-byte latency

#### Cost
EFS IA is cheaper than standard storage
EFS IA also charges for read and write
EFS standard class charges on the amount of storage used each month

#### Availability
Both provide same level of availability and durability

#### EFS Lifecycle Management
- Similar to S3 lifecycle management where files are moved from Standard to IA based on their last accessed time.
- You can set a time period like 30d, 60d and 90d after which data will be moved to the IA storage class to save cost
- However, as soon as the file in IA is accessed, the timer is reset and the file is moved back to Standard storage class
- The only exception from moving Standard to IA is for those files below 128Kb and any metadata of your files which will all remain in the Standard storage class
- The lifecycle feature can be switched on or off* (after Feb 13th '19)

### Performance Modes

EFS has 2 different performance modes
1. General purpose
	1. When to use
		1. Default performance mode and is typically used for most use cases. For example, home directories and general file-sharing environments. It offers an all-round performance and low latency file operation,
	2. Throughput: Standard
	3. IOPS: <7K
	4. Latency: Low latency
2. Max I/O
	1. When to use
		1. for many 1000s of EC2 instances concurrently
	2. Throughput: Unlimited
	3. IOPS: >7K
	4. Latency: Higher latency (The downside is, however, that your file operation latency will take a negative hit over that of General Purpose.)

The best way to determine which performance option that you need is to run tests alongside your application. If your application sits comfortably within the limit of 7,000 operations per second, then General Purpose will be best suited, with the added plus point of lower latency. However, if your testing confirms 7,000 operations per second may be reached or exceeded, then select Max I/O.

When using the General Purpose mode of operations, EFS provides a CloudWatch metric percent I/O limit, which will allow you to view operations per second as a percentage of the top 7,000 limit. This allows you to make the decision to migrate and move to the Max I/O file system, should your operations be reaching that limit. 

### Throughput Modes

Data throughput patterns on file systems generally go through periods of relatively low activity with occasional spikes in burst usage, and EFS provisions throughput capacity to help manage this random activity of high peaks.

Throughput is measured by the rate of mebibytes. The 2 modes offered are
1. Bursting Throughput (Default)
	1. The amount of throughput scales as your filesystem grows.
	   So the more you store, the more throughput is available to you.
	2. The default throughput available is capable of bursting to 100 mebibytes per second, however, with the standard storage class, this can burst to 100 mebibytes per second per tebibyte of storage used within the file system.
	3. So, for example, presume you have five tebibytes of storage within your EFS file system. Your burst capacity could reach 500 mebibytes per second.
	4. The duration of throughput bursting is reflected by the size of the file system itself. Through the use of credits, which are accumulated during periods of low activity, operating below the baseline rate of throughput set at 50 mebibytes per tebibyte of storage used, which determines how long EFS can burst for. Every file system can reach its baseline throughput 100% of the time. By accumulating, getting credits, your file system can then burst above your baseline limit.
	5. The number of credits will dictate how long this throughput can be maintained for, and the number of burst credits for your file system can be viewed by monitoring the CloudWatch metric of BurstCreditBalance.
	6. If you're finding that you're running out of credits too often, then you might consider using the Provisioned Throughput mode
2. Provisioned Throughput
	1. Provisioned Throughput allows you to burst above your allocated allowance, which is based upon your file system size.
	2. So if your file system was relatively small but the use case for your file system required a high throughput rate, then the default bursting throughput options may not be able to process your request quick enough. In this instance, you would need to use provisioned throughput.
	3. However, this option does incur additional charges, and you'll pay additional costs for any bursting above the default option of bursting throughput. That brings me to the end of this lecture, now I want to shift my focus on creating and connecting to an EFS file system from a Linux based instance.

---

# EFS In Practice

## Creating an EFS File System

### Mounting methods

1. Linux NFS - https://docs.aws.amazon.com/efs/latest/ug/mounting-fs-old.html
2. EFS mount helper (demo focuses on this)
	1. Simplifies the mount process
	2. Log to /var/log/amazon/efs
	3. Automatically connects to EFS at startup by editing /etc/fstab

### Prerequisites before connecting to EFS Filesystem from your EC2 Instances

1. Create EFS filesystem and EFS mount targets
2. Be running an EC2 instance with EFS Mount Helper
3. Needs an EC2 in a VPC configured to use Amazon DNS Servers with DNS hostnames
4. A security group allowing NFS filesystem access your EC2
5. Be able to connect to your Linux EC2 instance

### Demo

Covers
- EC2 Security Groups
- Mount Targets
- Lifecycle Management
- Throughput & Performance Mode
- Encryption

_TODO: insert image from Evernote_

---

_to be continued..._
