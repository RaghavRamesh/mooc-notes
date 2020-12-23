- [Home](https://raghavramesh.github.io/mooc-notes/)
- [Amazon Elastic Block Store (EBS)](https://raghavramesh.github.io/mooc-notes/DevOps/AWS/Storage/AmazonEBS.html)
- [Amazon EC2 Instance Storage](https://raghavramesh.github.io/mooc-notes/DevOps/AWS/Storage/AmazonEC2InstanceStorage.html)
- [Amazon S3](https://raghavramesh.github.io/mooc-notes/DevOps/AWS/Storage/AmazonS3.html)
- [Amazon S3 Bucket Properties and Management Features](https://raghavramesh.github.io/mooc-notes/DevOps/AWS/Storage/AmazonS3BucketPropertiesAndManagementFeatures.html)


# AWS Storage Services
AWS facilitates movement from DCs to Cloud because of
- security
- flexibility
- cost efficiency
- scalability
which means it needs to provide
- Compute
- Storage
- Database
- Network
and AWS does this very well. This course focuses on Storage

Why so many storage services?
For the same reason why there are so many on-premise storage options:
- Storage Areas Network (SAN)
- Network Attached Storage (NAS)
- Directly Attached Storage (DAS)
- Tape Backup

which is that different benefits and features are provided by different options:
- Cost variants
- Storage capacity
- Security features
- Varied levels of durability
- Varied levels of Availability
- Read/write speed
- Accessibility
- Media Type
- Auditable and traceable
- backup and file storage

Data storage categorization
1. Block
	1. Data is stored in chunks known as blocks
	2. Blocks are stored on a volume and attached to a single instance
	3. Provide very low latency data access
	4. Comparable to DAS storage used on premises
2. File
	1. Data is stored as separate files with a series of directories forming a data structure hierarchy
	2. Data is stored within a file system
	3. Shared access is provided for multiple users
	4. Comparable to NAS storage used on premises
3. Object
	1. Each object does not conform to a data structure hierarchy. Objects are stored across a flat address space
	2. Objects are referenced by a unique key
	3. Each object can also have associated metadata to help categorize and identify the object.