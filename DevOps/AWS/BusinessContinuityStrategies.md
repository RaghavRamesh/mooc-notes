# Introduction

- This course will focus on some of the common disaster-recovery strategies, designed to help your organization recover from an incident that has affected your AWS environment.
- By adopting a DR strategy, it helps to reduce the impact your organization following a disaster. You should always plan and prepare for the worst and design a business continuity plan that is appropriate for the systems that you will need to restore.
- The strategy you choose will depend on your design, criticality, and importance that the solution has on your business. 
- Topics covered:
	- RTO Recovery Time Objective
	- RPO Recovery Point Objective
	- DR strategies including Backup and Restore, Pilot Light, Warm Standby and Multi-Site
- The objectives of this course are to find you with an understanding of some of the common DR Strategies adopted when utilizing AWS infrastructure and the benefits of each method

---

# Disaster Recovery & Business Continuity

## Backup and DR Strategies

A company typically decides on an acceptable business continuity plan based on the financial impact to the business when systems are unavailable. So the company determines the financial impact by considering many factors, such as the loss of business and the damage to its reputation due to downtime or the lack of systems availability.

The common metrics for business continuity, commonly refer to Recovery Time Objective and the Recovery Point Objective. So let's just delve into these two concepts.

### Recovery Time Objective (RTO)

- The time it takes after a disruption to restore a business process to its service level as was defined by the operational level agreement.
- So for example, if a disaster occurs at 12 o'clock lunchtime and the RTO is eight hours, then the disaster recovery process should restore the business process to the acceptable service level by 8:00 p.m.

### Recovery Point Objective (RPO)

- The acceptable amount of data loss measured in time.
	- So just to confuse everyone, it is also a time value, but it's a slightly different one. The two are quite different concepts.
- The acceptable amount of data loss measured in time.
- So for example, if that disaster occurred at 12 o'clock around lunchtime and the RPO is one hour, the system should recover all data that was in the system before 11:00 a.m. So the data loss will spend only one hour between 11:00 a.m and 12:00 p.m.
- Recovery Point Objective is what's the last point in the data can we successfully absorb if there is an outage and for a highly transactional business, that's going to be extremely low. I mean, even having an hour of data loss, if you're dealing with customer transactions, is not gonna be acceptable to a transactional business.

And that's gonna impact how we design our systems to be as highly available and as fault tolerant as possible for a transactional business like that.
Another scenario might be that the business can absorb some outage, but it does need to have the systems up and running again as soon as possible. In which case RTO might be the priority.

And part of your business continuity planning needs to be, to define what is the priority, the Recovery Time Objective, i.e how quickly we can get the system back up and running again so it can answer queries and requests and be fully functional, or is it the Recovery Point Objective that's our priority that we must be able go back to the last possible point in time without any data loss.








There's a number of different scenarios that we can apply in AWS to help meet the RPOs and RTOs. And the first one is what we call backup and restore now with backup and restore data is stored as a virtual tape library using AWS storage gateway or another network appliance or of a similar nature.



### Backup and Restore

Now we need to select the appropriate tools and methods to back up our data into AWS. Three things to keep in mind first, ensure that you have an appropriate retention policy for this data.
1. How long we go to keep, these virtual tape archives for, is it six months? Is it a year? Is it five years? What are the commercial and, compliance requirements, etc?
2. Ensure that the appropriate security measures are in place for this data, including the encryption and access policies. So can we guarantee that where it's been stored is gonna be suitably secure?
3. We need to make sure that we regularly test the recovery of the data and the restoration of the system.

### Pilot Light

Data is mirrored and the environment is scripted as a template, which can be built out and scaled in the unlikely event of a disaster and a few steps that we need to go through to make pilot light work.


1. First, we set up Amazon EC2 instances to replicate or mirror our data.
2. Second, we ensure that we have all supporting custom software packages available in AWS. So that can be quite an over operational overhead to ensure that we have all of the latest and greatest custom software packages that we need for our environment available in AWS.
3. Third, we need to create and maintain Amazon machine images of a key service where fast recovery is required.
4. Fourth, we need to regularly run these servers, test them and apply any software updates and configuration changes to ensure that they're going to match what our production environment currently is in the event of a disaster
5. Fifth we need to consider automating the provisioning of AWS services as much as possible with cloud formation.

Basically horizontal scaling is often the most cost effective and scalable approach to add capacity to the pilot light system. As an example, we can add more web servers at peak times during the day.
However, we can also choose larger Amazon EC2 instance types and thus scale vertically for applications such as our relational databases and file storage, for example. And any required DNS updates can be done in parallel.

### Warm standby


Essentially, ready to go with all key services running in the most minimal possible way.
1. First we set up our Amazon EC2 instances to replicate or mirror data.
2. Secondly, we create a maintain Amazon machine images as required
3. Third, we run our application using a minimum footprint of AWS EC2 instances or AWS infrastructure. So it's basically the bare minimum that we can get by with.
4. Fourth, we patch and update software and configuration files in line with our live environment. So we're essentially running a smaller version of our full production environment.

Then during our recovery phase, in the case of failure of the production system, the standby environment will be scaled up for production load. And the DNS records will be changed to route all traffic to the AWS environment.

### Multi-site


With multi-site we set up our AWS environment to duplicate our production environment. So essentially we've got a mirror of reproduction running in AWS.
1. Firstly, we set up DNS waiting or a similar traffic routing technology if we're not using route 53 to distribute incoming requests to both sites.
2. we also configure automated fail over to reroute traffic away from the affected site in the event of an outage.

Now in our recovery phase, traffic is cut over to the AWS infrastructure by updating the DNS record in Route 53 and all traffic and supporting data queries are supported by the AWS infrastructure. Our multi-site scenario is usually the preferred one and where time is a priority Recovery Time and Recovery Point Time are our priorities and costs are not the main constraint the nets would be the ideal scenario.


Okay, so one key thing to ensure when we're running any of our scenarios is to ensure that we test the recover data. So once we've restored our primary site to a working state, we then need to restore to a normal service, which is often referred to as a fail back process. So depending on your DR strategy, this typically means reversing the flow of data replication so that any data updates received while the primary site was down can be replicated back without loss of data.

### Testing Recovered Data

#### Backup and Restore
1. Freeze data changes to the DR site
2. Take a backup
3. Restore the backup to the primary site
4. Re-point users to the primary site
5. Unfreeze the changes

#### Pilot Light, Warm Standby and Multi-site
1. Establish reverse mirroring/replication from the DR site back to the primary site, once the primary site has caught up with the changes
2. Freeze data changes to the DR site.
3. Re-point users to the primary site.
4. Unfreeze the changes.

### Replication: Considerations

When you replicate data to a remote location, you really need to think through a number of factors.
- Distance between the sites - larger distances typically are subject to more latency or jitter
- Available bandwidth - the breadth and variability of the interconnections. If that bandwidth doesn't support high burst activity, then it's not gonna suit some replication models.
- Data rate required by your application - the data rate should be lower than the available bandwidth
- Replication technology - the replication technology should be parallel (so that it can use the network effectively)

### Replication: The basics
(can be confusing)
#### Synchronous replication
- Atomic write update to both (puts a dependency on network performance and availability)
- E.g., when deploying a multi-AZ mode, Amazon RDS uses synchronous replication to duplicate data to a second availability zone. This ensures that data is not lost. If the primary availability zone becomes unavailable.

#### Asynchronous replication
- Non-atomic write update happens to secondary as network and bandwidth permit and the application continues to write data that might not be fully replicated yet.
- E.g., many database systems support asynchronous data replication, the database replica can be located remotely and the replica does not have to be completely synchronized with the primary database server. And that's acceptable in many scenarios, for example, as a backup source or reporting read only use cases. In addition to both database systems, you can also extend asynchronous replication to network file systems and data volumes.

---

## Summary

- Backup & Restore
	- Description: like using AWS as a virtual tape library
	- RTO: Highest (8-24 hours) because we need to factor in the time it would take us to access or download backup archives
	- RPO: Quite high as well, because if we're only doing daily backups, it could be up to 24 hours
	- Cost: low
- Pilot Light
	- Description: minimal version of our environment running on AWS which can be "lit up" and expanded to full-size when needed.
	- RTO: Lower than B&R (4-8 hours) but we need to factor in that we still may need to install applications or patches onto our AMIs before we have a fully operational system. 
	- RPO: reasonably low. Since the last snapshot
	- Cost: low
- Warm Standby
	- Description: we've got that scaled-down version of a fully functional environment, always running.
	- RTO: Lower than pilot light (<4 hours) is likely to be lower than pilot light as some of our services are always running.
	- RPO: Quite a good RPO. And the benefit of having a warm standby environment is that we can actually use it for Dev test or for one-off projects or for skunkworks, etc.
	- Cost: medium
- Multi-site
	- Description: fully operational version of our environment running off-site or in another region.
	- RTO: it's likely to give us our lowest Recovery Time Objective if we're using active/active fail-over, it could be a matter of seconds
	- RPO: likewise, it depends on the choice of data replication that we choose. But it's gonna be since our last asynchronous or synchronous DB write and using Route 53 as an active/active fail over, it's gonna give us a very, very aggressive, short Recovery Point Objective and Recovery Time Objective.
	- Cost: The considerations with that is that the cost is going to be reasonably higher proportionately than the other three options we have and we need to factor in it that there will be some ongoing maintenance required to keep that kind of environment running. The benefit is that we have a way of regularly testing our DR strategy. We also have a way of doing Blue-green deployments, and it gives us a lot more diversity in our IT infrastructure.

---