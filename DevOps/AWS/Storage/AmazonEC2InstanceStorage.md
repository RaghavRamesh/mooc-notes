# Amazon EC2 Instance Storage

### Instance Store Volumes
- The volumes physically reside on the same host as the EC2 instance itself
- Ephemeral storage for EC2 instances
- Not recommended for critical or valuable data
- If your instance is stopped or terminated your data is lost
- If your instance is rebooted, your data will remain in tact

### Benefits
- No additional cost for storage, it's included in the price of the instance
- Offer a very high I/O speed
- I3 instance Family:
	- 3.3 million random read IOPS
	- 1.4 million write IOPS
	- Speeds like this makes this an ideal candidate for high-demand NoSQL databases
	- But for persistence will have to copied/replicated to a persistent store
- Great for cache or buffer for rapidly changing data without the need for retention
- Often used within a load balancing group, where data is replicated and pooled between the fleet

- Instance store volumes are not available for all instances
- The capacity of instance store volumes increases with the size of the EC2 instance
- Instance store volumes have the same security mechanisms provided by EC2

### Anti-patterns
- Instance store volumes should not be used for:
	- Data that needs to remain persistent
	- Data that needs to be accessed and shared by multiple entities

---