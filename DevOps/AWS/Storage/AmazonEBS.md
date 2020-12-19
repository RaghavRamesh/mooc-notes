# Amazon Elastic Block Store (EBS)

- Provides Block level storage to EC2 instances
- (however, it) offers persistent and durable data storage (which is a huge plus)
- Greater flexibility than that of instance store volumes
- EBS volumes can be attached to EC2 instances for rapidly changing data
- Used to retain valuable data due to its persistent qualities and as such can be used to store PII
- Operate as a separate service to EC2
- EBS volumes act as network attached storage devices
- Each volume can only be attached to one EC2 instance
- Multiple EBS volumes can be attached to a single EC2 instance
- Data is retained if the EC2 instance is stopped, restarted or terminated

### EBS Snapshots
- Has the ability to take point-in-time snapshots of EBS Volumes as and when needed. Manual or automatic.
- Snapshots are stored in S3 and are available from all availability zones
- Incremental snapshots
- A new EBS volume can be (re)created from the snapshots and then attach that volume to the EC2 instance
- Can copy snapshot from one region to another
- Highly available - every write to an EBS volume is replicated multiple times within the same AZ of the region to help prevent complete loss of data.

### EBS Volume Types
Allows you to optimise your volumes on a cost-to-performance perspective
1. SSD-backed storage - Solid State Drive
	- Suited for work with smaller blocks such as
	- Databases using transactional workloads
	- Often used for boot volumes on EC2 instances
	- Types
		- General Purpose SSD (GP2)
		- Provisioned IOS (IO1)
2. HDD-backed storage - Hard Disk Drive
	- Designed for workloads requiring a high rate of throughput (MB/s) such as
	- Big Data processing and logging information
	- Larger blocks of data
	- Types
		- Cold HDD (SC1)
		- Throughput Optimized HDD (ST1)

#### General Purpose SSD (GP2)
- Provides single digit ms latency
- Can burst up to 3000 IOPS

#### Provisioned IOPS (IO1)
- Delivers predictable performance for I/O intensive workloads
- Max IOPS possible is set to 20000

#### Cold HDD (SC1)
- Offers the lowest cost compared to other volume types
- Designed for large workloads accessed infrequently
- High throughput capabilities (MB/s)
- Can't use as boot volumes for EC2 instances

#### Throughput Optimized HDD (ST1)
- Designed for frequently accessed data
- Suited to work with large data sets requiring throughput-intensive workloads
- Can't use as boot volumes for EC2 instances

### Encryption
- EBS offers encryption at rest and in transit
- Encryption is managed by EBS service itself
- Does so by integrating with AWS KMS
	- KMS uses customer master keys (CMK) to create data encryption keys (DEK) enabling encryption of data for AWS services such as EBS
- Any snapshot taken from an encrypted volume will also be encrypted
- Encryption is only available for certain instance types

### Creating a new EBS Volume
2 ways to create from within the management console
1. During the creation of a new EC2 instance
2. As a stand alone EBS Volume
Must specify which AZ that volume will exist in
EBS volumes can only be attached to EC2 instances that exist within the same AZ

### Changing the size of an EBS Volume
- EBS volumes are elastically scalable
- Increase the volume size via the AWS Management Console or AWS CLI
- After the increase you must extend the filesystem on the EC2 instance to utilize the additional storage

### Pricing
- Unlike S3 where you're charged based on how much space you are using, for EBS you're charged on a monthly basis for the volume you're provisioned

### Anti-patterns
EBS is not well suited fro all storage requirements like
- Temporary storage
- Multi-instance storage access (EBS can only be attached to one EC2 instance at a time)
- Very hight durability and availability (if you need this, you'll be better suited if you used S3 or EFS)

---