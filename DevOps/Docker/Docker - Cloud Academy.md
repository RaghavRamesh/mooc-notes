# Introduction to Docker - Cloud Academy course by Ben Lambert

### Introduction

#### Part 1 - Course Intro

##### Learning objectives
* What Docker is
* How to create Docker images
* How to map ports between Docker and Host OS
* The basics of Docker networking
* How to use volumes for persistent storage
* How to tag images

##### Agenda
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

#### Part 2 - What is Docker?

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

#### Part 4: Installing Docker
_skipped_

---

### Features
#### Part 5: Creating and Executing your First Container Using Docker

* `sudo docker run -it ubuntu /bin/bash`
    * The VM that runs the Docker daemon is CentOS.
    * The docker registry (Docker Hub, Docker Store) is where “ubuntu” and “hello-world” comes from
    * Specify a tag to get a specific version for the image in production
    * When the above command runs, it first checks if the image is present locally, otherwise it pulls from the docker registry. Then it stores the image in /var/lib
    * Behind the scenes the “run” command pulls the Docker image. The run command gives the ability to execute a command from inside of the container.
    * To get an interactive shell you get pass “-it”
* Official Docker Registries
    * Docker Hub
    * Docker Store
* `$ docker run`
    * Downloads image if it doesn’t exist locally
    * Runs the given command
* The flags “I” and”t"
    * Create an interactive session

---

#### Part 6: Images vs. Containers

* At high level, the difference between the two is similar to the difference between an executable and a running application.
* Each running application is its own instance and independent of the others.
* The running application is also independent from executable in that changes to the app won’t impact the executable.
* In this analogy, the executable is like an image and the running app is like a container.
* An image is a template that defines how the container will look once it creates an instance.

* Images are built on the concept of layers. There is always a base layer, and there there is some number of additional layers that represent file changes.
* Each layer is stacked on top of the others, consisting of the differences between it and the previous layer. The union file system that we talked about in a previous lesson is used to merge the changes.
* `docker inspect <image-name>` - can be used to inspect the image. Shows most of the info present in the corresponding image json file present in the `/var/lib/images`
* `docker start <container-name>` - to start a previously stopped container
* `docker attach <container-name>` - to attach Docker to the container along with the command with which the container was initially run.
* Every docker run command will create a new container. Each container is independent of the other and is created based on the image contents at the time of creating the container

---

#### Part 7:  Images from the Dockerfile
* Dockerfile is a text file with commands that you can use to create your own image
* `docker container prune` - will remove all stopped containers
* `docker rm <container name>` - to remove specific container
* `docker images` - get all locally available images
*  Dockerfile
    * FROM - allows you to se the image that you want to build on top of. Makes Docker flexible as you can use any of the community images, an image from the store, or your own base image, as the start.
        * “scratch” is a special barebones image. It’s not something one can run the same way we ran “hello world” or “ubuntu” images
        * It’s meant to be used as a minimalist base.
    * COPY - allows copying files from host into a layer of the image.
        * After this command is processed, a new layer of the image is created.
    * CMD - default command to run when the container starts up. There should be one command instruction per Dockerfile, if there are multiple, it will use the last one.
* `docker build .` - looks for Dockerfile in the specified directory
* `docker rmi <image id>` - to remove image
* `docker build -t <repo/image name>` - to specify repo name which will also default tag it to "latest"
    * Tags in Docker have their own structure
* `docker run <repo name>` - runs the image you just created

---

#### Part 8: Images from Containers

* The Dockerfile isn’t the only way to create an image.
* You can also use the docker commit command to commit changes that you’ve made to an existing container. The recommended way is to use Dockerfile though because you get treat that image as code which means you can check it into source control, share it with other developers and you can use it in automated testing to build the image, etc.
* Example
    * Run an ubuntu image
    * Install Python
    * Convert this change to the container into an image using: `docker commit <container-id> ubuntu_python`
    * We need to set the default command so that when this image is run using docker run. So in this case it will run bash and then exit because that was the default command for ubuntu
    * So remove that image ubuntu_python and start over
    * `docker commit —change=‘CMD [“python”, “-c”, “import this”]’ <container-id> <tag-name>`
    * Create container using `docker run ubuntu_python` (the tag name specified above)

---

#### Part 9: Port Mapping

##### Example
* Go web app program with 2 endpoints
* Runs on port 8080 and serves some markup
* Run a container in the background using the -d flag
* Fetch the IP address of the container using the `docker inspect`

* You can either interact with the container by talking to its IP and its port or you can bind it to a port on the host running the container, a.k.a., port mapping
* Docker allows you to do this dynamically or explicitly. The Dockerfile specifies that the container exposes 8080. By default, the expose instruction doesn’t do anything to the host.
* Docker lets you bind the container port to a host port with the publish or publish all flags. Publish all will dynamically map the exposed ports of the container to open ports on the host.
* The -P flag is the short form of publish all. `docker run -d -P webapp`. # 0.0.0.0:32768->8080/tcp. Which means the container binds the web app to port 32768 of the host and that means curling localhost:32768 will return some HTML.
* Use the -p flag (publish) to specify port on the host to bind.
    * `docker run -d -p 3000:8080 webapp`. <hostport>:<containerport>
    * Running the same command again will fail because only one process can be bound to a port at the same time.

---

#### Part 10 - Networking

##### Example
* Base image Ubuntu 16.04 and add some networking tools on top using RUN instruction
* `docker network ls`: shows existing networks. 3 listed. Default by Docker. Scope of this lesson is to cover these 3.

##### Bridge
* Bridge network is the default.
* Whenever you start up a new container and don’t specify a network, it’s going to use the default bridge network.
* Run `ip addr show` to list of interfaces
* You can detach from a running container that you’ve shelled into without stopping it by using “Ctrl+p Ctrl+q"
* Run 3 such containers and run the arp scan command to notice that the 3 containers are present in the same network and have access to each other as well as the outside world (pinging google.com works)

##### Host
* Adds the container to the host’s networking.
* Happens when you bind a port of the container to the host.
* Has no isolation between the host and the container.
* `docker run -d —network=host ubuntu_networking /webapp`
* `docker inspect <container>`
* It will not have an IP because it’s bound to the host
* In host networking whatever ports you open up on the container is going to be bound to the host. So you can directly query localhost:8080 in the host and it will have been bound to the container’s 8080 port and serve the content from the server running in that port.

##### None
* As the name suggests, there’s no network at all, completely isolated
* `docker run -it —network=none ubuntu_networking`
* If you list the network interfaces `ip addr show`, you’ll only see the loopback interface.
* Pinging google.com will fail

---

#### Part 11 - Introduction to Persistent Storage Options in Docker

* The last layer of the image is a writable layer, which means apps that need write access will work just fine.
* However, as soon as you stop the container, whatever you’ve written there is gone, which means the writable layer is not an effective way to handle persistent storage
* There are 3 options for persistent storage provided by Docker
    1. Bind mounts
    2. Volumes
    3. In-memory Storage (tmpfs)

##### Bind mounts
* They have been around for a while
* They work by mounting a file or directory that resides on the host, inside the container
* Remains an effective mechanism that allows you to access files from the host inside the container.
* Once the container stops, the data remains because it lives on the host
* Downsides
    * They aren’t as decoupled from the hosts as you might like
    * Need to know the exact path on the host that you want to mount in the container
* Upside
    * The upside is that this could work well for development, because you don’t need to rebuild the image to access the new source code, so you make changes to your source and it reflects immediately
    * This is how Docker provides DNS resolution to containers by default by mounting /etc/resolv.conf from the host machine to each container
    * Sharing src code or build artifacts. For e.g., mounting the target/ directory into a container and each time you build the Maven project on the Docker host, the container gets access to the rebuilt artifacts
    * If you use Dockerfile for development this way, your production Dockerfile would copy the production-ready artifacts directly into the image, rather than relying on a bind mount
    * When the file or directory structure of the Docker host is guaranteed to be consistent with the bind mounts the containers require
* Use cases
    * Sharing config files from host machine to the containers
* However, the preferred way to handle persistent file storage is with volumes.
* `docker run -it —mount type=bind.src=“/var/logs/demo”.dst=“/logs/myapp” scratch_volume`
* `docker volume ls` will return nothing because you are managing the volume not docker
* Run multiple such containers and you’ll see the logs in the host in the src location

##### Volumes
* Volumes are basically bind mounts except that Docker manages the storage on the host
* So you don’t need to know the fully qualified path to a file or directory
* This makes it easier when working cross platform, because Docker handles the volume
* They aren’t limited to the host file system either, they allow you to use different drivers
* The drivers support the use of external storage mechanisms like Amazon’s S3, Google Cloud Storage and more.
* When you stop a container using volumes or bind mounts, the data remains on the host.
* In contrast, the 3rd storage options, tmpfs, is different.
* `docker run -it —mount type=volume.src=“logs”.dst=“/logs/myapp” scratch_volume`
* `docker volume ls` will return the location of the volume in the host machine
* Run multiple such containers and you’ll see the logs in the host in the src location

##### Temp FS
* It’s a temporary file system, an in-memory file system
* From inside the container, you still interact with the files the same way you would any other file
* The difference is that tmpfs is not persistent because tmpfs allows file system access for the life of the running container, it’s commonly used to hold sensitive information, that includes things like access tokens.
* `docker run -it —mount type=tmpfs.dst=/logs ubuntu`
* No need to specify the src location because tmpfs is not persistent storage and nothing on the host is going to persist the data
* Write temporary data to the /logs directory
* The data is available in the location inside the container until the container is running
* Once stopped the data disappears. So when you stop and start the container, the data won’t be there.
* This type of volume is useful for storing sensitive information

When in doubt on which volume type to use, use Volumes

---

#### Part 12 - Tagging

* Tags provide a way to identify specific versions of an image
* Tags make it easy to deploy a specific version and then if something goes wrong, you can easily roll back to the previous version
* One of the useful features of tags is that an image can have more than one tag.
* This is useful if you want to have version numbers as well as named versions.
* Tags are versatile and are an important mechanism for a solid continuous delivery process

##### How to tag an image?
* You can specify a registry name. If you don’t Docker assumes you want to use its registry by default
* `docker build -t “tag_demo” .`
    * `Successfully tagged as tag_demo:latest`
    * We only provided the image name, so Docker provided the tag of “latest” automatically
* `docker tag tag_demo:latest tag_demo:v1`
    * To set the tag of that image to "v1"
    * `docker images` when you list the images, you will see two entries with the same id one with latest and another with v1 tag. They are just different ways to reference them.
    * If I run `docker run tag_demo` with the image without specifying the tag, it will use the image with “latest” tag by default
    * `docker run tag_demo:v1`. Both these statements are the same for this case.
* Make a change to the image and `docker build -t tag_demo:v2 .`
    * `Successfully tagged as tag_demo:v2`
    * `docker images` will list an image with v2 tag with a different image id.
* `docker run tag_demo:latest`
    * The current latest tag still refers to v1 because we explicitly set the tag of v2 image as v2.
    * `docker tag tag_demo:v2 tag_demo:latest` - now ids will match v2 and latest
* It’s considered a best practice to specify version tags while using an image in your Dockerfile

##### How to push an image to Docker Hub?
* `docker push tag_demo`
    * Throws an error because not yet logged in
* `docker login` after you have created an account with Docker Hub
* `docker push tag_demo:latest`
    * Throws an error because cannot just upload an image with whatever name we want. Need to tag the image with username.
* `docker tag tag_demo:latest sowhelmed/tag_demo:latest`
* `docker images` will have same image id as `tag_demo`
* `docker push sowhelmed/tag_demo`
    * Successfully pushed image to Docker Hub under your username.
    * Anyone now can pull the image you’ve pushed.
* Pulling an image with `latest` tag doesn’t automatically mean you get the latest version as we have seen above unless image author explicitly sets the latest version of the image with the `latest` tag.

---

### Summary

#### Part 13 - Summary

1. What is Docker?
2. Docker Architecture
3. Installation
4. Creating Containers
5. Images vs. Containers
6. Images from the Dockerfile
7. Images from Containers (`docker commit`)
    * Best practices for creating images
        * Keep the image as small as possible
        * Use official public images as base images
        * Ensure images have one purpose
        * Reduce the number of layers
8. Port Mapping
9. Networking - Bridge, Host, None
10. Volumes - Bind, Volume (recommended), TempFS
11. Tagging
12. Summary

----
