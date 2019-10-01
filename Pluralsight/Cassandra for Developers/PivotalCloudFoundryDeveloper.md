# Pivotal Cloud Foundry Developer

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

Module agenda
1. Cloud Native Apps
2. Elastic Runtime Architecture
3. High Availability

##### Fundamental changes when moving to the Cloud
* DS: Distributed systems are hard to build, test, manage and scale - e.g. Microservices.
* Ephemeral infrastructure: Virtual machines are containers are temporary
* Immutable infrastructure: Updates to systems and applications are not done in-place but rather new, updated instances are created instead.
* Changes in app design
    * The 12 Factor App: Methodology for building web apps suitable for running on cloud platforms. Will take through whatever is pertinent at each section - starting with these 4
        * 1. Processes: execute the app as one or more stateless process. Store persistent state elsewhere.
        * 2. Concurrency: Scale out via the process model to be able to elastically scale.
        * 3. Disposability: Maximise robustness with fast startup and graceful shutdown.
        * 4. Logs: Treat logs as event streams. Application no longer has the responsibility of managing logs. All you have to do is to write out to stdout.

##### Elastic Runtime Architecture
* Elastic Runtime Subsystems
    * Diego
        * Scheduler. Schedules tasks and long-running processes (LRPs)
        * Task: guaranteed to run at most once.
            * e.g. stage an application no matter the number of instances
        * LRP: typically represented as a web app. LRPs can have multiple instances
        * Container: An application instance is run within an immutable container. It gives isolation in terms CPU, memory and disk.
        * Cell: Containers are run within a cell.
        * Garden: Containers are managed by Garden. A garden has many different back-ends so that it can support many platforms. Garden is an interface. Can have Windows-based and Linux-based backends.
        * Rep: It is a process running in the cell itself. It represents the Cell in the BBS (Bulletin Board System) / auctions.
        * Auction: A bid on executing tasks or LRPs.
        * Executor: A sub-process of the Rep and manages container allocations on the cell.
        * Metron: Forwards logs to Metron which forwards the logs to Loggregator.
        * BBS: is the API to access the Diego database for tasks and LRPs and it persists them in `etcd`.
        * Brain: is composed of two components.
            * Auctioneer: holds auctions for tasks and LRPs.
            * Converger: reconciles desired LRPs vs Actual LRPs through auctions.
    * Loggregator
        * Metron (living on Cell): gets logs from Metron
        * Doppler: Gather logs from Metron and create App Syslog Drains - drains to Splunk or Papertrail.
        * Traffic Controller: responsible for handling `cf logs` - also exposes a web socket endpoint called the firehose.
        * Firehose: a websocket endpoint that exposes app logs, container metrics and ER component metrics. Does not include ER component logs.
        * Nozzle: Pipe the firehose into nozzles to format the logs. e.g., Datadog Nozzle to see a nice visualisation
    * Cloud Controller API
        * Cloud Controller: Exposes an API for using and managing the Elastic Runtime.
            * Persists Org/Space/App data in the Cloud Controller Database
        * Blob Store: Persists app packages and droplets to the blob store.
        * CC-Bridge: translates app specific messages into the generic language (Diego) of tasks and LRPs. CC -> CC-Bridge -> BBS
    * Routing
        * The router routes traffic to appropriate component running in the different Cells.
* Key sequence flows through the ER
    * Stage and Run Request Flow: CC passes requests to run and stage applications to the CC-Bridge
        * CC -> CC-Bridge -> BBS -> Brain (Auctioneer) -> (Winner chosen and container provisioned on winning ) Cell -> Task and LRPs are executed.
    * Push sequence - `cf push`


##### High Availability
* Availability Zones - application instances are evenly distributed across availability zones. AZs are mapped to specific racks. In the event that one of the zones is down, your app instances are still up and running.
* BOSH Managed Processes - elastic runtime processes are monitored and automatically restarted.
* Failed VMs - will be recreated automatically.
* Self Healing Application Instances - Once running, failed application instances will be recreated.



* * *

### Module 3 - Services and manifests 