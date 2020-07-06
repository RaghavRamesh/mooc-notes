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

---

### Part 4 - Service Accounts

---

### Part 5 - Leveraging kubectl

---

## Course Summary

### Part 6 - Summary

---
