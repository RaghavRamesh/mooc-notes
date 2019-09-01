### Module 1 - Course Overview
* * *

### Module 2 - What use Docker as a Web Developer?

* What is Docker?
    * Party line - Lightweight, open, secure platform
    * Simplify building, shipping and running apps in different environments
    * Shipping container system for code / What ships with Docker? Images, containers. Back in the day, there was no consistent way to ship containers. Docker brought standards / consistent way to ship containers
    * Runs natively on Linux or Windows Server
    * Runs on Windows or Mac Development machines (with a virtual machine) or Linux (natively)
    * Relies on “images” and “containers"
    * Role of images and containers
        * An image is used to build a container
        * It will have the necessary files to run something on an OS like Ubuntu or Windows. Then your application framework, db that support that. For e.g., NodeJS or ASP.NET framework files
        * Blueprint that helps with a container going. They are not useful on their own but they can be used to to create an instance of the image. We’ll have images that we can use to create running instance of a container. Containers are where the live applications or db or caching server are run.
    * Image - A read-only template composed of layered filesystems used to share common files and create Docker container instances
    * Container - An isolated and secured shipping container created from an image that can be run, started, stopped, moved and deleted. What’s cool about this technology is it starts and stops really fast.
    * Where does Docker run?
        * It can run on Windows or Linux servers. So if you want to run on a development machine you’re gonna need a virtual machine unless your machine is a Linux machine in which case you can directly run Docker containers on it natively.
        * Docker ships with “Docker client"
            * It integrates with OSes like Linux and Windows
            * Integrates with Docker Engine (Daemon)
            * Integrates with LXC and Windows Server Container Support
            * TODO: insert image in Evernote
            * Think of Docker client as the commands that are given to load the ship, remove things off the ship
            * Docker engine is like the cranes and the people running those that actually do the work of operating the containers
    * Difference between Docker Containers and Virtual Machines
        * Bottom line is you have a copy of the OS for each virtual machine which can be expensive to start and stop. They run well but are bulky. Docker Containers run directly on top of the Host Operating System
        * TODO: insert image in Evernote
* Docker benefits (for web developers)
    * Accelerate developer on boarding (new dev joins)
    * Eliminate app conflicts (across different versions)
    * Environment consistency (across dev, staging, production)
    * Ship software faster
* Docker Tools (available with Docker)
    * 2 roads to choose from
        * Docker Toolbox
            * Came out when Docker first hit the scene (Windows 7/8)
            * Provides image and container tools
            * Virtual Machine
            * Works on Windows, Mac, Linux. Although hard requirement for Windows 7 / 8 but there is a better option for Windows 10, Mac and Linux
            * Will use Docker Machine to integrate with Virtual Box
        * Docker Community Edition (Windows 10 Pro+ or Mac)
            * Provides image and container tools
            * Using Hyper-V (Windows) / Hyperkit (Mac) to run VMs (instead of Virtual Box in Docker Toolbox)
            * Works on Windows, Mac, Linux. Although there are some limitations for this too so choose wisely.
            * Don’t normally need to use Docker Machine
    * Tools
        * Main tool: Docker Client
        * Docker Machine (won’t normally need to use this if on DCE)
        * Docker Compose (orchestrator). Doing orchestration manually can get tedious so this really helps
        * Docker Kitematic (GUI instead of CLI)
        * Virtual Box (only needed if going to install Docker Toolbox)
* Docker in Action (see video)

Module summary
* Docker simplifies building, shipping and running apps
* Runs natively on Linux and Windows servers
* Docker is NOT the same as using VMs
* Key benefits
    * Accelerate developer on boarding
    * Simplify working with multiple apps
    * Consistency between environments
    * Ship faster!

* * *

### Module 3 - Setting up your Docker Environment
- Installing Docker on Mac
- “ On Windows
- Getting started with Docker Kitematic and in action

Installing Docker on Mac
- Followed video and successfully installed 

Getting started with Docker Kitematic
- GUI tool used to provision VMs and work with Images and Containers
- Visually search for Docker images
- Create, run and manage containers
- Good for beginners to understand what’s going on

Docker Kitematic
- Learnt how to search for images in Docker Hub, create, stop and remove them

* * *

### Module 4 - Using Docker Tools
- Getting started with Docker machine - used to interact with the VM
    - _Video. Not necessary if using Docker CE_
- Getting started with Docker client - lets us do the following
    - Interact with Docker Engine
    - Build and manage images
    - Run and manage containers
- Key commands
    - `docker pull [image name]`
    - `docker run [image name]`
        - To run the image in a container. By itself, the image is not very useful unless used somewhere. Will also pull the image if not already available.
    - `docker images`
        - List images.
    - `docker ps`
        - List containers. By default only shows running containers. -a lists all.
    - `docker rm [container id]`
        - Remove container
    - `docker rmi [image id]`
        - Remove image

* * *

### Module 5 - Hooking your source code into a container

Module agenda
- The layered file system
- Containers and volumes
- Source code, volumes and containers
- Hooking a container volume to source code
- Removing containers and volumes

Different ways to run src code on a container but this module focuses on the following

1. Create a container volume that points to the source code
2. Add your source code into a custom image that is used to create a container

##### The layered file system
- There are multiple layers. 
- Image generally contains only read-only layers. E.g. Ubuntu image
- Container layer is a read-write layer. That’s an essential difference. This layer interacts with the image layers. 
- Our own source code, database, log files can either be put in the container in this RW layer or baked in as image layer. 
- File layers can be shared across containers. 

##### Containers and volumes
- RW file layer of containers go away along with the container when deleted. Volumes are useful when you want to retain some files. 
- What is a volume? Special type of directory in a container typically referred to as a “data volume"
- Can be shared and reused among containers
- Updates to an image won’t affect a data volume - they stay separate
- Data volumes are persisted even after the container is deleted

##### Source code, volumes and containers
- Can define a volume and it writes to a directory in the Host and Docker manages that. 
- `docker run -p 8080:8080 -v /var/www node` 
    - -v creates the volume in the Docker Host. Can be customised.
    - /var/www is the container volume alias but actually writes to Host
- `docker inspect mycontainer`
    - Returns a “mounts” field which tells the source (on the Docker Host) and destination (container alias) of the volume
- Ran a node express server in Docker container

##### Removing containers and volumes
- When you run with -v without explicitly specifying where in Docker Host, then docker will manage the volume. It will create a dir in some part of the hard disk
    - Can remove by docker rm -v [container name]
- If you specify the location in Docker host, docker will not delete the volume / source code. 

* * *

### Module 6 - Building Custom Images with Dockerfile

Module agenda
- Getting started with Dockerfile
- Create a custom Dockerfile
- Building a custom image
- Publishing an image to Docker Hub

The second way of getting src code into a container - add your source code into a custom image that is used to create a container

##### Getting started with Dockerfile

Just a text file with instructions in it that the docker build command can take as input and generate a layered FS and create a Docker image
- Text file used to build Docker images
- Contains build instructions
- Instructions create intermediate image that can be cached to speed up future builds
- Used with “docker build” command

##### Key Dockerfile instructions
- FROM - from nothing or a base image - node
- MAINTAINER - author - Raghav Ramesh or email
- COPY - instruction to copy src code to container - . /var/www
- WORKDIR - start working directory - /var/www
- RUN - commands go here -  npm install
- EXPOSE - expose ports - 8080
- ENTRYPOINT - command to start things up - [“node”, “server.js”]
- ENV - envvars
- VOLUME - defining volumes

##### Creating a custom node.js Dockerfile
- _Followed along with the video_

##### Building a Node.js image
- Followed along with the video
- Lots of intermediate containers which will be use for cached builds 

##### Publishing an image to Docker Hub
- `docker push [username]/[imagename]` (using your Docker Hub account)

##### Summary
- Dockerfile is a simple text file with instructions that is used to create an image
- Each Dockerfile starts with a FROM instruction
- Custom images are built using: `docker build -t [username]/[imagename] .`
- Images can be pushed to Docker Hub to make it available for others

* * *

### Module 7 - Communicating between Docker containers

Module agenda
- Getting started with container linking
- Linking containers by name (legacy linking)
- Container linking in action
- Getting started with container networks - bridge, container network
- Container networks in action
- Linking multiple containers

Generally need to talk to multiple containers running different components of a system

##### Legacy linking
- Give a container a name and another container can link to it

Step to link containers
1. Run a container with a name - `docker run -d -—name my-postgres postgres`
2. Link to running container by name - `docker run -d -p 5000:5000 -—link my-postgres:postgres danwahlin/aspnetcore`
3. Repeat for additional containers

##### Getting started with container networks
Another way to connect containers with additional functionality. 
Container network aka bridge network
Need a better way to group containers / isolate group of containers - isolated network
Containers within an isolated network can talk to each other by name

1. Create a custom bridge network and give it a name
    1. `docker network create -—driver bridge isolated_network`

2. Run containers in the network 
    1. `docker run -d —-net=isolated_network -—name mongodb mongo`
    2. No need to do -—link. As long as the containers are within the same isolate network, they can already talk to each other. -—link is in fact not supported in this world.

##### Linking multiple containers

As you start adding more and more containers, it gets tedious to run a lot of commands to connect them all in a network. There is an easier way… Docker Compose

##### Summary
- Docker containers communicate using (legacy) link or network functionality
- The -—link switch provides “legacy linking”
- The —net command-line switch can be used to setup a bridge network
- Docker Compose can be used to link multiple containers to each 

* * *




