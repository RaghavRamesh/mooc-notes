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

### Module 5 - Installing K8S

- Minikube
    - `minikube dashboard` to visualize in a browser
    - MK is similar to Docker for Mac / DFW - runs a Linux VM in the local computer
    - MK - single node cluster

- GKE
- AKS
- Manual

* * *

### Module 6 - Working with Pods
- Smallest unit of scheduling in K8S
- Pods:Kubernets :: VMs:VMWare :: Containers:Docker
- Pods are bigger than Containers but are much much smaller than VMs
- Pods can host one or more containers (but use only when they need to be tightly coupled)
- Define the pod in a manifest file and send it to the apiserver (you don’t generally do this. You uses services and replication controllers instead, even if it is only 1 pod)
- Irrespective of how many containers are there in a pod, it gets only 1 IP
- Every network inside a pod shares a single network namespace, “cgroups” limits, volumes, IPC namespaces
- Atomic deployments: No such thing as a half-deployed pod. Pending state until all containers are up and ready. 
- One pod gets scheduled to one node. Cannot be spread over multiple.
- Pods are ephemeral. There is no bringing them back from the dead. You just replace them. Cattle instead of pets.
- Same kubectl create command for all k8s objects with “kind” specified in manifest files

* * *

### Module 7 - Kubernetes Services
- Ok, so we have apps running in pods. How do we access our app?
    - From outside the cluster
    - From inside the cluster
    - Service nails both of these
- REST objects 
- They are an abstraction over pods
- Reliable network endpoint - service IP, DNS, Port never change unlike pods. 
- Requests to service gets load balanced to all underlying pods
- From inside the cluster, the service IP and DNS will work
- From outside a service sets up some networking magic 
- Endpoint object is maintained which keeps track of the pod IPs
- Labels tie up service and pods
    - Updating labels updates which pods the service load balances over

Service discovery
- DNS based (best way) cluster add-on - DNS service on the cluster. Kubelets go to this service for DNS
- Or; Environment variables of pods after creating the services

Service ports are going to be between 30000 and 32767

Different `spec.type`s / `ServiceType`s  - 
- ClusterIP: Stable internal cluster IP
- NodePort: Exposes the app outside of the cluster by adding a cluster-wide port on top of ClusterIP
- LoadBalancer: Integrates NodePort with cloud-based load balancers - highest abstraction (Ingress) - although not all Cloud balancers are created equal

`selector.app` in service yaml file has to match our pod’s replication controller - otherwise cannot tie up service and pods.

Endpoint object
- When we create a service, an endpoint object is created which keeps track of pod IPs when they get created on-the-fly

K8S services in the real world

Updating business apps using labels. 

Services have a subset of the pods’ labels except say, version. So when a new version of the pod is available, a new label is added to the service matching the new version, so service and the old pods are no longer tied up. Notice that the old pods are still there but they are not tied to the service. 

* * *

### Module 8 - Kubernetes Deployments
- Deployments are the future
- K8S deployments are all about rolling updates and simple rollbacks - the whole reason why they exist
- Full on REST objects
- Use the declarative way. `kubectl run` uses the iterative way
- Create them as a YAML / JSON file and give them to the `api server` to deploy
- Wrap around RCs
- RCs in the world of deployment are Replica Sets (video replaces RC above with RS)
- How did we handle deployments before Deployments?
    - Deploy pods through an RC. Then create a new RC with a different name then do kc rolling-update with new rc file. 
    - No proper audit trail
    - Felt like a kludge and a bolt-on
- With Deployments
    - Manifest -&gt; Master -&gt; Deployed to cluster
    - Behind the scenes we get an RS. 
    - Then when you want to push an update, make changes to the same manifest file. Apply the changes using `kubectl apply —record`
    - Then in the background, a new RS is created, then pods are smoothly rolling updated. 
    - Ensure you manage the manifest file in a source control. 
    - The RSs don’t get garbage collected because they are needed for rollbacks

Use —record flag while applying new deployment changes for audit history when you temp 

`kubectl rollout history deployment &lt;deploy-name&gt;`

`kubectl rollout status deployment &lt;deploy-name&gt;` to watch while it finishes rolling

* * *

### Module 9 - What’s Next?
- Go declarative - they are self-documenting and lend themselves to config mgmt repos
- Go Highly Available (H/A) `etcd` and other control plane bits. Makes sense to back them up. 
- Explore more K8S objects - LoadBalancer Services (Ingress)
- Look at the wider ecosystem - CoreOS rkt, Docker, GKE, ...




































