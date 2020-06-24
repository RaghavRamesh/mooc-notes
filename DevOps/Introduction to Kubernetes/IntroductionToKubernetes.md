# Introduction to Kubernetes - Cloud Academy course by Logan Rakai

### Course Introduction

#### Part 1 - Introduction

##### Course Overview
* Kubernetes Overview
* Deploying Containerised Applications to k8s
* The Kubernetes ecosystem

##### Learning Objectives
* Describe k8s and what it is used for
* Deploy single and multiple container applications on k8s
* Use k8s services to structure N-tier applications
* Manage application deployments with rollouts in k8s
* Ensure container preconditions are met and keep containers healthy
* Learn how to manage configuration, sensitive, and persistent data in k8s
* Discuss popular tools and topics surrounding k8s in the ecosystem

##### Prerequisites
* Docker

Recommendation: Do the Intro to K8s Playground lab after this lesson

---

### Overview of Kubernetes

#### Part 2 - Kubernetes Overview

* Open-source orchestration tool designed to automate deploying, scaling, and operating containerised applications.
* Born out of Google’s experience running production workloads at scale
* Allows organisations to increase their velocity by releasing and recovering faster with k8s’ self-healing mechanisms
* K8s is a distributed system
* Machines may be physical, virtual located on-prem or in the cloud
* Schedules containers on machines
* Moves containers as machines are added/removed
* Can use different container runtimes, most commonly Docker
* Modular, extensible design
* Excellent end-user abstractions
    * Declarative configuration
    * Deploy containers
    * Wire up networking
    * Scale and expose services
* For Operations
    * Automatically recover from machine failures
    * Built-in support for machine maintenance
    * Join clusters with federation - if one dies, containers will automatically move to other clusters
* Feature highlights
    * Automated deployment rollout and rollback
    * Seamless horizontal scaling
    * Secret management
    * Service discovery and load balancing
    * Linux and windows container support
    * RBAC
    * Batch job processing
    * CPU and memory quotas
    * Persistent volume management
    * Stateful application support
    * Simple log collection
* There has been a surge in tools to support enterprises adopting containers in production

---

#### Part 3 - Deploying Kubernetes

_skipped taking notes_

---

#### Part 4 - Kubernetes Architecture

* K8s clusters are the highest level of abstraction
* Cluster refers to all of the machines collectively and can be thought of as the entire running system
* Nodes are the machines in the clusters. They can be physical or virtual machines
* Nodes are categorised as workers or masters
* Each worker node includes software to run containers managed by the k8s control plane
* Master nodes run the control place
* The control plane is a set of APIs and software that k8s users interact with
* The APIs and software are referred to as master components

##### Scheduling
* Control plane schedules containers onto nodes. The term scheduling doesn’t refer to time in this context. Similar to how a kernel schedules processes onto the CPU.
* Scheduling decisions consider required CPU and other factors.
* Scheduling refers to the decision process of placing containers onto nodes.

##### K8s Pods
* Containers are grouped into pods
* All containers in the pod run on the same node
* Pods are the smallest building block in k8s
* More complex and useful abstractions are built on top of Pods.

##### K8s Services
* Services define networking rules for exposing groups of Pods
    * To other Pods
    * To the public internet

##### K8s Deployments
* Manage deploying configuration changes to running Pods
* Horizontal scaling

##### Summary
* K8s is a container orchestration tool
* A group of Nodes form a K8s Cluster
* K8s runs containers in groups called Pods
* K8s Services expose Pods to the Cluster and to the public internet
* K8s Deployments control rollout and rollback of Pods

---

#### Part 5 - Interacting with Kubernetes
##### Kubernetes API Server
* Modify cluster state information by sending requests to the K8s API server
* The API server is a master component that acts as the front-end for the cluster

##### Interacting with K8s
The different methods are as follows
1. REST API
    * It is possible but not common to work directly with the API server
    * You might need to if there is K8s client library for your programming language
2. Client Libraries
    * Handles authenticating and managing individual REST API requests and responses
    * K8s maintains official client libraries for Go, Python, Java, .NET and JS.
    * There are also many community-maintained libraries when no official lib exists.
3. Command-line tool kubectl - most commonly used
    * Issue high-level commands that are translated into REST API calls
    * Works with local and remote clusters
    * Your k8s success correlates with kubectl skill. You can accomplish all your day-to-day skills with this command
    * Manages all different types of k8s resources, and providers debugging and introspection features
    * Luckily, kubectl follows easy-to-understand design patterns: when you learn to manage one resource, you learn to manage them all

##### Basic `kubectl` commands
* `kubectl create` to create resources (Pods, Services, etc.)
    * Using built-in sub-commands of create or
    * By using files. The files are commonly in yaml format and are referred to as manifests
* `kubectl delete` to delete resources
* `kubectl get` to get list of resources of a given type
* `kubectl describe` to print detailed info about a resource(s)
* `kubectl logs` to print container logs of a particular pod or of a specific container in a multi-container pod

##### Web dashboard
* Optional so not all clusters have it but is a great way to get an overview of your cluster(s)

---

### Deploying Containerised Applications to Kubernetes

#### Part 6 - Pods

_to be continued..._