Module 1 - Course Overview


Module 2 - What use Docker as a Web Developer?

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











