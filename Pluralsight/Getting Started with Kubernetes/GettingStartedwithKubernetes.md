# Getting Started with Kubernetes - PluralSight course by Nigel Poulton

### Module 2 - Course Introduction

* Started in Google
* Predecessors were Borg and Omega, but K8S was built from the ground up and is not exactly the next version of these two
* Kubernetes - name comes from Star Trek reference 7th Borg was a woman Borg. Greek. Another nautical reference.

***

### Module 3 - What is K8S?

Containers bring scalability challenges
Platform agnostic
Lets you target deployments

***

### Module 4 - K8S architecture

* Big picture view
    * An orchestrator for micro service apps / containers.
    * Package up your app, containerise it and give it to the K8S cluster using a Deployment (manifest file)
    * Bunch of masters = control plane and nodes - run the actual work
* Masters
    * Master only cares if its immediate abstraction layer is Linux, further down it doesn’t care
    * Run apps on nodes and not on master so that it can do the job of making sure nodes doing work properly
    * `kube-apiserver` (tends to be confused with the master because this is the interface point where the commands and queries come into)
        * front-end to the control plane
        * Exposes the API
        * Consumes JSON (via manifest files)
    * Cluster store
        * Persistent storage
        * Cluster state and config are stored here
        * Uses `etcd` - open source distributed key-value store
            * Distributed, consistent, watchable
            * Source of truth for the cluster
            * Have a backup plan
    * `kube-controller-manager` (could change)
        * Controller of controllers
        * Endpoints controller
        * Namespace controller
        * Helps maintain _desired state_
    * `kube-scheduler`
        * Watches `api server` for new pods and assigns work to nodes
        * Affinity/anti-affinity
        * contraints
        * resources
        * ...
* Nodes a.k.a. “Minions” - much more apt name because minion is an unimportant faceless figure that does what the master says and if it doesn’t it gets swapped out
    * Kubelet
        * Main k8s agent on the node (fair to say that it _is_ the node)
        * Registers node with cluster
        * Watches `api server` for work assignments
        * Instantiates `pods`
        * Reports back to the `master` - not responsible for spawning a new node
        * Exposes endpoint on :10255 on nodes lets you inspect aspects of the Kubelet
            * `/spec` and `/healthz` and `/pods` and more...
    * Container Engine
        * Does container management
            * Pulling images
            * Starting / stopping containers
            * ..
        * Pluggable
            * Usually Docker
            * Can be rkt
    * `kube-proxy` - network brains of the node
        * Pod IP addresses
            * All containers in a pod share a single IP. If multiple containers, then need to use different ports
        * Load balances across all pods in a `service`
* Declarative Model and Desired State
    * Manifest files - record of intent which constitute what we call as the desired state of the cluster. It’s possible for the actual state of the cluster to diverge from this, then k8s will rectify and doesn’t rest until it is back to desired state
    * We don’t / shouldn’t interact with k8s directly to achieve desired state, we should let it do the job
    * Declarative
        * Self documenting
        * Spec-Once deploy-many
        * Versioned
        * Simple rolling updates and rollbacks
* Pods
    * Pods are to K8S as VMs are to VMWare and Containers are to Docker
    * Containers always run inside of pods
    * Pods can have multiple containers (advanced use-case)
    * Ring-fenced environment / sandbox for containers
        * Network stack
        * Kernel namespaces
        * ...
        * All containers in a pod share the pod environment
        * Only put two or more containers in a pod when you need tight coupling between them. Cuz if you want to scale, need to scale entire pods.
        * Also, entire pod exists in one container
        * Pods are ephemeral - pending, running, succeeded/failed. Cattle vs pets
    * Deploying pods
        * Usually via higher level objects like deployment.yml. But can also deploy directly by providing the master with the pod’s manifest file
        * Also, via Replication Controller, but being replaced by Deployment
* Services
    * Pods are ephemeral and their IPs hence keep changing. So can’t rely on them. This is where services come into play.
    * Service is just a K8S object (also specified using a manifest file) that abstracts away the pods.
    * Also load balances requests coming into the pods.
    * Labels rock! They tie up services and pods. That’s how services decide how to load balance over its pods.
    * Only send to healthy pods
    * Can be configured for session affinity - not set by default
    * Can point to things outside the cluster
    * Random load balancing
    * Uses TCP by default (but UDP is supported as well)
* Deployments - they are the future (replacing Replication Controllers)
    * They are all about declarativeness. You tell the cluster how you want your deployment to look like and the cluster takes care of the hard work. If somehow, the actual state deviates from the desired, then k8s gets all-hands-on-deck until the desired state is achieved.
    * REST objects
    * Deployed via YAML or JSON manifests
    * Deployed via the `api server`
    * Add features to Replication Controllers (Replica Sets - next gen replication controllers)
    * Deployment strategies
        * Rolling update
        * Canary releases
        * Blue-green
* Recap
    * K8S is all about orchestrating containerised apps
    * Only dependency is Linux - underneath that can be anything
    * Master (control plane) and nodes
        * Multi-master HA is definitely a thing - may happen soon in the future
        * `api server` - front-end to control plane
        * Cluster Store - state and config of cluster lives here (etcd) - only stateful part of the control plane
    * Nodes / minions - all the work happens here.
        * Node stack is way more simpler than the Master stack
        * Kubelet - main k8s agent
        * Container engine
        * `kube-proxy`
    * Deployment manifest file is given to Master and it takes over from there
    * Every node has its own set of system pods for logging, health checking,etc.
    * Objects in the k8s API
        * Pods - atomic unit of scheduling
        * Replication Controllers - scale pods, desired state, etc.. but being pushed aside / replaced by Deployments
        * Deployments - Replication Controllers + rolling updates, rollbacks...
        * Services - stable networking and load balancing

***






































