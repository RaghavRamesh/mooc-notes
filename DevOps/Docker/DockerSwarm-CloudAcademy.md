# Container Orchestration with Swarm Mode - Cloud Academy course by Logan Rakai

## Introduction

### Part 1 - Course Introduction

#### Pre-requisites
* Docker
* Docker Compose

#### Learning objectives
* Describe what Docker swarm mode can accomplish
* Explain the architecture of a swarm mode cluster
* Use the Docker CLI to manage nodes in a swarm mode cluster
* Use the Docker CLI to manage services in a swarm mode cluster
* Deploy multi-service applications in swarm mode using Stacks

---

### Part 2 - Overview

#### Agenda
* Why we need swarm mode?
* What swarm mode can do for you?
* Main concepts of Docker swarm mode
* They universal control plane

#### Why Swarm?
* When container applications reach a certain level of complexity or scale, you need to make use of several machines
* Container orchestration products and tools allow you to manage multiple container hosts in concert

#### Swarm Mode
* It’s a feature built into the Docker Engine providing native container orchestration and cluster management in Docker
* When enabled, you can manage a cluster of containers in the same easy way as you would a single container
* Features
    * Integrated cluster management within the Docker Engine
    * Declarative service model
    * Desired state reconciliation
    * Certificates and cryptographic tokens to secure the cluster
    * Container orchestration features - service scaling, multi-host networking, resource-aware scheduling, load balancing, rolling updates, restart policies
* Docker has 2 cluster management solutions
    * Docker Swarm standalone
        * The 1st container orchestration project by Docker
        * Uses Docker API to turn a pool of Docker hosts into a single virtual Docker host
        * Now referred to as Docker Swarm standalone
    * Docker Swarm mode (swarmkit) - the newer one
        * Built into the Docker engine since version (1.12)
        * Docker generally recommends swarm mode
        * This course focuses on swarm mode

#### Swarm Mode Concepts
* Swarm
    * One or more Docker Engines running in swarm mode
* Node
    * Each instance of the Docker Engine in the swarm
    * It is possible to run multiple nodes on a single machine (e.g., virtual machines)
* Managers
    * Every Swarm needs at least one manager
    * Managers accept specifications from users and drive the swarm to the specified desired state
* Workers
    * Responsible for running the delegated work
    * Workers also run an agent which reports back to managers on the status of their work
* Service
    * Specifications that users submit to managers (Same as service in DC)
    * Declares its desired state, including the networks and volumes, the number of replicas, resource constraints and other details
    * The Manager ensures the actual state of the swarm matches the service spec (sounds familiar?)
    * There are 2 kinds of services: replicated and global
* Replicated Service
    * The number of replicas for a replicated service is based on the scale desired
* Global Service
    * Allocates one unit of work for each node in the swarm
    * Can be useful for monitoring services
* Tasks
    * The units of work delegated by managers to realise a service configuration
    * Tasks correspond to running containers that are replicas of the service
* Universal Control Plane
    * UCP is only relevant for the enterprise edition of Docker. The web interface equivalent for Docker CLI for Swarm mode for cluster management and RBAC

---

## Architecture

### Part 3 - Architecture: Networking

#### Agenda
* Overlay networks
* Service discovery
* Load balancing
* External access

#### Networking
* Unlike a single container, the networking needs in Swarm mode are much more complex
* In swarm mode, services need to communicate with one another and the replicas of the service can be spread across multiple nodes

#### Overlay Networks
* Multi-host networking in swarm is natively supported with the overlay driver
* No need to perform any external configuration
* You can attach a service to one ore more overlay networks
* Overlay networks only apply to swarm services
* Managers automatically extend overlay networks to nodes

#### Network Isolation and Firewalls
* Network isolation and firewall rules apply to overlay networks just as they do for bridge networks
* Containers within a Docker network have access on all ports in the same network
* Traffic originating inside of a Docker network and not destined for a Docker host is permitted
* Traffic coming into a Docker network is denied by default
#### Service Discovery
* A service discovery mechanism is required in order to connect to the nodes running tasks for a service
* Swarm mode has an integrated service discovery system based upon DNS
* The same system is used when not running in swarm mode
* The network can be an overlay spanning multiple hosts, but the same internal DNS system is used
* All nodes in a network store corresponding DNS records for the network

#### Internal Load Balancing
* Each individual task is discoverable with a name to IP mapping in the internal DNS
* Requests for VIP address are automatically load balanced across all the healthy tasks in the overlay network

#### DNS Round Robin
* DNS Round Robin allows you to configure the load balancing on a per service basis
* The DNS server resolves a service name to individual task IP addresses by cycling through the list of IP addresses of the nodes
* If you need more control, DNS RR should be used for integrating your own external load balancer

#### External Access
* In swarm mode, there are two modes for publishing ports in Swarm
* Host mode
    * The container port is published on the host that is running the task for a service
    * If you have more tasks than available hosts, tasks will fail to run
    * You can omit a host port to allow Docker to assign an available port number in the default port range of 30000-32767
    * There isn’t load balancing unless you configure it externally
* Ingress Mode
    * You have the option to load balance a published port across all tasks of a service
    * Requests are round robin load balanced across the healthy instances regardless of the node
    * It is the default service publishing mode

#### Routing Mesh
* The routing mesh combines an overlay network and a service virtual IP
* When you init a swarm, the manager creates an overlay ingress network
* Every node that joins the swarm is in the ingress network
* When a node receives an external request it resolves the service name to a VIP
* The IPVS load balance the request to a service replica over the ingress network
* You can add an external load balancer on top of the load balancing provided by the routing mesh

---

### Part 4 - Orchestration

#### Agenda
* Service placement: which nodes service tasks are placed on
* Update behaviour: how service updates are rolled out
* Rollback behaviour: how services can be rolled back to a previous version

#### Service placement
* Can declare a set number of replicas (like deployment in k8s)
* Or can be started on every worker node in a cluster as a global service (like daemon set in k8s - not entire sure about this though)
* For replicated services, decisions need to be made by swarm managers for where service tasks will be scheduled
* 3 ways to influence scheduling:
    * CPU and memory reservations
    * Placement constraints
    * Placement preferences - influence how tasks are distributed across nodes
* Global services can also be restricted to a subset of nodes with these conditions
* A node will never have more than one task for a global service

---

### Part 5 - Consistency

#### Agenda
* Consistency
* Raft
* Tradeoffs

A Swarm can have several managers and worker nodes. If so how can consistency be ensured?
* All managers share a consistent internal state of then entire swarm
* Workers do not share a view of the entire swarm
* Managers maintain a consistent view of the state of the cluster by using a consensus algorithm

#### Raft Consensus
* Raft achieves consensus by electing one manager as the leader
* The elected leader makes the decisions for changing the state of the cluster
* The leader accepts new service requests and service updates and how to schedule tasks
* Decisions are acted only when there is a quorum
* Raft allows for (N-1)/2 fails and the swarm can continue operating
* If more managers fail, the cluster state would freeze (no new scheduling decisions are made)
* The first manager is automatically the leader
* If the current leader fails, a new leader is elected

#### Manager tradeoffs
* More managers => more failure tolerance but there are tradeoffs
* More managers increase the amount of managerial traffic required for maintaining a consistent view of the cluster and the time to achieve consensus
* Manager best practices:
    * You  should have an odd number of managers
    * A single manager swarm is acceptable for development and test swarms
    * 3 manager swarm can tolerate 1 failure; 5 can tolerate 2
    * Docker recommends a max. Of 7 managers
    * Distribute mangers across at least 3 availability zones

#### Working manager
* By default, managers perform worker responsibilities
* Having over utilised managers can be detrimental to the performance of the swarm
* You can use conservative resource reservation to make sure that managers won’t become starved for resources
* You can prevent any work from being scheduled on manager nodes by draining them

#### Working nodes
* More worker nodes don’t necessarily have any detrimental effects in the cluster. Helps with increasing capacity

---

### Part 6 - Security

#### Agenda
* Cluster management
* Data plane
* Secrets
* Locking a swarm

#### Cluster management
* Swarm mode uses PKI to secure swarm communication and state
* Swarm nodes encrypt all control plane comms using mutual TLS
* Docker assigns a manager that automatically creates several resources
* Root CA; Key pair; Worker token; Manager token;
* When a new node joins the swarm, the manager issues a new certificate which the node uses for communication
* New managers also get a copy of the root CA
* You can use an alternate CA instead of allowing Docker
* The CA can be rotated out if required
* CA will automatically rotate the TLS certs of all swarm nodes

#### Data Plane
* You can enable encryption of overlay networks at the time of creation
    * When a traffic leaves a host, an IPSec encrypted channel is used to communicate with a destination host
    * The swarm leader periodically regenerates and distributes the key used
    * Overlay network encryption is not support for Windows as of Docker version 17.12

#### Raft Logs/ Secrets
* Encrypted at rest (unlike k8s secrets)

#### Locking a Swarm
* By default, they keys are stored on disk along with the Raft logs
* You can use auto-locking to take control of the keys in the Swarm

---

## Demos
_skipped taking notes_

---

## Wrap up

### Part 11 - Summary
* Networking
    * Overlay networks
    * Load balancing - VIP, DNS RR
    * Routing Mesh - Ingress, external access
* Container orchestration
    * Influencing placement and rolling updates, rollbacks
* Consistency
    * Manager, worker nodes
    * Raft consensus
    * Tradeoffs with multiple managers
    * Raft logs
* Security
    * Secure by default - PKI, token semantics
    * Overlay network encryption
    * Swarm autolock
* Demos
    * Setup a swarm
    * Node management
    * Service management
    * Working with stacks

---
