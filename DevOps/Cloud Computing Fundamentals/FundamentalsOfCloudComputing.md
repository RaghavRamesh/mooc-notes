# Fundamentals of Cloud Computing - Pluralsight course by David Davis

### Module 1 - Course Overview
- Introduction
- The Basics
- Infrastructure as a Service
- Infrastructure as a Service Networking
- Storage in the Cloud
- Infrastructure as a Service: Security
- Comparing Infrastructure as a Service (IaaS) and Platform as a Service (PaaS)
- Software as a Service (SaaS) Cloud Solutions
- Summary

***

### Module 2 - Introduction
- Why cloud computing?
    - It’s everywhere - you’re using cloud computing in some shape or another on a daily basis
    - It’s powerful
    - It makes your life easier - allows you to do more, with less, be more efficient, save time and money

***

### Module 3 - The Basics
- What is cloud computing?
    - On-demand computing resources delivered to you over the Internet
    - A computing service that you traditionally did local (on-premises), now performed remotely across the Internet (off-premises)
        - Computing that you need but done by someone else situated somewhere else

    - According to VMWare - “Cloud computing is an approach to computing that leverages the efficient pooling of an on-demand, self-managed, virtual infrastructure."

- Traits of a cloud
    - Elastic - scales up or down quickly
    - Metered - pay only for what you use
    - Self-service - No (or reduced) need for IT Experts

- You must be able to answer these questions:
    - Where are my applications running?
    - Where is my data stored?

- Benefits of cloud computing
    - Fast access to resources or applications you or your company needs
    - Only pay for what you use
    - No capital expenditure to get started
    - Potential to eliminate the need for local IT staff to maintain infrastructure and applications
    - Potential to lower costs - when it comes to computing and application.
    - Deploy what you need, yourself, with self-service
    - Allow IT staff to instead focus on the business

- Risks of cloud computing
    - Placing your trust in the cloud provider - if they go down, you don’t have access to your applications and data
        - Need to be aware of the SLA with the cloud provider

    - Potential for data loss
    - Potential for slow access to your data
    - Potential questions related to legal/regulatory
    - Potential for loss of customisation
    - Potential for unknown costs
        - Might end up with a much larger bill than anticipated

- Many forms of cloud computing
    - SaaS - Gmail, Dropbox
    - IaaS
    - PaaS - only need to manage their application without worrying about underlying OS, servers, networking, etc.

- History
    - CC was known by many other names and they go back as early as 1950 - 1960s

***

### Module 4 - Infrastructure as a Service
- What is virtualisation?
    - Dictionary definition: Virtualisation is the logical division of physical computing resources.
    - Makes a physical server into a virtual host machine so that the virtual host can then run virtual machines
    - Started in 1960s as a way to slice up mainframe resources

- Hypervisor
    - It is the software that makes virtualisation possible.
    - Running on a hypervisor, a virtual machine is software-based instance of a physical server where a guest operating system has access to emulated virtual hardware.
    - A hypervisor is loaded on the virtual host to run virtual machines
    - Virtual guests run on a virtual host, which provides the physical resources
    - An operating system and applications are loaded in the guest. They don’t know the difference between running on physical or virtual hardware. They think they have full access to the full hardware and resources. They are provided the virtual resources by the hypervisor.

- Container
    - Dictionary definition: A container is an OS-level virtualisation were the OS kernel provides isolated user spaces to run specific applications.
    - Have been around for a long time - since Linux / Unix
    - Could run inside a virtual machine. Can use them with virtualisation.
    - Have less overhead and faster startup times than virtual machines
    - Have been popularised with the excitement around Docker containers
    - Cloud computing utilises both virtualisation and containers

- Different types of IaaS Cloud offerings
    - Private clouds
        - Run in company’s own buildings, infra, data centres, etc.

    - Public clouds
        - AWS EC2, Azure VMs, etc.

    - Hybrid cloud
        - It’s not real place where you can run applications. What it means is that the Private and Public clouds are connected and they are working together.
        - For e.g., can augment public cloud resources to support private cloud.
        - Also called Enterprise Cloud.

- Virtualisation vs. Private Cloud
    - Virtualisation
        - Required for cloud computing
        - Provides:
            - Scalability / elastic computing
            - Resource sharing and pooling
            - Load balancing for the compute
            - High availability at the infrastructure level
            - Portability
            - Cloning (apps, data, etc.)

    - Private Cloud
        - On top of virtualisation, provides:
            - Abstraction of underlying infrastructure layer - you don’t know anything about the virtualisation underneath.
                - Don’t know where the hosts are running, where VMs are stored, how networking is setup.
            - Secure and contained multi-tenancy
            - Self-service portal - can deploy a new VM easily without having to worry about the underlying infra
            - Catalog of applications
            - Chargeback / showback - can see how much we are using
            - Potential to burst to a hybrid cloud - i.e., users of the cloud won’t know whether their stuff is running in the private or the public cloud

- IaaS pricing is, ideally, utility / consumption / subscription-based where you pay for what you use
    - They can get complex
        - On-demand pricing - pay by the hour with no long term commitments. Majority use case
        - Spot-instances - can get up to 90% discount of the actual price. Downside is that the compute can go away at any point so don’t wanna run any critical apps
        - Reserved instances - cost lesser than on-demand but need to commit for 1-3 years. For production apps in the long-term
        - Dedicated hosts - for most critical apps

- SLA with your Cloud Provider is important
    - Defines what level of performance and availability the IaaS provider will provide you
    - And what they will do if they are unable to provide you that level of service - i.e., money back, credits, etc. E.g. when data is suddenly gone, what compensation will the provider provide you for that loss for your customers, your reputation.
    - Every provider is going to have a different SLA - need to check if your company is okay with that

- Migrating to the Cloud
    - Greenfield deployments - i.e., new application =&gt; directly do it in the cloud. Easiest.
    - Things to consider before migrating to the cloud
        - Costs associated with using the cloud? There are some calculators that can help
        - Security, availability and performance?
        - Migrate vs rebuild?
        - Enterprise-grade functionality? Like firewall, data protection, anti-virus software, disaster recovery, replication, high availability. Can’t assume these are automatic, need to make sure these are available.
        - Tools that can help migrate? E.g. for data migration.

***

### Module 5 - IaaS Networking
- IaaS Networking Options - How do servers in the IaaS cloud connect to the network?
    - Public networking - the VMs are on the Internet using public IP addresses
    - Private networking - via VPN, or in some cases, dedicated connections - purchased through e telecom carriers

- Virtual Private Cloud (VPC) Networking
    - Amazon VPC is not a private cloud, but a private network that can be connected to your network
    - You can control over -
        - Private IP address networking
        - Routing
        - Network access lists
        - And VPN connectivity back to your on-premises infrastructure

***

### Module 6 - Storage in the Cloud
- Where are the files stored?
    1. Inside the Virtual Machine - for e.g., database running in VM
        - Each VM has a storage on its own


    2. Outside the virtual machine
        - Each VM can access external storage like block or object storage. E.g. Amazon EBS - Elastic Block Storage; Object storage - perfect for unstructured data like graphics of a website.

- Cloud File Storage
    - Dropbox
    - Microsoft OneDrive
    - Google Drive

- Object storage - for unstructured data
    - Pictures, videos, and archival data.
    - E.g. Amazon S3 - Simple Storage Service, Microsoft Azure Blob Storage

- Data Protection in the Cloud
    - File backup? There are different cloud data protection solutions - Crash plan, Carbonite, AWS Storage Gateway, Microsoft Azure Backup
    - VM backup? Use disaster recovery. Solutions like Zerto, VEEAM, Infrascale
    - Pay only for what you consume and in some cases only pay when there is a disaster

***

### Module 7 - Infrastructure as a Service: Security
- Cloud security concerns
    - Is my data safe?
    - Who can see my data?
    - Can my data be modified?
    - Who is responsible for security my data?
    - Unless you have managed security, the security of your data in the cloud is under shared responsibility
    - Solutions
        - AWS Security - they deal with compliance, penetration testing, vulnerability reporting, free support and professional services, etc.

- Compliance - laws, requirements, policies, rules, standards, governance, regulations in dealing with data. Check out Amazon Cloud Compliance. Companies may need to meet these before migrating to the cloud.
- Vulnerabilities and Mitigation
    - Who keeps my operating systems and applications up to date?
    - Who performs backups of the data, should something happen? Have they been tested?
    - Who performs security scans of my VMs?
    - Checkout Amazon Vulnerability Reporting

***

### Module 8 - Comparing IaaS and PaaS

PaaS
- PaaS is for developers - that’s the main distinction between PaaS and IaaS
    - IaaS is for infrastructure people or for developers who want more control over their infra

- What is PaaS?
    - A cloud service for developers who want to develop, run and manage applications
    - No servers, storage, network, OS, middleware or databases are needed.
    - Examples
        - AWS Elastic Beanstalk
        - Microsoft Azure App Service
        - Google App Engine
        - Cloud Foundry
        - Heroku


IaaS vs PaaS

IaaS
- All servers, storage and networking are provided
- Developers must install their own software such as web servers, database servers, etc.

PaaS
- PaaS runs on top of infrastructure as a service
- Developers have access to already installed web servers, database servers, and development libraries

***

### Module 9 - Software as a Service Cloud Solutions

Benefits of SaaS Cloud Solutions
- No hardware/software to buy or install
- No software to maintain or upgrade
- You only pay for what you use
- New features are automatically included
- Affordable for everyone that used to be only for large corporations

***

### Module 10 - Summary
