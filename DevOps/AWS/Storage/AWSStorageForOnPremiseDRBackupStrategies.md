# Using AWS for On-Premises Backup & Disaster Recovery

## Introduction

With an on-premises data backup solution within your data center, it’s critical for your business to have a disaster recovery plan built into your business continuity plans. You need to have a plan in place should a disaster occur that affects your operation of the business. The same is true when you start to leverage the cloud for its storage capabilities for your backed up data.

This course explains how cloud storage fits in with DR and the different considerations when preparing to design a solution to back up your on-premises data to AWS. It will explain how Amazon S3, AWS Snowball, and AWS Storage Gateway can all be used to help with the transfer and storage of your backup data.

This course where I shall be focusing on how some of the AWS storage services can help you with disaster recovery and data backup of your on-premise production resources resulting in an effective business continuity plan by preventing data loss, whilst at the same time reducing your RTO and RPO.

### Course agenda

- Where does Cloud Storage fit in with disaster recovery?
- Considerations when planning a DR storage solution
- Using Amazon S3 as a data backup solution
- Using AWS Snowball for data transfer
- Using AWS Storage Gateway for data backup
- And then finally, a course summary

---


## Disaster Recovery

### Where does Cloud Storage fit in DR?


The sooner your systems are operational and readily available to  resume services, the better for you as a business and your customers.

Now herein lies the problem with traditional data backup solutions.  The data you need might not be available to you:

- Your backup data might be stored in the same location as your production data, and so the disaster, such as a fire or flood or worse, would have impacted your backup data too.
- If using a tape backup method, then the tapes could be faulty, due to  overuse, and the data become unreadable, or even lost in transit.
- There may also be manual activity involved, from tape rotation to  tape labelling, which may result in specific data not being backed up or  not being able to find the right data.
- There are also many other reasons of why the traditional data backup method can be ineffective, such as scalability.

#### Issues with traditional Backup methods

- Scalability: As your infrastructure expands, so will your NAS or SAN and tape libraries. This can be very costly to maintain and implement.
- Costs: implementing an effective backup solution can be a huge up-front capital expenditure cost
- Data Availability: if your data is stored off site, the retrieval of your data is impacted

It's no secret that using cloud storage services can be considerably cheaper as a backup solution than that of your own on-premise solution. But cost aside, the speed in which you can launch an environment within AWS to replicate your on-premise solution, with easy access to production backup data, is of significant value to many organizations.

#### Benefits of Cloud Storage

- Offsite backup
- No scalability constraints
- No CAPEX costs
- High durability and availability
- Enhanced security features

#### Example

Now let's imagine this production environment, in your local data  center, experienced an outage. You could perform the following steps to  quickly recover from the disaster.
You could launch a new environment with new instances, based on your  own custom AMIs, complete with custom applications within a VPC. Then  you could create EBS storage volumes, based on your backed up data, from  within AWS, and attach these volumes to those instances. You now have  your application servers up and running, with the latest production  data. Couple this with some minor networking components, and you could  then communicate with other sites you may have running.


---


### Considerations when planning an AWS DR Storage Solution


#### Balance

There is a fine line between how you architect your data storage needs, which must be fit for a purpose or the data it holds, but it may also have to conform to specific governance and compliance regulations for DR.

So determining which solution or service to use to store the data and which solution to use to ensure you can recover your data effectively in the event of a disaster is a balance.

#### RTO and RPO

From a DR perspective, this is largely down to the particular RTO and RPO for the environment you are designing.

- As a quick refresher, Recovery Time Objective, RTO, is defined as the  maximum amount of time in which a service can remain unavailable before it's classed as damaging to the business.
- Recovery Point Objective, RPO, is defined as the maximum amount of time for which a data could be lost for a service.

Depending on the values of these, it can help you select the most  appropriate storage method. For example, if your RTO was an hour, then  restoring data from Amazon Glacier may not be effective, as it can take a  number of hours to process, depending on your retrieval method.

#### How will you get data in/out of AWS?

Direct Connect
If you have a Direct Connect connection to AWS, then you can use this to move data in and out of the environment, which can support connectivity of up to 10 gigabits per second.

VPN Connection
If you don't have a direct connection link between your data center and AWS then you may have a hardware or software VPN connection which could also be used.

Internet Connection
Now if you don't have either of these as connectivity options then you can use your own internet connection from the data center to connect and transfer the data to AWS. Depending on how much data you need to move or copy to AWS, then these lines of connectivity may not have the required bandwidth to cope with the amount of data transferred.

#### Large Data Transfer

##### AWS Snowball
- Physical appliance sent to customer site
- 50TB or 80TB in size
- Use multiple snowball appliances to scale to Petabyte scale

For extreme situations, AWS offers an even larger solution
##### Snowmobile
- Exabyte-scale transfer service
- Transfer up to 100PB per snowmobile (45-foot shipping container) pulled by a semi-trailer truck

##### Storage Gateway
Acts as a gateway between your on-premise storage and the AWS environment

#### How quickly do you need your data back?

- Dependent upon your RTO requirements
- How critical is the data to your business?
- Different storage services offer different accessibility options
- Your network infrastructure to AWS will impact retrieval rates

#### How much data do you need to import and export?

- The amount of data you have to transfer can affect your storage solution
- Calculate your target transfer rate: https://thecloudcalculator.com/calculators/file-transfer/
- Check limitations of storage services used, minimums and maximums, this varies between services, such as S3, Storage Gateway and Snowball

#### Durability

When looking at the durability of a data backup, you'll need to  ascertain the criticality of that data to ensure it offers the most  suitable resiliency and redundancy. For example, if I look at the Amazon  S3 service it has the following classes available:
- $$$: The Standard Class, which provides 11 nines of durability and 4 nines of availability.
- $$: The Infrequent Access Class, known as IA, provides 11 nines of  durability, but only 3 nines of availability, and this is often used as a  backup store over the Standard Class.
- $: And Amazon Glacier. This also provides 11 nines of durability, and is  used as a cold storage solution. This offers the cheapest storage cost  of the three, but does not allow immediate access to the files.

#### Security

A key focus for any data you store in the Cloud is security. Ensuring  that your data has the right level of security safeguarding it from  unauthorized access is fundamental, especially if it contains sensitive  information such as customer data.
You may need to abide by specific governance and compliance controls  and so you need to ensure that where you store your data in the Cloud is  able to offer the correct functionality to ensure your data remains  compliant.
When working with sensitive information, you must ensure that you  have a means of encryption both in-transit and when at rest. You should  understand how your selected storage method operates and manages data  encryption if this level of security is required.
A sound understanding of Cloud storage access security is a must for  your support engineers, who will be maintaining the environment.
If security is not configured and implemented correctly at this  stage, it could have devastating and damaging effects to you as a  business should the data be compromised and exposed in any way, which  has already happened to many organizations who failed to understand the  implications of their security controls.

#### Compliance

Compliance comes into play specifically when looking at security of  your data. There are a number of different certifications, attestations,  regulation, laws, and frameworks that you may need to remain compliant  against.
To check how AWS storage services stack up against this governance,  AWS has released a service called AWS Artifact, which allows customers  to view and access AWS Compliance Reports. These are freely available to  issue to your own auditors, to help you meet your controls.
The service itself is accessed by the AWS Management Console, and all  of the reports available are issued by external auditors to AWS  themselves, and within each report it will contain the scope indicating  which services and region the report is associated with.

---

## AWS Storage Services

### Using Amazon S3 as a Data Backup Solution

As you probably know, Amazon S3 is a highly available and durable service, with huge capacity for scaling. It can store files from 1byte in size, up to 5TBs, with numerous security features to maintain a tightly secure environment.

This makes S3 an ideal storage solution for static content, which makes Amazon S3 perfect as a backup solution.

Amazon S3 provides three different classes of storage, each designed to provide a different level of service and benefit.

#### S3: Standard

- Durability: 99.999999999%
- Availability: 99.99%

#### S3: Standard Infrequent Access

- Durability: 99.999999999%
- Availability: 99.9%
- Used for data that is accessed less frequently than data held in Standard Class
- Commonly used for backups as you still have immediate retrieval access when needed
- Uses the same SLA as the Amazon S3 Standard class
- Uses the same security and data management capabilities as Amazon S3
- Primary difference is the cost

#### S3: Amazon Glacier

- Data is stored in Archives
- Archives can be up to 40Tb in size
- Archives are stored within Vaults
- Different security measures to that of Amazon S3
- This class is essentially used for data archiving and long-term data retention, and is commonly referred to as the cold storage service within AWS.
- To move data into Glacier:
	1. Use lifecycle rules from within Amazon S3 to move data from Standard and IA Class to Amazon Glacier
	2. AWS Software Development Kits (SDKs)
	3. Amazon Glacier API
- Durability: 99.999999999%
- Encryption in transit and at rest
- Own security measures: Vault Lock security enabling WORM control and vault access policies
- Amazon Glacier can also be used to help comply with specific governance controls, such as, HIPPA and PCI, within an overall solution

#### Amazon Glacier: Data Retrieval

- The most significant difference between Glacier and the other two classes, is that you do not have immediate data access when you need it. If you need to retrieve data from Amazon Glacier, than depending on your retrieval option, it can take anything between a number of minutes to a number of hours for that data to be made available for you to than explore.
- This can be a problem if you're on a DR situation whereby you need immediate access to specific data, as is possible within S3. As a result, you need to be aware of the data that you are archiving to Amazon Glacier, specifically if you have low RTO; the are a number of retrieval options available, each with different cost and time for retrieving your data.
	- Expedited
		- Used for urgent access of a subset of an archive
		- Less than 250MB
		- Data available within 1-5 mins
		- Expensive
	- Standard
		- Used to retrieve any of your archives regardless of size
		- Data available within 3-5 hours
		- Cheaper
	- Bulk
		- The cheapest option for data retrieval
		- Used to retrieve petabytes of data
		- Data available within 5-12 hours

### Costs



Among Standard, Standard - IA and Glacier though, Glacier is the cheapest.

##### S3: Cross Region Replication (CRR)

Depending on specific compliance controls, there may be a requirement to store backup data across a specific distance from the source. By enabling Cross Region Replication, you can maintain compliance whilst at the same time still have the data in the local region for optimum data retrieval latency.

Enable it for the following reasons:
- Reduce Latency of Data Retrieval
- Governance and Compliance
	- Depending on specific compliance controls, there may be a requirement to store backup data across a specific distance from the source. By enabling Cross Region Replication, you can maintain compliance whilst at the same time still have the data in the local region for optimum data retrieval latency.
	- As a result of Cross Region Replication, you would have eleven nines of durability, and four nines availability for both the source and the replicated data.

##### S3: Performance

From a performance perspective, S3 is able to handle multiple concurrent, and as a result, Amazon recommends that for any file that you're trying to upload to S3, larger than 100MB, than you should implement multipart upload. This feature helps to increase the performance of the backup process.

There are a number of benefits to multipart upload. These being,
- Speed and Throughput: As mutliple parts can be uploaded at the same time in parallel, the speed and throughput of uploading can be enhanced.
- Interruption Recovery: Should there be any transmission issues or  errors, it will only affect the part being uploaded, unaffecting the  other parts. When this happens, only the affected part will need to be  resent.
- Management: There is an element of management available whereby you  can, if you're quiet, pause your uploads and then resume them at any  point. Multipart uploads do not have an expiry time, allowing you to  manage the upload of your data over a period of time.


#### Security

Getting your data into AWS and on to S3 is one thing, but ensuring that it can't be accessed or exposed to unauthorized personnel is another. More and more I hear on news feeds where data has been exposed or leaked out into the public, due to incorrect security configurations made on S3 buckets, inadvertently exposing what is often very sensitive information to the general public.

Some of these security features, which can help you maintain a level of data protection are:
- IAM Policies: These are identity and access management policies  that can be used to both allow and restrict access to S3 buckets and  objects at a very granular level depending on identities permissions.
- Bucket Policies: This are JSON policies assigned to individual  buckets, whereas IAM Policies are permissions relating to an identity, a  user group, or role. These Bucket Policies can also define who or what  has access to that bucket's contents.
- Access Control Lists: These allow you to control which user or AWS  account can access a Bucket or object, using a range of permissions, such as read, write, or full control, et cetera.
- Lifecycle Policies: Lifecycle Policies allow you to automatically manage and move data between classes, allowing specific data to be relocated based on compliance and governance controls you might have in place.
- MFA Delete: Multi-Factor Authentication Delete ensures that a user has to enter a 6 digit MFA code to delete an object, which prevents accidental deletion due to human error.
- Versioning: Enabling versioning on an S3 bucket, ensures you can recover from misuse of an object or accidental deletion, and revert back to an older version of the same data object. The consideration with versioning is that it will require additional space as a separate object is created for each version, so that's something to bear in mind.


---

### Using AWS Snowball for Data Transfer

- Used to securely transfer large amounts of data in and out of AWS (Petabyte scale)
- Either from your on-premise DC to Amazon S3, or from S3 to your DC using a physical appliance, known as a snowball
- 50TB or an 80TB storage device depending on your region
- The snowball appliance is dust, water and tamper resistant
- Built for high speed data transfer
	- RJ45 (Cat6)
	- SFP+ Copper
	- SFP+ Optical

#### Encryption & Tracking

- By default, all data transferred to the snowball appliance is automatically encrypted using 256-bit encryption keys generated from KMS, the key management service.
- Whilst on the topic of security, it also features end to end tracking using an E ink shipping label. This ensures that when the device leaves your premises, it is sent to the right AWS facility.
- The appliance can also be tracked using the AWS Simple Notification Service with text messages or via the AWS Management Console

#### Compliance

- From a compliance perspective, AWS Snowball is also HIPAA compliant allowing you to transfer protected health information in and out of S3.
- When the transfer of data is complete via into S3 or into a customer's data center and the appliance is sent back to AWS. It is then the responsibility of AWS to ensure the data held in the Snowball Appliance is deleted and removed.
- To control this process, AWS conforms to standards and guidelines set by NIST, the National Institute of Standard and Technology, to ensure this is performed and controlled and that all traces of data are removed from the media.

#### Data Aggregation


When sending or retrieving data, Snowball appliances can be aggregated together. For example, if you need to retrieve 400 terabytes of data from S3, then your data will be sent by five 80 terabyte Snowball appliances.

So from a disaster recovery perspective, when might you need to use AWS Snowball? Well, it all depends on how much data you need to get back from S3 to your own corporate data center and how quickly you can do that. On the other hand, how much data do you need to get into S3?

This'll depend on the connection you have to AWS from your data center. You may have direct connect connections, a VPN, or just an internet connection. And if you need to restore multiple petabytes of data, this could take weeks or even months to complete.

As a general rule, if your data retrieval will take longer than a week using your existing connection method, then you should consider using AWS Snowball.

#### AWS Snowball Process

1. Create an export job
2. Receive delivery of your appliance
3. Connect the appliance to your network
4. Connect the appliance to your network when appliance is off; Power on and configure network settings
5. You are now ready to transfer the data
6. Access the required credentials; Install the Snowball Client; Transfer the data using the client; Disconnect appliance when the data transfer is complete
7. Return snowball appliance to AWS

---

### Using AWS Storage Gateway for on-premise data backup

Storage Gateway allows you to provide a gateway between your own data center's storage systems such as your SAN, NAS or DAS and Amazon S3 and Glacier on AWS.



- The software appliance can be downloaded from AWS as a virtual machine
- 3 different configs
	- File Gateway
	- Volume Gateway
	- Tape Gateway

#### File Gateways

- File gateways allow you to securely store your files as objects within S3.
- Using as a type of file share which allows you mount on map drives to an S3 bucket as if the share was held locally on your own corporate network.
- When storing files using the file gateway they sent to S3 over HTTPS and are also encrypted with S3's own server side encryption SSE-S3.
- In addition to this local a on-premise cache is also provisioned for accessing your most recently accessed files to optimize latency with also helps to reduce egress traffic costs.
- When your file gateway's first configured you must associate it with your S3 bucket which the gateway will then present as a NFS V.3 or V4.1 file system to your internal applications.
- This allows you to view the bucket as a normal NFS file system, making it easy to mount as a drive on Linux or map a drive to it in Microsoft. Any files that are then written to these NFS file systems are stored in S3 as individual objects as a one to one mapping of files to objects

#### Volume Gateways

- These can be figured in one of two different ways, Stored volume gateways and cached volume gateways.
- Stored
	- Stored volume gateways are often used as a way to backup your local storage volumes to Amazon S3 whilst ensuring your entire data library also remains locally on-premise for very low latency data access.
	- Volumes created and configured within the storage gateway are backed by Amazon S3 and are mounted as iSCSI devices that your applications can then communicate with.
	- During the volume creation, these are mapped to your on-premise storage devices which can either hold existing data or be a new disk. As data is written to these iSCSI devices the data is actually written to your local storage services such as your own NAS, SAN or DAS storage solution. However the storage gateway then asynchronously copies this data to Amazon S3 as EBS snapshots.
	- Having your entire dataset remain locally ensures you have the lowest latency possible to access your data which may be required for specific applications or security compliance and governance controls whilst at the same time providing a backup solution which is governed by the same controls and security that S3 offers.
	- Volumes created can be between 1GiB and 16TB and for each storage gateway up to 32 stored volumes can be created which can give you a maximum total of 512TB of storage per gateway. Storage volume gateways also provide a buffer which uses your existing storage disks. This buffer is used as a staging point for data that is waiting to be written to S3.
	- During the upload process the data is sent over an encrypted SSL channel and stored in an encrypted format within S3. To add to the management and backup of your data storage gateway makes it easy to take snapshots of your storage volumes at any point, which are then stored as EBS snapshots on S3. It's worth pointing out that these snapshots are incremental ensuring that only the data that's changed since the last backup is copied helping to minimize storage costs on S3.
- Cached
	- Cached volume gateways are differed to stored volume gateways in that the primary data storage is actually Amazon S3 rather than your own local storage solution.
	- However cache volume gateways do utilize your local data storage as a buffer and the cache for recently accessed data to help maintain low latency, hence the name, Cache Volumes.
	- Again, during the creation of these volumes they are presented as iSCSI volumes which can be mounted by an application service. The volumes themselves are backed by the Amazon S3 infrastructure as opposed to your local disks as seen in the stored volume gateway deployment. As a part of this volume creation you must also select some local disks on-premise to act as your local cache and a buffer for data waiting to be uploaded to S3.
	- Again, this buffer is used as a staging point for data that is waiting to be written to S3 and during the upload process, the data is encrypted using an SSL channel where the data is then encrypted within SSE S3. The limitations is slightly different with cache volume gateways in that each volume created can be up to 32TB in size with support for up to 32 volumes meaning a total storage capacity of 1024TB per cache volume gateway.
	- Although all of your primary data used by applications is stored in S3 across volumes, it is still possible to take incremental backups of these volumes as EBS snapshots. In a DR scenario, and as I mentioned in the previous section, this then enables quick deployment of the datasets which can be attached to EC2 instances as EBS volumes containing all of your data as required.

#### Tape Gateway - Gateway VTL - Virtual Tape Library

This allows you again to back up your data to S3 from your own corporate data center but also leverage Amazon Glacier for data archiving. Virtual Tape Library is essentially a cloud based tape backup solution replacing physical components with virtual ones.

VTL Components

- Storage Gateway. The gateway itself is configured as a tape gateway which as a capacity to hold 1500 virtual tapes.
- Virtual Tapes. These are a virtual equivalent to a physical backup tape cartridge which can be anything from 100 GB to two and a half terabytes in size. And any data stored on the virtual tapes are backed by AWS S3 and appear in the virtual tape library.
- Virtual Tape Library. VTL. As you may have guessed these are virtual equivalents to a tape library that contain your virtual tapes.
- Tape Drives. Every VTL comes with ten virtual tape drives which are presented to your backup applications is iSCSI devices.
- Media Changer. This is a virtual device that manages tapes to and from the tape drive to your VTL and again it's presented as an iSCSI device to your backup applications.
- Archive. This is equivalent to an off-site tape backup storage facility where you can archive tapes from your virtual tape library to Amazon Glacier which as we already know, is used as a cold storage solution. If retrieval of the tapes are required, storage gateway uses the standard retrieval option which can take between 3 - 5 hours to retrieve your data.

- Once your storage gateway has been configured as a tape gateway, your application and backup software can mount the tape drives along with the media changer is iSCSI devices to make the connection.
- You can then create the required virtual tapes as you need them for backup, and your backup software can use these to back up the required data which is stored on S3.

---
