# Introduction to Docker - Cloud Academy course by Ben Lambert

### Introduction 

#### Part 1 - Course Intro

#### Learning objectives
* What Docker is
* How to create Docker images
* How to map ports between Docker and Host OS
* The basics of Docker networking
* How to use volumes for persistent storage
* How to tag images

#### Agenda
* What is Docker?
* Docker Architecture
* Installation
* Creating Containers
* Images vs. Containers
* Images from the Dockerfile
* Images from Containers
* Port Mapping
* Networking
* Volumes
* Tagging
* Summary

---

### Part 2 - What is Docker?

Docker is a container platform that allows you to separate your application from the underlying infrastructure by bundling up your code and all of its dependencies into a self-contained entity that will run the same on any supported system. 

Picture an actual physical shipping container. You can use a shipping container to transport anything that will fit inside. Since shipping containers come in standard sizes, they're easier to manage because they're consistent. It really doesn't matter what's inside the container because the infrastructure used to transport them is the same.

Challenges in SE: deploying code, managing dependencies, handling rollbacks, etc. All of these things are made more challenging because the development environments are seldom identical to production and this is where Docker containers can help. Docker containers are similar to their physical namesake. 

They allow you to take whatever software you need to run, bundle it up into a consistent format, and run it on any infrastructure that knows how to handle a container. 

Having your code run inside of a container means that it’s isolated from other processes and other containers. So each container can have its own process space, its own network stack, resource allocation, etc. 

Docker containers vs. VMs
* VMs run full OS on top of virtualised hardware whereas Docker containers share the kernel of the Host OS and only needs the additional OS software in the individual containers. For e.g., the Linux kernel is shared among containers running Ubuntu and CentOS
* For this reason, VMs have unnecessary OS files that are not necessary to be unique across the apps running in different VMs. VMs are thus, slow to boot up (in the order of minutes) as opposed to containers (in the order of seconds). 

---

#### Part 3 - Docker Architecture

* Client-server architecture 
* 
* Server
    * docker daemon - responsible for objects such as images, containers and networks
    * Server exposes a REST API that the client consumes over Unix sockets or a network interface
    * The server runs in the docker host
* Client
    * The client is the docker binary - the docker command
    * Client issues commands like “docker pull” which goes to the daemon, which then pulls image from the docker registry

Containers are not new, Docker just made them popular. 

Why did Docker become so popular?
* Docker took a lot of the current container technologies and combined them into one product. 
* Docker came along at a time where people were looking for better ways to build, deploy and run highly scalable, secure apps. 
* So the combination of market demand and well established technologies made it a prime candidate to become synonymous with the term container. 

Docker is actually built on some very well established Linux kernel features, that when combined together, allow processes to be run in isolated environments. 

Linux kernel features that Docker uses 
* Namespaces
    * Docker uses a few different namespaces. They are pid, net, ipc, mnt and uts. 
    * When these are all used together they allow for a very high level of process isolation. 
* Control groups or Cgroups
    * Since containers allow processes to be run in isolation, you can’t have any single process consuming all of the system resources. 
    * E.g., if you’re running 5 containers and one of them consumes all of the system’s CPU the end result is sort of a DoS attach because the other 4 containers are not able to get the CPU that they need to function. So control groups allow for sort of limits to be set on different subsystems, which ensures that the processes aren’t going to be taking more than they should be allowed.
    * Resource limiting, prioritisation, accounting, control
* UnionFS
    * This functionality helps to keep down the overall size of the Docker containers. 
    * A union FS starts with a base image and then can merge in any changes. 
    * When you create a Docker container, you have a starting image, which is a set of files that make up the base image for your container. 
    * As you start making customisations by adding or removing packages, files, directories, etc., those create different layers. Each layer is a set of file changes that the union file system can merge into the previous layer. Because of this layered design you don’t end up with duplicate files because each layer just needs to know the about the changes. 
* Together these features allow Docker to better run your code in isolation

---

### Features
_to be continued…_

