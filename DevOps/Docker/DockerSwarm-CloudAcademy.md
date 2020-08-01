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

_to be continued..._
