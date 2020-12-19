
# Amazon S3

## Introduction
Course Objectives
- To provide an overview of Amazon S3, including what the service is, the basics of the Amazon S3 console and its associate storage classes

---

## Overview of Amazon S3
- Amazon Simple Storage Service. Most widely and heavily used storage service in AWS simply down to the fact that it can be a great fit for many different use cases, as well as integrating with many different AWS services.
- Fully managed, object-based storage service that is highly available, highly durable, very const-effective, and widely accessible.
- Unlimited* scalability, far more scalable than your own on-premise storage.
- There are limitations on file sizes
	- smallest file size it supports is 0 bytes
	- largest file size is 5 TB
- Object Storage System => does not conform to a hierarchical system like a file system
	- flat address space identified by a unique URL
	- compared to a file system which is quite hierarchical
- Availability of data in S3 is 11 9s
	- Because it maintains multiple copies of data in different availability zones
- To store objects in S3, you first need to define and create a bucket
	- Container for your data
	- Bucket name should be unique across the world.
	- Default: 100 buckets (soft limit)
	- Any object uploading to a bucket is given a unique identifier
	- Can create folders within a bucket if needed for categorization. However, S3 isn't a file system and many features of S3 work at a bucket level and not a folder level
	- The unique id contains the bucket, folder (if any) and the name of the file itself

---
## Storage Classes

S3 allows you to choose storage classes based on performance features and costs and it's down to you to select the storage class that you require for the data. Following storage classes are available:
1. S3 Standard
2. S3 Intelligent Tiering - S3 INT
3. S3 Standard Infrequent Access - S3 S-IA
4. S3 One Zone Infrequent Access - S3 Z-IA
5. S3 Glacier
6. S3 Glacier Deep Archive - S3 G-DA

### S3 Standard
General purpose storage class. It is ideal for a range of use cases where you need high throughput with low latency with the added ability of being able to access your data frequently.
- High throughput
- Low latency
- Frequent access to data
- Durability - 11 9s
- Availability SLA - 99.99%
- SSL to encrypt data in transit and at rest
- Lifecycle rules to automate data storage management
	- Allows you to configure rules that automatically move your data from one type of storage class to another. For e.g., moving data to a cheaper storage class after a set period of time

### S3 INT
- High throughput
- Low latency
- Ideal when the frequency of access is unknown. Effectively, we have unpredictable data access patterns and so by using this storage class, it can help to optimize your storage costs.
	- Depending on your data access patterns of objects in the Intelligent Tiering Class, S3 will automatically move your data between 2 different tiers: frequent access and infrequent access (which are different from the existing storage classes)
- Durability - 11 9s across multiple availability zones offering protection against the loss of a single AZ
- Availability SLA isn't as high as Standard. Set at 99.9%
- SSL to encrypt data in transit and at rest
- Supports lifecycle rules

### S3 S-IA
Can be seen as the infrequent tier of the Intelligent Tiering class
- High throughput
- Low latency
- Infrequent access. So cheaper than S3 Standard
- Durability - 11
- 9s across AZs
- Availability - 99.9%
- SSL encryption in transit and at rest
- Supports lifecycle rules

### S3 Z-IA
- High throughput
- Low latency
- Infrequent access. So cheaper than S3 Standard
- Durability - 11 9s across a single AZ
- Availability - 99.5% - because stored in 1 AZ. Should the AZ storing data become available then you will lose access to your data or even worse it may become completely lost should the AZ be destroyed in a catastrophic event
- SSL encryption in transit and at rest
- Supports lifecycle rules

### S3 Glacier
- Used for archival data
- Can be accessed separately from Amazon S3 service but closely interacts with it
- Comes at a fraction of the cost for storing same amount of data than the S3 storage classes
- Doesn't provide same features as S3 but more importantly doesn't provide instant access to the data - can take up to several hours
- Low cost durable storage a.k.a. cold storage ideally suited for long-term backup and archival requirements
- Capable of storing same data types as S3
- Durability 11 9s - multiple AZs within a single region
- Capacity - effectively unlimited
- No GUI
- Dashboard allows you to create vaults, set data retrieval policies and event notifications
- 1. Create vault as container for archive
- 2. Move your data into the Glacier vault using APIs/SDK
- Or, can use the lifecycle rule to move it to Glacier
- Low throughput
- High latency
- Access to data at a cost through expedited, standard and bulk options
	- Expedited: Under 250MB, available in 5 minutes, most expensive
	- Standard: Any size, 3-5 hours, second most expensive
	- Bulk: PB of data 5-12 hours, cheapest
- 11 9s
- SLA - 99.99%
- SSL - transit and at rest
- Supports lifecycle rules and S3 PUT APIs

### S3 G-DA
- Cheapest, focuses on long-term storage
- Ideal for data that's only stored for data regulation and compliance reasons with minimal retrieval within 12 hours
- 11 9s
- Availability - 99.99%
- SSL - in transit + at rest
- Can only use S3 PUT APIs

Main diff is durability and availability

Ask the following questions:
1. How critical is my data?
2. Does it require the highest level of durability?
3. How reproducible is the data?
4. Can it be easily created again if need be?
5. How often is the data likely to be accessed?
---
