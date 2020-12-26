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

## Managing EFS Security

### Agenda

- Access control and the permissions required to both operate and create your EFS filesystem
- How EFS manages data encryption
- Necessary security groups

### Access Control

Before you can create and manage your EFS file system, you need to ensure that you have the correct permissions to do so.
- To initially create your EFS file system you need to ensure that you have 'Allow' access the following services:
	- `elasticfilesystem:CreateFileSystem`
	- `elasticfilesystem:CreateMountTarget`
	- `ec2:DescribeSubnet`
	- `ec2:CreateNetworkInterface`
	- `ec2:DescribeNetworkInterface`
- When applying these permissions to your policies, the resource for the elastic file system actions will point to the following resource:
	- Resource: "arn:aws:elasticfilesystem:us=west-2:<account-id>:file-system/*"
- A resource is not required for the EC2 actions and, as a result, the value will be represented via a wildcard.
Full example of what the policy should look like:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid" : "PermissionToCreateEFSFileSystem",
      "Effect": "Allow",
      "Action": [
        "elasticfilesystem:CreateFileSystem",
        "elasticfilesystem:CreateMountTarget"
      ],
      "Resource": "arn:aws:elasticfilesystem:region-id:file-system/*"
    },
    {
     "Sid" : "PermissionsRequiredForEC2",
      "Effect": "Allow",
      "Action": [
        "ec2:DescribeSubnets",
        "ec2:CreateNetworkInterface",
        "ec2:DescribeNetworkInterfaces"
      ],
      "Resource": "*"
    }
  ]
}
```

In addition to the these policies, you'll also need the following permissions to manage EFS using the AWS management console:
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid" : "Stmt1AddtionalEC2PermissionsForConsole",
      "Effect": "Allow",
      "Action": [
        "ec2:DescribeAvailabilityZones",
        "ec2:DescribeSecurityGroups",
        "ec2:DescribeVpcs",
        "ec2:DescribeVpcAttribute"
      ],
      "Resource": "*"
    }
    {
     "Sid" : "Stmt2AdditionalKMSPermissionsForConsole",
      "Effect": "Allow",
      "Action": [
        "kms:ListAliases",
        "kms:DescribeKey"
      ],
      "Resource": "*"
    }
  ]
}
```

The above permissions allow the console to
- View EFS resources
- Query EC2
- Display VPCs, AZs and security groups
- Enable KMS actions if encryption is enabled on the EFS filesystem

### Encryption

EFS supports both encryption at rest and in transit
#### At rest
- Uses the KMS to manage encryption keys
- A KMS master key is required. It can encryption data of upto 4KB in size, but is usually used in relation to data encryption keys: CMK - Customer Master Key which is the key that is used to encrypt the data
- 2 types of KMS
	1. Those created by customers which can either be created using KMS, or by importing key material from existing key management applications into a new CMK
	2. Those that are managed and created by AWS themselves. In the example in the image, the CMK selected is an AWS managed master key. 

#### In transit
- The encryption is enabled through the utilization of the TLS protocol, which is transport layer security, when you perform your mounting of your EFS file system.
- The best way to do this is to use the EFS mount helper as I did  earlier in a previous demonstration. The command used to implement the  use of TLS for in-transit encryption is as follows:
```
sudo mount -t efs  -o tls fs-12345678:/ /mnt/efs
```

- This will ensure that the mount helper creates a client stunnel process using TLS version 1.2. 'Stunnel is an open-source multi-platform application used to provide a universal TLS/SSL tunneling service. Stunnel can be used to provide secure encrypted connections for clients or servers that do not speak TLS or SSL natively.'(Wikipedia) This stunnel process is used to listen out for any traffic, using NFS, which it then redirects to the client stunnel process.


---

## Importing Data

### How to import on-premise data into EFS
- The recommended course of action is to use another service called AWS DataSync.
- This service is specifically designed to help you securely move and migrate and synchronize data for your existing on-premises site into AWS Storage Services such as Amazon EFS or Amazon S3 with simplicity and ease.
- The data transfer can either be accomplished over a direct connect link or over the internet.
- To sync source files from your on-premises environment, you must download the DataSync agent as a VMware ESXi host to your site.
- The agent is configured with the source and destination target and associated with your AWS account, and logically sits in between your on-premise file system and your EFS file system. 
- DataSync is also very useful if you want to transfer files between EFS file systems either within the same AWS account or cross-account and owned by a third-party.

### Use cases

- You can migrate an NFS file system from Amazon EC2 to Amazon EFS within the same AWS region.
- Replace an NFS file system from Amazon EC2 in one AWS region to an Amazon EFS file system in a different AWS region for disaster recovery.
- You can migrate an Amazon EFS file system from EFS standard with no lifecycle management to an EFS file system with lifecycle management enabled. File systems with lifecycle management enabled will automatically move to a lower-cost Infrequent Access storage class based on a predefined lifecycle policy.
- You can migrate an Amazon EFS file system from one performance mode to another performance mode within the same AWS region
- Replicate an Amazon EFS file system from one AWS region to another Amazon EFS file system in a different AWS region for disaster recovery.

---

# Summary


### What is EFS and what it does

- EFS provides simple scalable file storage for use with Amazon EC2 instances. Amazon Elastic File Storage or EFS is considered a file-level storage and is also optimized for load latency access. 
- Amazon Elastic File Storage or EFS is considered a file-level storage and is also optimized for load latency access. 
- EFS supports access by multiple EC2 instances and it can meet the  demands of tens, hundreds, or even thousands of EC2 instances  concurrently.
- Uses standard file-system semantics such as locking files, renaming files, updating them, and using a hierarchical structure.
- EFS provides the ability for users to browse cloud network resources. EC2 instances can be configured to access Amazon EFS instances using configured mount points, and mount points can be created in multiple availability zones. EFS is a fully managed, highly available and durable service.
- And EFS uses standard operating system APIs, so any application that is designed to work with standard operating system APIs, will work with EFS. It supports both NFS versions 4.1 and 4.0 and the EFS file system is also regional. 

### Storage classes and performance options

- Amazon EFS offers two different storage classes, which offer different levels of performance and costs. These being Standard and Infrequent Access, known as IA.
- The standard storage class is the default storage class used, and Infrequent Access is used to store data that is rarely accessed but provides a cost reduction on your storage.
- IA access results in an increased first-spike latency impact when both reading and writing data when compared to that of Standard storage class. 
- IA charges for the amount of space used and for each read and write you make to the storage class, whereas standard storage only charges for the amount of storage space used per month.
- EFS lifecycle management will automatically move data between storage classes based upon file access. If a file has not been read or written to for over 30 days, EFS lifecycle management will move the data to the IA storage class to save on costs. 
- When the file is accessed again, the 30-day timer is reset, and it is moved back to the standard storage class. The EFS lifecycle management will not move data below 128K in size, or any metadata.
- EFS supports two performance modes, General Purpose and Max I/O. General Purpose is a default performance mode and is used for most use cases, offering all-round performance and low-latency file operation.
- General Purpose allows only up to 7,000 file system operations per second, whereas Max I/O offers virtually unlimited amounts of throughput and IOPS. Max I/O file operation latency will be reduced compared to General Purpose.
- EFS provides a CloudWatch metric, percent IO limit, which allows you to view your operations per second as a percentage of the top 7,000 limit. 
- EFS also supports two throughput modes, Bursting Throughput and Provision Throughput.
- Bursting Throughput, which is the default mode, scales as your file system grows. EFS credits are accumulated during periods of low-latency activity, operating below the baseline rate of throughput, set at 50 mebibytes per tebibyte of storage used.
- Every file system can reach its baseline throughput 100% of the time, and using EFS credits allows it to burst above the baseline limit. 
- Credits can be monitored with a CloudWatch metric of BurstCreditBalance.
- Provisioned Throughput allows you to burst above your allocated allowance. However, this option does incur additional charges. 

### Creating an EFS Filesystem

Refer demo video

### Managing EFS Security

- To create your EFS file system, you need to have allow access (refer lecture notes)
- To manage EFS using the AWS management console, you'll also need the following permissions (refer lecture notes)
- EFS supports both encryption at rest and in transit
- Encryption at rest is enabled via a checkbox when using the Management Console
- Encryption at rest uses the Key Management Service, known as KMS, to manage your encryption keys
- Encryption in transit is enabled by utilizing the Transport Layer Security (TLS) protocol when you perform your mounting of your EFS file system
- It is best to use the EFS mount helper to implement encryption in transit
- The mount helper will create a client stunnel process using TLS version 1.2
- The stunnel process listens for any traffic using NDS which it then redirects to the encrypted port

### Import data

- the recommended course of action is to use AWS DataSync to import data.
- AWS DataSync is designed to securely move and migrate and synchronize data from your existing on-premises site into AWS storage services.
- Data transfer can be accomplished over a Direct Connect link, or over the internet.
- To sync files from your on-premises environment, you must download the DataSync agent. You then need to configure the agent with a source and destination target, and DataSync can also transfer files between EFS file systems.

---
