## Introduction

### Course Objectives
Introduce and explain the available properties that are configurable at the bucket level that Amazon S3 has to offer to help you manage and administer your data effectively.

### Standard

- Versioning
- Server access logging
- Static website hosting
- Object-level logging
- Default encryption

### Advanced

- Object lock
- Tags
- Transfer acceleration
- Events
- Requester pays

---

## Versioning

This is a bucket feature that allows for multiple versions of the same object to exist. This is useful to allow you to retrieve previous versions of a file, or recover a file should it subjected to accidental deletion, or intended malicious deletion of an object.

### Enabling Versioning

Not enabled by default. But once enabled, cannot disable. You can suspend, which will prevent any further versions of the object from being created.

### Versioning States

Which means the bucket can be in any one of these versioning states:
1. Unversioned (Default)
2. Versioning-enabled (will incur additional costs as you will be storing multiple versions of the same object)
3. Versioning-suspended

Versioning can be enabled via AWS management console either during the creation of a bucket or enable on an existing bucket

Objects with versioning enabled are not overwritten, instead a new object with a new version id is created.

Note: costs will increase rapidly if you have 1000s of rapidly changing files

### Deleting objects

- Let’s assume we have our same s3-versioning.docx file and I chose to delete it from within the Console. Assuming I was ‘hiding’ the versions
- in the bucket, so I’m simply showing current versions only, the object
- would be deleted and not visible in the bucket. However, the object has
- not been deleted. If I clicked on ‘Show’ versions, then I would see the following.
Here we can see we have our existing 3 versions, all with the same Version IDs as expected, however, we have a 4th version at the top identified with a Delete Marker in brackets.
So what’s happened here? Well basically, this delete marker version becomes the current version of the object and ensures that any GET request to access the object will return a ‘404 not found value’. However, ALL other versions still exist, including the same version of the file that was submitted for deletion, so you can still view and open these files from the console when you have all versions shown, or by specifying the VersionID in a GET API call to call a specific version.
If you wanted to permanently delete an object in a versioned bucket then you must use the DELETE Object versionId in an AWS SDK, specifying the exact version that you want to delete. If you delete the version with the delete marker, then the object would reappear in your bucket using the latest version of the object available.

---

## Server-Access Logging

In a nutshell, when server-access logging is enabled on a bucket, it captures details of requests made to the bucket and its objects. Logging is important when it comes to security, root-cause cause analysis following incidents, and it can also be required to conform to specific audit and governance certifications.

However, server-access logging is not guaranteed and is conducted on a Best-Effort basis by S3. The logs are collated and sent every few hours but there's no guarantee that every request will be captured and that you will receive a log for a specific request within a set time frame.

### Enabling the feature

By default SAL is disabled. Enabling access logging on your buckets is a very simple process using the S3 Management Console: specify target bucket (used to store any logs created by enabling SAL on your source bucket, which must be in the same region) and target prefix (optional). Can be enabled both during bucket creation and after it's created.

### Permissions

To allow S3 to write access logs to a target bucket, it will, of course, require specific permissions. These permissions will require write access for a group known as the Log Delivery group, which is a pre-defined Amazon S3 group used to deliver log files to your target buckets.

- Source and target buckets should be in the same region
	- Different buckets are used for each
- Permissions of the S3 Access Log Group can only be assigned via Access Control Lists
	- Manual setting permissions for this via an SDK = Must update the ACL
- If encryption is enabled, access logs will only be delivered if this is set to SSE-S3 as encryption with KMS is not supported

Logs

Naming convention: <target-prefix><YYYY-MM-DD-HH-MM-SS-UniqueString/>
Log entry:
- bucket owner - canonical user id of the owner of the source bucket
- bucket name - bucket related to the request
- timestamp - in UTC
- remote IP address - of the identity carrying out the request
- requestor - IAM identity or "-" for unauthenticated user
- more...

---
##Static Website Hosting
How to configure S3 bucket to host a static website

### Enabling the feature

By default it is disabled. 3 options while enabling the feature:
1. Use this bucket to host a website
	1. add index doc (home page)
	2. error doc
	3. redirection rules - xml rules for advanced redirection
2. Redirect requests
	1. redirect requests to the s3 bucket endpoint to a specified website
	2. protocol (http, https)
3. Disable website hosting

Additionally it provides a region-specific endpoint which allows users to access your website via that URL. However,
- it doesn't support HTTPS requests
- the bucket and its contents must be marked as publicly accessible
- it does not support requester pays (more on RP later)

### Permissions

By default, blocked to public. To allow public access, enable it in Permissions tab and then add a bucket policy


---

## Object-Level Logging

This feature is actually more closely related to the AWS CloudTrail service than S3 in a way, as it’s AWS CloudTrail that performs logging activities against Amazon S3 data events. These data events are specific API calls used in S3, such as GetObject, DeleteObject, and PutObject.

So what is CloudTrail? CloudTrail is a service that has a primary function to record and track all AWS API requests made. These API calls can be programmatic requests initiated from a user using an SDK, the AWS command-line interface, from within the AWS management console or even from a request made by another AWS service.

When an API request is initiated, AWS CloudTrail captures the request as an event and records this event within a log file which is then stored on S3. Each API call represents a new event within the log file. CloudTrail also records and associates other identifying metadata with all the events. For example, the identity of the caller, the timestamp of when the request was initiated and the source IP address.

Capturing S3 data events can be configured in 2 ways: Firstly, if you want to capture data events for all or some of your S3 buckets, then you can configure this from within one of your Trails using the AWS CloudTrail console itself as shown here. Secondly, if it’s not already enabled via AWS CloudTrail for your bucket you can configure it at the bucket level using the Properties tab. Selecting the Object-level logging tile will present you with options to configure it.

---

## Default Encryption

Whenever you have sensitive data being stored in S3 it’s imperative to have some level of encryption enabled as an additional layer of security to protect your data.

Using default encryption, you are able to set a default encryption mechanism for every new object that is uploaded to the bucket. However, please note that for any objects that are already in your bucket prior to enabling default encryption, they will NOT be encrypted.

### Enabling the feature

To enable default encryption on a particular bucket you can select the ‘Default Encryption’ tile from the bucket properties tab. This will enable you to configure one of 2 different default encryption options.
1. AES-256, also known as SSE-S3 which stands for Server-side encryption using S3 managed keys, and
2. AWS-KMS, this is often referred to as SSE-KMS which stands for Server-side encryption using KMS managed keys. KMS is the Key Management Service. If you are unfamiliar with this service you can learn more about it in this course here.

Check video for how encryption works with both these methods

### Other encryption available to use within S3
- Server-side encryption using customer-managed keys (SSE-C)
- Client-side encryption using KMS managed keys (CSE-KMS)
- Client-side encryption using customer-managed keys (CSE-C)

---

## Object Lock

- Considered an 'advanced' property
- Used to meet a level of compliance known as 'WORM' - Write Once Read Many
- Allows a level of protection against your objects in your bucket and prevents them from being deleted either using a fixed retention period or until the end of time!
- Ability to add retention periods using Object Lock help S3 to comply with regulations such as FINRA, the Financial Industry Regulatory Authority

### Enabling the feature

- Can only be enabled at object creation time.
- Can't apply it on an existing bucket.
- First need to enable versioning - not possible to enable Object lock otherwise
- Once enabled, it's permanently enabled, cannot be disabled

### Retention modes

- Enabling the feature is not sufficient to protect your objects. You will be presenting with 2 retention modes which set the default retention on the bucket
	- Governance mode
	- Compliance mode

### Governance Mode

- By enabling Governance Mode it prevents your users from performing a delete or an overwrite of any of the versions of your objects in the bucket throughout the duration set by the retention period.
- However, if you have very specific permissions, including s3:BypassGovernanceMode, s3:GetObjectLockConfiguration, s3:GetObjectRetention, then a user will still be able to delete an object version within the retention period or change any retention settings set on the bucket.
- Will be asked to define retention period in days. A timestamp metadata is added reflecting the retention period. Once the retention period is over, the object can then be deleted.

### Compliance Mode

- Key difference between Gov and this mode is that there are no users that can override the retention periods set or delete an object and that also includes your AWS root account which has the privileges.
- Essentially, any object added to a bucket configured for Compliance Mode means that the object will remain for the duration of the retention period

### Per-object Retention

- You can also set object lock on a per-object basis if you don't want to set a default retention mode of Governance or Compliance.

### Legal Hold
- The legal hold element only appears for object versions and not at the bucket level and acts much like a retention period and prevents the object from being deleted, however, legal holds do not have an expiration date.
- Therefore, the object will remain protected until a user with permissions of s3:PutObjectLegalHold disables the legal hold on the object.
- If an object is already protected by a retention period, a legal hold can also be placed on the object.
- When the retention period expires, the object will still be protected by the legal hold regardless of the fact that the retention period has expired.

---

## Tags
- Bucket tags or cost allocation tags. Similar to labels in k8s, they are key-value pairs that can be used for categorization.
- Tags are available for all services in AWS and are useful for filtering. For e.g., can display billing info related to a particular project

### Enabling the feature
- Do it via the Tags tile in properties tab.
- One point to note is that you must activate your cost allocation tags from within AWS Billing before they will show up on any reports. To do this, go to your AWS Billing and Cost Management dashboard from within the AWS Management Console, select Cost Allocation tags and then activate any user-defined tags that you created.

---

## Transfer Acceleration

- When transferring data into or out of Amazon S3 from and to your remote client, or to another AWS region, transfer acceleration can dramatically speed up the process by utilizing another AWS service, Amazon CloudFront.
- Amazon CloudFront is a content delivery network service (CDN), which essentially provides a means of distributing traffic worldwide via edge locations. AWS edge locations are sites deployed in major cities and highly populated areas across the globe.
- When transferring data to S3 from your client with transfer acceleration enabled at the bucket level, the request will go via one of the CloudFront Edge Locations, from here the transfer request will then be routed through a high speed optimized AWS network path to Amazon S3.

### Cost

- When using transfer acceleration you should be aware that there is a cost. Whereas normal data transfer into amazon S3 is free from the internet, with transfer acceleration, this is a cost associated per GB depending on which edge location is used.
- Also, there is an increased cost for any data transferred OUT of S3, either to the internet or to another Region, again due to the edge location acceleration involved.

### Enabling the feature

- As expected, you can do via the properties tab -> Transfer Acceleration tile
- Can then enable or suspend as required
- Bucket name must be DNS compliant and not contain any periods
- Also, to make use of the transfer acceleration feature itself, any requests, such as GET or PUT to the bucket, must use this new transfer acceleration endpoint.

### Operations not supported

- GET Service (list buckets)
- PUT Bucket (create bucket)
- DELETE Bucket
- Cross region copies using PUT Object - Copy

---

## Events


- How to configure your bucket to monitor specific events that may occur within the them.
- Any events which are recorded can then be sent to either an SNS topic, an SQS Queue or a Lambda Function

### Enabling the feature

- Via the Properties tile
- Allows you to choose which events to monitor
- Prefix and suffix allow you to match specific objects
- Send to allows you to choose method of transfer - either an SNS topic, an SQS Queue or a Lambda Function
	- Permissions need to granted for S3 to send events via the above methods

---

## Requester Pays

- When this feature is enabled, any costs related to requests and data transfer becomes the responsibility of the requester instead of the bucket owner
- The bucket owner will still however pay for the storage costs associated with the objects stored in the bucket

### Enabling the feature

- A fundamental condition of enabling requester pays is ensuring that all access is authenticated to your bucket, as anonymous access requests will not be able to take advantage of the requester pays attribute.
- To enable requester pays is very simply, select the ‘Requester Pays’ tile from the properties of the required bucket
- The options available are either to enable requester pays or disable requester pays.
- From this point, any POST, GET or HEAD requests to the bucket must include x-amz-request-payer in the header and this parameter confirms that the requester is aware that there are cost implications associated with that request for requester pays.

---

## Summary

1. Versioning
	1. allows multiple versions of the same object to exist
	2. This is useful to allow you to retrieve previous versions of a file, or recover a file should it subjected to accidental deletion, or intended malicious deletion of an object.
2. Server Access Logging
	1. when this is enabled on a bucket it captures details of requests that are made to that bucket and its objects.
	2. Logging is important when it comes to security, root-cause cause analysis following incidents, and it can also be required to conform to specific audit and governance certifications.
3. Static Website Hosting
	1. if you are looking to create a simple and static website that requires no server-side scripting of any kind, then it can easily be hosted within one of your Amazon S3 buckets.
4. Object-level Logging
	1. This feature is actually more closely related to the AWS CloudTrail service than S3, as it’s AWS CloudTrail that performs logging activities against Amazon S3 data events.
	2. These data events are specific API calls used in S3, such as GetObject, DeleteObject, and PutObject.
5. Default Encryption
	1. When using default encryption, you are able to set a default encryption mechanism for every new object that is uploaded to the bucket.
	2. However, for any objects that are already in your bucket prior to enabling default encryption, they will NOT be encrypted.
6. Object Lock
	1. This feature is often used to meet a level of compliance known as WORM, meaning Write Once Read Many.
	2. It allows you to offer a level of protection against your objects in your bucket and prevents them from being deleted, either for a set period of time that is defined by you or alternatively prevents it from being deleted until the end of time!
	3. The ability to add retention periods using Object Lock help S3 to comply with regulations such as FINRA.
7. Tags
	1. Known as S3 cost allocation tags, you can assign key-value pairs at the bucket level to help with bucket and object categorization.
	2. Using the Cost Explorer you can then report on these key-values to identify and highlight the costs associated with your resources with specific key-value pairs.
8. Transfer Acceleration
	1. Transfer acceleration can dramatically speed up the process of transferring data into or out of Amazon S3 from and to your remote client, or to another AWS region, by utilizing Amazon CloudFront.
	2. With transfer acceleration enabled at the bucket level, the request will go via one of the CloudFront Edge Locations, where the request will then be routed through a high speed optimized AWS network path to Amazon S3.
9. Events
	1. Events allow you to monitor specific events that may occur within your buckets which can then be sent to either an SNS Topic, an SQS Queue or a Lambda Function.
10. Requester Pays
	1. which simply passes the costs of all object requests and data transfer becomes to the requester instead of the bucket owner.
	2. The bucket owner will still, however, pay for the storage costs associated with the objects stored in the bucket.

---
