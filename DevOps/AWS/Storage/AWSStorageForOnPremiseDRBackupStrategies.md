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



---

### Using AWS Storage Gateway for on-premise data backup



---

## Summary


