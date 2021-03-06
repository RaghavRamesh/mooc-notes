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
* Master nodes run the control plane
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

#### Part 8 - Multi-Container Pods

##### Agenda
* Multi-container pods
* Namespaces
* Pod Logs

##### Kubernetes Namespaces
* Namespaces separate resources according to users, environments or applications
* Role-Based Access Control (RBAC) to secure access per Namespace
* Using namespaces is a best practice

##### Namespace Manifest
* Namespaces don’t require a `spec`. The main thing is the `metadata.name`.
* It’s a good idea to label it as well
* The `kind` is `Namespace`
* Create namespace via `kubectl create namespace` or using `kubectl apply -f <manifest file>`
* When running `kubectl` commands remember to specify the namespace using the `-n` command otherwise the `default` namespace will be applied
* The `-n` command can be specified anytime after `kubectl` word

##### Multi-Container Manifest
* Don’t mention the namespace in the other resource manifests as it makes those manifests less portable because the provided namespace can’t be overridden at the command line
* Not a good idea to put the “latest” tag in the `containers[x].image` as k8s may pull the latest new version of the image without you realising it which may not be compatible with the rest of your application
* To prevent always pulling an image and using an existing image if present you can set the `imagePullPolicy` to `IfNotPresent`. Useful to know this but you’re better of using the specific version of the image.
* When the specific tags are used, the `imagePullPolicy` is set to `IfNotPresent` by default
* To talk to other containers within the pod, use the env vars to configure the host (localhost) and the port
* Containers within a pod share the same IP address and so they can reach one another using the localhost and with the container specific port

##### Logs
* `kubectl logs`
* `-f` to follow logs real time
* `—tail` to specify number of lines from the last at the point the command is entered

There is an issue with the current design for the example. Since k8s can only scale at a Pod level and not at a container level, if you want to scale up one of the containers you will end up scaling up the containers like the Redis container which means each Redis container will maintain its own counter value which is not desirable. So a better design would be to break apart the application into multiple Pods and connecting them to each other using Services. Having said that, it does make sense to sometimes include multiple containers in a Pod - it comes down to how tightly coupled the containers are and if it makes sense to think of them as a single unit.

##### Summary
* Containers communicate over localhost within a Pod
* `kubectl logs` for logs
* Logs record what’s written to standard output and standard error

---

#### Part 9 - Service Discovery

##### Why Services?
* Supports multi-Pod design
* Provides static endpoint for each tier
* Handles Pod IP changes
* Load balancing

##### Service Discovery Mechanisms
1. Environment variables
    * Service addresses automatically injected in containers
    * Env vars follow naming conventions based on service name
2. DNS
    * DNS records automatically created in cluster’s DNS
    * Containers automatically configured to query cluster DNS

##### Demo
* The yaml file allows us to create multiple resource manifests in a single file by separating them with a line that contains 3 hyphens.
* It’s possible to cram all resource manifests into a single file but grouping the manifests as tiers like data tier, support tier, etc. enables us manage them tier by tier.
* Use labels to group resources together
* For a service to select using a label, the label should be specified in a Pod
* In Service, `port.name` field is required to distinguish between different ports. But optional if there is only 1 port
* `ClusterIP` is the default Service type and so `type` is optional if the intended service type is `ClusterIP`
    * It creates a virtual IP inside the cluster for internal access only
* The resources are created in the order they are specified in the manifest file

##### Environment Variables for Service Discovery
* In the Pod manifest the value for the host and port take the following convention
    * IP address: `<all_caps_service_name>_SERVICE_HOST` (Hyphens in service name replaced by “_”)
    * Port: `<all_caps_service_name>_SERVICE_PORT`
    * Named port: `<all_caps_service_name>_SERVICE_PORT_<all_caps_port_name>`
    * For e.g.,
        * `name: REDIS_URL`
        * `value: redis://$(DATA_TIER_SERVICE_HOST):$(DATA_TIER_SERVICE_PORT_REDIS)`
* When using environment variables in the value field need to enclose them in parentheses and precede it with “$"
* In order to use env vars created by k8s for service discovery, the Service must be created before the Pod.
* K8s doesn’t update env vars of running containers, they only get set at startup.
* The service should also be in the same namespace for the env vars to be available

##### DNS for Service Discovery
* K8s will add DNS A records for every service
* Naming convention is as follows:
    * IP address: `<service_name>.<service_namespace>`. No need to convert hyphens to underscores or all caps when using DNS for service discovery
    * Port: needs to be extracted from SRV DNS record and that isn’t something we can use in the manifest so we either have to hard-code the port information or use the SERVICE_PORT env var
    * However, if the service is in the same namespace, then you can simply use the service name
    * For e.g.,
        * `name: API_URL`
        * `value: http://app-tier.service-discovery:8080`
    * The cluster DNS resolves the DNS name to the service IP address.
* It is possible to use the DNS SRV port record to configure the pod on startup using init containers.

##### Summary
* Services for N-tier applications
* ClusterIP Service for internal-only access
* Environment variables and DNS allow Pods to discover Services
* Environment variables for Services available at Pod creation time and the same Namespace
* DNS dynamically updated and across Namespaces. They overcome the shortcomings of env vars. DNS records are added and deleted from the cluster’s DNS as services are added and deleted. The DNS names allow communication with services in other namespaces.
* SRV DNS records are created for service port information.

---

#### Part 10 - Deployments
* Pods should not be created directly, they should be created using higher-level abstractions like Deployments
* Deployments represent multiple replicas of a Pod. The `spec` field contains similar fields as that of a Pod’s manifest
* Describe a desired state that K8s needs to achieve
* K8s master components include a Deployment Controller master component which converges actual state to the desired state

##### Differences between Pod and Deployment manifests
* `apiVersion` - `v1` vs `apps/v1`. Higher level abstractions for managing applications are in their own api group and not part of the core API.
* `kind` is set to `Deployment` as opposed to `Pod`
* `spec` - contains deployment specific settings like `replicas`, `selector`
    * Contains `template` where `template.spec` exactly matches `spec` in a Pod
    * `selector.matchLabels` should overlap with the labels declared in the pod template below
    * `template.metadata` includes labels on the pods. Note that this metadata doesn’t need a `name` because k8s generates a unique name for each pod replica in the deployment.

##### Demo
* `kubectl scale` with `—replicas=5` is similar to editing the replicas value in the Deployment manifest and then running `k apply`
* Replicas replicate pods, not containers inside pods.

##### Summary
* Deployments to manage Pods in each tier
* K8s ensures actual state matches desired state
* `kubectl scale` to scale number of replicas
* Services seamlessly support scaling
* Scaling is best with stateless Pods

Instead of scaling arbitrarily we may want to automatically scale up our resources based on some metrics

---

#### Part 11 - Autoscaling

* Scale automatically based on CPU utilisation (or custom metrics)
* Set target CPU along with min and max replicas
* Target CPU is expressed as a percentage of the Pod’s CPU request
* Min. and max. Replicas limits are respected irrespective of the metric percentage

##### Metrics and CPU requests
* Autoscaling depends on metrics being collected
    * Metrics server is one solution for collecting metrics. There are other solutions k8s integrates with to collect metrics
    * Several manifest files are used to deploy Metrics Server (https://github.com/kubernetes-incubator/metrics-server)
    * We need to use the Metrics Server up and running before we can use Autoscaling.
    * Once the metrics server is running, autoscalers can fetch the metrics using the k8s metrics API
    * You can confirm that the metrics server is doing its thing by watching the `kubectl top pods` command. It lists the CPU and memory usage of the pods in the namespace. This command can be used to debug resource utilisation issues
* The other thing that the autoscaler depends on is having a CPU request in the deployment’s pod spec.

##### Autoscaler Manifest
* Autoscaler a.k.a. horizontal pod auto-scaler, is just another resource in k8s
* The `HorizontalPodAutoscaler` kind is a part of the `autoscaling/v1` api.
* Its `spec` includes the `minReplicas` and `maxReplicas`, the `targetCPUUtilizationPercentage` field sets the avg. CPU percentage across the replicas.
* If target CPU utilisation is 70%, k8s will reduce the no. of replicas if the CPU util goes below 63% and increase if 77%. Default tolerance is 10%.
* The `spec` also has the `scaleTargetRef` field that identifies what it’s scaling.
* `kubectl autoscale` command also achieves the same result
    * `kubectl` accepts short-hand notations for resources, we can use `kubectl api-resources` for a full list of shorthand notations
* `kubectl edit` - combines modifying, saving and applying the manifest into one command
    * It directly edits the server-side version of the manifest file
    * In general, it makes sense to edit the local manifest file so that the changes can be checked-in to source control
* HPA checks about every 15 secs whether it should scale

##### Summary
* Autoscaling depends on metrics being collected
* Pods must have CPU requests
* `HorizontalPodAutoscaler` (hpa) is configured with target CPU, and min and max replicas
* Once it’s created, k8s does the heavy lifting to dynamically scale the resources
* `kubectl apply` for applying resources rather than deleting and creating
* `kubectl edit` to edit and apply

---

#### Part 12 - Rolling Updates and Rollbacks

##### Rollouts
* Rollouts update Deployments
* Any change to a Deployment’s `template` triggers a rollout
* Different rollout strategies are available

##### Rolling Updates
* Default rollout strategy
* Update replicas in groups rather than all-at-once
    * This allows service to continue uninterrupted while the update is being rolled out
* However, you need consider that during the rollout the pods will be using both the old and the new configuration and the app should gracefully handle that
* Alternative is the recreate strategy - incurs downtime for the application
* Scaling is not a rollout. Recall that the number of replicas is not a part of the deployment’s `template`
* `kubectl` has commands to check, pause, resume and rollback (undo) rollouts

##### Demo
* As part of rolling update strategy, `maxSurge` and `maxUnavailable` fields control the rate at which updates are rolled out.
    * `maxSurge` specifies how many replicas over the desired total are allowed during a rollout. A higher surge allows new pods to be created without waiting for old ones to be deleted.
    * `maxUnavailable` controls how many old pods can be deleted without waiting for new pods to be ready. The defaults are 25%.
    * You may want to configure them if you want to trade off the impact on availability or resource utilisation with the speed of the rollout. For e.g., you can have all the new pods start immediately but in the worse case you could have all the new pods and all the old pods consuming resources at the same time effectively doubling the resource utilisation for a short period.
* Trigger a rollout by updated a `template` field of the deployment
* `kubectl rollout status` streams progress updates in real time
* `kubectl rollout pause` pauses ongoing rollout
* `kubectl rollout resume` picks up right where it left off and goes about its business
* `kubectl rollout undo` to rollback to the previous version
* `kubectl rollout history` to see the previous revisions and then specify revision to rollback to with the `—to-revision` option

##### Summary
* Rollouts are triggered by Deployment `template` changes
* Rolling update is the default rollout strategy
* Rollouts can be paused, resumed and undone

Rollouts depend on container status. K8s assumes that created containers are immediately ready and the rollout should continue. This does not work in all cases. We may need to wait for the web server to accept connections. This is where probes and init containers come into the picture.

---

#### Part 13 - Probes

* In the previous lesson and demo, K8s assumed that a pod was ready as soon as the container started. That isn’t always true, for e.g., if the container needs time to warm up K8s should wait before sending any traffic to the new pod.
* It’s also possible that pod is fully operational but after some time it becomes non-responsive, for example if it enters a deadlock state. K8s shouldn’t send any more requests to the pod and would be better off to start a new pod.
* K8s provides probes to remedy both the above situations.
* Probes are sometimes referred to as health checks.

##### Readiness Probes
* Used to check when Pod is ready to serve traffic / handle requests
* Useful after startup to check external dependencies
* Readiness Probes set the pod’s ready condition. Services only send traffic to ready Pods

##### Liveness Probes
* Detect when Pod enters a broken state and no longer serve requests
* Kubernetes will restart the Pod for you - key difference between readiness and liveness probes. We have to decide which to use for what based on what course of action we want k8s to take
* Declared in the same way as readiness probes
* Common situation: containers may be live but not ready

##### Declaring Probes
* Probes can be declared in a Pod’s containers
* All container probes must pass for the Pod to pass
* Probe actions can be a command that runs in the container, an HTTP GET request, or opening a TCP socket
    * HTTP request is considered successful if response code is [200,399]
    * TCP request is considered successful if a connection can be established
* By default probes check containers every 10 seconds

##### Demo
* Data Tier (Redis)
    * Liveness: Open TCP socket
    * Readiness: `redis-cli ping` command
    * Container may be live but not yet ready
* App Tier (Server)
    * L: HTTP GET /probe/liveness
    * R: HTTP GET /probe/readiness
* By default 3 sequential probes should fail - so there is some buffer bulit-in. K8s won’t immediately restart if the first probe fails
* `initialDelaySeconds` for first probe. In general, use a higher delay for liveness probe than readiness probe
* Types of probes: `tcpSocket`, `httpGet`, `exec` and `exec.command`
* If things go awry, you can use a combination of `describe` and the `logs` command to debug what’s wrong
* Unfortunately, failed probe events don’t show in the events output in describe, but you can use the pod restart count as an indicator of failed liveness probes

##### Summary
* Container readiness probes monitor when Pods are ready to serve traffic and are temporarily out of service
* Container liveness probes monitor when Pods have entered a broken state and should be restarted
* Probe actions include commands, HTTP GET requests, and opening TCP sockets

Probes kick in after containers are started. If you need to start or prepare things before the container starts there is a way to do that as well - init containers.

---

#### Part 14 - Init Containers

##### Motivation for Init Containers
* Sometimes you need to perform tasks to check for pre-reqs before a main container can start - e.g., waiting for a service to be created, etc.
* The code that performs that could be crammed into the main container image but it’s better to separate the initialisation wait logic from the container image
* Initialisation is tightly coupled to the main application and so belongs in the Pod
* Init containers allow you to run initialisation tasks before starting the main containers.

##### Init containers
* Pods can declare any number of init containers
* Init containers run in order and to completion. Then the main containers in the Pod can start
* Init containers can use their own images different from that of the main container
* Easy way to block or delay starting an application until some preconditions are met. They are similar to readiness probes in that sense but they are only run at Pod startup and can perform other useful work
* All the above features make init containers a vital part of the k8s toolbox
* They are run every time a Pod is created. They are run once for every replica in a deployment. If the Pod restarts due to a failed liveness probe, the init containers would run again as part of the restart. Thus you have to assume that init containers run at least once. Thus you have to ensure init containers are idempotent

##### Demo
* E.g. use case of init container - add an init container to our app tier that will wait for Redis before starting any application servers
* Init containers have the same fields as regular containers except for `readinessProbe` because they must run to completion before the state of the Pod can be considered ready.

##### Summary
* Init containers allow you to perform tasks before main application containers have an opportunity to start
* Useful for checking preconditions and preparing dependencies

---

#### Part 15 - Volumes

* Containers in a Pod share the same network stack but each has its own file system
* Sometimes useful to share data between containers in a Pod
* Lifetime of container file systems is limited to the container’s lifetime
* Can lead to unexpected consequences if a container restarts

##### Agenda
* How to handle non-ephemeral data - allowing us to separate data from containers
* Deploy a persistent data tier
* Options available for storing data

##### Options available for storing data
* 2 high-level storage options: Volumes and Persistent Volumes
* Used by mounting a directory in one or more containers in a Pod
* Pods can use multiple Volumes and Persistent Volumes
* Difference between Volumes and Persistent Volumes is how their lifetime is managed - one depends on lifetime of a particular Pod and the other is independent of the lifetime of Pods.

##### Volumes
* Volumes are tied to a Pod and their lifecycle
* Share data between containers in a Pod and tolerate container restarts
* Although technically possible, consider using Volumes for non-durable storage that is deleted with the Pod
* Default Volume type is `emptyDir` - creates an initially empty directory on the node running the Pod to back the storage used by the Volume.
* Any data written to the directory remains if a container in the Pod is restarted.
* Once the Pod is deleted, the data in the Volume is permanently deleted.
* Note that since the data is stored in a specific node, if a node is rescheduled to a different node, the data will be lost.
* If the data is too valuable to lose when a Pod is deleted or rescheduled you should consider using Persistent Volumes

##### Persistent Volumes
* Independent of Pod’s lifetime and are separately managed by K8s
* Pods must claim Persistent Volumes to use throughout their lifetime
* Persistent Volumes will continue to exist outside of their pods. They will continue to exist after the Pods that have claimed them have been deleted.
* Can be mounted by multiple Pods on different Nodes if underlying storage supports it
* Can be provisioned statically in advance by the cluster admin or dynamically on-demand for more flexible self-serve use cases
Pods must make a request for storage before they can use a persistent volume

##### Persistent Volume Claims
* Describe a Pod’s request for Persistent Volume storage
* Includes how much storage, type of storage, and access mode
* Access mode can be read-write once, read-only many or read-write many.
* PVC stays pending if no PV can satisfy it and dynamic provisioning is not enabled
* Connects to a Pod through Volume of type PVC

##### Storage Volume Types
* Wide variety of volume types to choose from
* Use PVs for more durable storage types
* Supported durable storage include GCE Persistent Disks, Azure Disks, Amazon EBS, NFS and iSCSI

##### Demo
* Deployment, PV and PVC manifests are typically in a single file as they go together
* `accessMode` can only be one of the 3 modes
* The PVC spec outlines what it is looking for in a PV.
* For a PV to be bound to a PVC, it must satisfy all the constraints in the claim.
* Specify `volume` in deployment manifest with the `persistentVolumeClaim.claimName` corresponding to the PVC name
* The `volumeMounts[x].mountPath` in the deployment `spec` can be different for different containers even if the volume is the same
* To confirm the claims request is satisfied by the PV, `Status: Bound` is present in the describe output.

##### Summary
* PVs are independent of the lifecycle of the Pods
* `kubectl exec` runs commands in Pod containers

---

#### Part 16 - ConfigMaps and Secrets

##### Motivation for ConfigMaps and Secrets
* Until now all container configuration has been in the Pod spec
* This makes it less portable than it could be
* If sensitive information such as API keys and passwords is involved it presents a security issue

##### ConfigMaps and Secrets
* Separate configuration from Pod specs
* Results in easier to manage and more portable manifests
* Both are similar but Secrets are specifically for sensitive data
* Secrets reduce the risk of their data being exposed. However, the cluster administrator also needs to ensure that all the proper encryption and access control safeguards are in place
* There are specialised types of Secrets for storing Docker registry credentials and TLS certs
* We will focus on generic secrets

##### Using ConfigMaps and Secrets
* Data stored in key-value pairs
* Pods must reference ConfigMaps and Secrets to use their data
* References can be made by mounting Volumes or setting environment variables

##### Demo
* ConfigMap
    * `kind: ConfigMap`
    * There is no `spec`. Only a `data` field with a single `config` key. More than 1 key value pair for `data` is possible
    * Reference the above config map as a volume in deployment -  `spec.volumes[x].configMap`. You can access specific key value pairs defined in the ConfigMap
        * Then specify the volume under `volumeMounts` in the necessary container
    * Config values can be dynamically changed and they will be picked up by k8s and the mounted volumes will be updated
    * Since the deployment’s `template` is not updated, it does not trigger a rollout though. To do this use the `kubectl rollout restart` to restart the appropriate deployment
* Secret
    * `kind: Secret`
    * Along with `data`, there is a `stringData` field available in Secrets.
    * K8s automatically decodes the base64 encoded strings when used in a container
    * It’s encoding, not encrypting, so it doesn’t provide any security. So treat the encoded strings as sensitive info as well.
    * Don’t check-in secret manifests in source control
    * `stringData` allows you to specify secrets without first encoding them since k8s encodes fields under `data` for you
    * `stringData` fields are not actually stored in the api server which can be verified when you `edit` the Secret
    * You need to restart rollout for the env vars referencing the secret to take effect

##### Summary
* ConfigMaps and Secrets separate configuration data form pod specs or what would otherwise be stored in container images
* Both store data as key-value pairs
* Secrets are for sensitive data
* Referenced via Volumes or env vars

---

### The Kubernetes Ecosystem

#### Part 17 - Kubernetes Ecosystem

* Vibrant ecosystem around the core of K8s
* Helm
    * K8s package manager
    * Packages are called charts and are installed on your cluster using Helm CLI
    * Helm charts make it easy to share complete apps
    * Search Helm Hub for public charts
    * E.g., use pre-existing templates / charts instead of building from scratch
* Kustomize
    * Customise YAML manifests in K8s
    * Helps you mange the complexity of your applications. Obvious use case is env-specific deployments
    * Works by using a kustomization.yaml file that declares customisation rules
    * Original manifesto are untouched and remain usable
    * E.g. use cases
        * Generating ConfigMaps and Secrets from files
        * Configure common fields across multiple resources
        * Apply patches to any field in a manifest
        * Use overlays to customise base groups of resources
* Prometheus
    * Open-source monitoring and alerting system
    * A server for pulling in time series metric data and storing it
    * Inspired by an internal monitoring tool at Google called borgmon
    * De facto standard solution for monitoring K8s
    * Prom + K8s
        * K8s components supply all their own metrics in Prom format
            * Many more metrics than Metrics Server
        * Adapter available to autoscale using metrics in Prom rather than CPU utilisation
        * Commonly paired with Grafana for visualisation
        * Define alert rules and send notifications
        * Easily installed via Helm chart
* Kubeflow
    * Makes deployment of machine learning workflows on K8s simple, scalable and portable
    * A complete ML stack
    * Leverage K8s to deploy anywhere, autoscale, etc.
* Knative
    * Platform for building, deploying and managing server less workloads on K8s
    * Can be deployed anywhere with K8s, avoiding vendor lock-in
    * Supported by Google, IBM and SAP

---

### Course Conclusion

#### Part 18 - Conclusion
* What K8s is
* K8s Architecture
* K8s in Practice
    * Example kubectl commands
    * K8s terminology
    * N-tier apps and Service discovery
    * Deployment rollouts
    * Monitoring containers with probes
    * Preparing Pods with init containers
    * Persistent data storage
    * Separating config and sensitive data using ConfigMaps and Secrets
* K8s Ecosystem

---