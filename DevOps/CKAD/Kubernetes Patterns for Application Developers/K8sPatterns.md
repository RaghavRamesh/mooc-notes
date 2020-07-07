# Kubernetes Patterns for Application Developers - a Cloud Academy course by Logan Rakai  
## Introduction
### Part 1 - Introduction
#### Agenda
* Multi-container Pod patterns
* Networking topics
* Service Accounts - mechanism for Pods to authenticate themselves in a cluster
* Leveraging `kubectl` 

---

## Kubernetes Patterns
### Part 2 - Multi-container Patterns
* Pods allow you to have multiple containers sharing the same network space and can also share storage between containers
* Often, using a single container is the right choice, but there are several common patterns for when you should use multiple containers 
* 3 patterns
    1. Sidecar
    2. Ambassador
    3. Adapter
* Pods are an extra level of abstraction above containers. Why the abstraction?
    * Containers alone not enough to effectively manage workloads
    * Pods allow specifying additional info such as restart policies, probes to check health of containers 
    * Allow you to seamlessly deal with different types of underlying containers, for e.g., Docker and Rocket
    * Co-locate tightly coupled containers without packaging them as a single-image

#### Sidecar Pattern
* Uses a helper container to assist a primary container
* Commonly used for logging, file syncing, watchers
* Benefits include leaner main container, failure isolation, independent update cycles

##### File Sync Sidecar Example
* Primary container: web server
* Sidecar: Content puller (from a CMS)
* Content Puller syncs with a CMS
* Web server serves the content
* The content is synced using a volume

#### Ambassador Pattern
* Ambassador container is a proxy for communicating to and from the primary container
* Primary container only needs to consider connecting to localhost, while the ambassador controls proxying the connections to different environments
* Commonly used for communicating with databases
* Instead of configuring env vars in the primary container to control db connection, in this pattern, the application can be simplified to always connect to localhost, with the responsibility of connecting to the right database given to the ambassador. 
* In production, the ambassador can implement logic to work with sharded databases as well, but the app in the primary container only needs to consider a single logical database, accessible over localhost
* Another benefit: during application development you can run a database on your local machine without requiring the ambassador, keeping the development experience simple
* The ambassador may also be used my multiple applications written in different languages since this responsibility is removed from the primary container

##### Database Ambassador Example
* Primary container: web app
* Ambassador: database proxy
* Web app handles requests from users
* Db requests are sent to the Db Proxy over localhost
* DP then forwards the requests to the appropriate database, possibly sharding the requests

#### Adapter Pattern
* Adapters present a standardised interface across multiple Pods
* Adapter pattern is the opposite of the Ambassador pattern in that the Ambassador pattern presents a simplified view for the primary container whereas the Adapter pattern presents a simplified view of the application to the outside world
* The pattern is commonly used for normalising application logs, or monitoring data, so they can easily be consumed by a shared aggregation system. The adapter may communicate with the primary container using either a shared volume when dealing with files or over a localhost. 
* The pattern allows adapt an application out without requiring code changes. This may be required when you don’t have access to an application’s source code. 

##### Example
* Application gets metrics from top command but metrics server expects data in a different format
* Adapter pattern: Store the output of the top command in a shared volume. Add an adapter container that reads from this volume, transforms the data in the format that the metrics server expects
* To expose the metrics in the expected JSON format, the web server container can expose a REST endpoint for the metrics server to pull the metrics or the adapter container can push the file contents into a metric aggregation system. 

---

### Part 3 - Networking
#### Agenda
* Networking basics
* Services
* Network Policy

#### Networking Basics
* Pods
    * Each Pod is assigned 1 unique IP in the cluster
    * Containers in the Pod share the same IP and can communicate to each other over localhost
    * Pods are schedule onto Nodes and any Node can reach the Pod using the Pod’s IP address
    * Other Pods located in other Nodes can do the same. This depends on the networking plugin used
    * But Pods are ephemeral and they can be killed and can come back up with different IPs. Can’t rely on a single Pod IP to rely on replication. This is were Services come in. 

#### Services
* Services maintain a logical set of Pod replicas using labels. Pod replicas may be spread over many nodes. 
* The service maintains a list of endpoints as Pods are added and removed from the set. 
* Clients only need to know the IP address of a Service. 

##### Service Discovery
* Pods can discovery services via env vars as long as the pod was created after the service. 
* Or by adding the DNS add-on in the cluster. 
* The DNS can resolve the service name or the namespace qualified service name to the IP associated with the service. 

##### Types of Services
* ClusterIP
    * The IP assigned the service is called the Cluster IP. It’s the most basic type of service. 
    * The Cluster IP is only reachable within the cluster. 
    * The kube-proxy component that runs on each node is responsible for proxying requests to one of the service’s endpoints.
* NodePort
    * Other types of services allow clients outside the service to connect to the service. 
    * First such service type is call NodePort. NodePort causes a given port to be opened on every node in the cluster. The Cluster IP is still given to the service. Any request to any node to that port is routed to the cluster IP. 
* LoadBalancer
    * Allows external access. Exposes the service externally through a cloud provider’s load balancer. 
    * The load balancer type also creates a cluster IP and node port for the service. 
    * Requests to the load balancer are sent to the node port and routed to the cluster IP. 
    * Different features of cloud provider load balancers such as connection draining and health checks are configured using annotations on the load balancer. 
* External name
    * It is different in that it is enabled by DNS, not proxying. 
    * You can configure an external name service with the DNS name and requests for the service return a CNAMe record with the external DNS name. This can be used for services running outside of K8s, such as a database as a service offering.

#### Kubernetes Network Policy
* Rules for controlling network access to Pods
* Similar to security groups controlling access to virtual machines
* Scoped to Namespace

##### Network Policy Caveat
* K8s network plugin must support Network Policy. Otherwise there will be nothing to enforce the created network policies. 
* You might think that you have secured access to the application but the pods are actually still open for requests from anywhere. 
* The Pods can be created successfully with no error message. 
* The cluster admin can tell if your cluster supports network policy or not. 
* E.g., Calico, Romana are plugins that support Network Policy

_to be continued..._


---

### Part 4 - Service Accounts

---

### Part 5 - Leveraging kubectl

---

## Course Summary

### Part 6 - Summary

---
