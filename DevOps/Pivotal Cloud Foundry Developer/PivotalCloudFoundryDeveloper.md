# Pivotal Cloud Foundry Developer - Pluralsight course by Pivotal Education

### Module 1 - Introduction

##### Pivotal Cloud Foundry Introduction
Agenda
1. Evolution of cloud architectures
2. Industry trends
3. Cloud Foundry
4. Pivotal Cloud Foundry - Pivotal built upon CF to provide an enterprise offering

- Advent of IaaS meant the bottom half was taken care of by the service provider which expedited the process of getting an application out to prod.
- Cloud Native Platform, took away some more of the responsibilities from the hands of the developer. 
Deploying an application: IaaS vs PCF

IaaS
- Provision a VM
- Install application runtime
- Deploy application
- Configure Load Balancer
- Configure SSL termination
- Configure service connectivity
- Configure firewall

PCF equivalent
- `cf push`. That’s it. 

Common application needs are handled by the platform, so you can focus on business value. 

Scaling an application: IaaS vs PCF
IaaS
- Provision a VM
- Install application runtime
- Deploy application
- Configure Load Balancer
- Configure SSL termination
- Configure service connectivity
- Configure firewall

PCF equivalent
- `cf scale`

Staging - taking your app, determining all the runtime support it needs. The outcome is a droplet - app + dependencies which is run within a container 

##### Industry Trends
Jamie Dimon said, Silicon Valley is coming. A number of industries have been displaced by the software industries. 
Business expectations have gone up but IT budget has stayed the same. So there is a huge innovation gap. 
Software continues to be constrained by Silos even in the cloud world. PCF provides an advantage by being capable of running on top of any of these Cloud providers. 

##### Cloud Foundry
- Open source
- Lot fo companies have committed to the project including banks like Citi and JPMC

##### Pivotal Cloud Foundry
- Simplified the install experience
- It’s a large distributed system and PCF gives an easy UX using management dashboard
- Provides Apps manager - app dashboard, marketplace
- Pivotal brings a number of services - Redis, RabbitMQ, MySQL, SSO, AWS, Spring Cloud Services, CloudBees Jenkins, apigee API gateway, etc. 
- 3 core tenets - 
    - Self serve platform for developers
        - cf push 
        - Service Marketplace
        - Microservice ready platform
    - Operationally robust
        - Elastic Scale
        - High Availability 
        - Metrics
        - Logs
        - Quotas
    - IaaS independent
        - AWS
        - VMware
        - Openstack
        - Azure
* * *

### Module 2 - Logging, Scale, and High Availability

* * *

### Module 3 - Services and manifests 
