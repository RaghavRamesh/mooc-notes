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

* Basic building block in K8s
* One or more containers in a Pod
* Pod containers all share a container network that allows any Pod to talk to any other Pod regardless of which nodes they are running on
* One IP address per Pod
* K8s does all the heavy-lifting to make all of the above happen

##### What’s in a Pod Declaration
Because Pods include containers, the declaration of a pod includes all of the properties that you would expect for running containers, for example with `docker run`. These include the
* Container image
* Container ports
* Container restart policy
* Resource limits

##### Manifest files
* Declare desired properties
* Manifests can describe all kinds of resources
* The `spec` contains resource-specific properties

##### Manifests in Action
* `kubectl create` sends manifest to K8s API Server
* API Server does the following for Pod manifests
    1. Select a node with sufficient resources
    2. Schedule Pod onto node
    3. Node pulls Pod’s container image
    4. Starts Pod’s container

##### Why Use Manifests?
* Can check into source control
* Easy to share
* Easier to work with than stringing together long sequences of commands

##### Demo
* `apiVersion`: v1 is the core apiVersion containing many of the most common resources such as Pods and nodes. K8s supports many apiVersions
* `kind`: indicates what the resource is
* `metadata`: contains info relevant to the resource and can help identify resources
    * Minimum metadata is `name`
    * `name` must be unique in a namespace
* `spec`: specification for the declared `kind` and must match what is expected by the defined `apiVersion`
    * Essentially where all of the meat goes
* K8s cannot update the port on a running pod, so we need to delete and recreate it
* While deleting providing -f file will delete all resources specified in that file
* `label`: in addition to providing meaningful identification information, labels are used to make selections
* Quality of Service Classes
    * K8s can schedule pods based on their resource requests. Those pods without resource requests are easier to schedule because the scheduler doesn’t need to find nodes with the requested amount of resources. It will just schedule them onto any node that isn’t under pressure or starved for resources. However, these pods will be the first to be evicted if a node becomes under pressure and needs to free up resources.
    * That’s called best effort quality of service. BestEffort pods can also create resource contention with other pods on the same node and usually it is a good idea to set resource requests.
    * Specifying the resource requests and limits will use the Guaranteed QoS class

##### Summary
* Pods are K8s’ basic building block
* Declare Pods and other resources in manifest files
* Manifest files contain `apiVersion`, `kind`, `metadata` and `spec`
* `metadata` includes name (required) and labels (optional but recommended)
* Pod `spec` include container names and images

---

#### Part 7 - Services

* A Pod is inaccessible apart from other pods in the container network. Even for pods in the container network, it isn’t very convenient to access the web server as it is because pods need to find the IP address of the pod and keep track of any changes to it (remember that k8s will reschedule pods to other nodes, for e.g., if the node fails, in which case the IP will change).
* To overcome all these challenges, there are K8s Services.
* A Service defines networking rules for accessing Pods in the cluster and from the internet.
* Use labels to select a group of Pods.
* Service has a fixed IP address.
* Distribute requests across Pods in the group to balance the load

* Specify the Pod label so the Service can select the Pods with those labels
* Service is assigned a static IP address and Port. Through this the pod can be reached within the container network and from outside of the cluster.

##### Manifest
* `apiVersion` - same as for Pod: `v1`
* `kind` - set to ‘Service'
* `metadata` can contain the same labels as that of the Pod since it’s related to the same application. This isn’t required but it is important to stay organised.
* `spec`
    * `selector`: required, define the labels to match the pods against
    * `port`: required, refers to the value of the pod’s container port
    * `type`: optional, defines how to actually expose the service
        * NodePort allocates a port for the service on each node in the cluster. Usually it’s better to allow k8s to choose a port from the available ports to avoid the chance that the port you specify is already taken. That will cause the service to fail to create

##### Demo
* Cluster - IP - internal IP of the service
* NodePort doesn’t have an external IP
    * The allocated port for the service is commonly between 30000 and 32767

##### Summary
* Services expose Pods via a static IP address
* NodePort Services allows access from outside the cluster

---

Part 8 - Multi-Container Pods
_to be continued..._