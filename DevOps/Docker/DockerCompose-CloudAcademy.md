# Managing Applications with Docker Compose - Cloud Academy course by Logan Rakai

## Intro

### Part 1 - Course Introduction

#### Prerequisites
* Experience with Docker
* Have Docker installed
* Good IDE

#### Agenda
* Overview
* Docker Compose files
* Docker Compose CLI
* Running web apps
* Building images with Compose
* Multi-environment Compose

#### Learning Objectives
* Understand Compose files
* Configure your applications Compose files
* Use the Compose CLI to manage the lifecycle of your applications
* Build your own images from source with Compose
* Extend Compose to adapt to multiple environments

---

### Part 2 - Docker Compose Overview

#### Agenda
* Motivation
* What is Docker Compose?
* Parts of Compose

#### In a world without Docker Compose
* Example
    * An application consisting of two services that communicate with one another
    * Each service corresponds to a container in the diagram
    * Service A: stateless and should be accessible from the host machine on a port
    * Service B: required to persist data
    * You have Dockerfiles for both
    * Task: spin up a temporary environment with the application running to perform some tests and tear it down when you are finished
* How do you go about doing this?
    * In a world without DC, you might go about achieving that with the following series of Docker commands.
    * You want to follow best practices in isolated your app’s containers from the other containers that running on the Docker host. To give you the most control over that you create a user-defined bridge network:
        * `docker network create —driver bridge app_network`
        * Using a user-defined network gives you access to automatic DNS resolution from container names to IP address that your application might take advantage of
        * For service B
            * `docker volume create serviceB_volume`
        * Now build the Docker images
            * `docker build -f Dockerfile.serviceA .`
            * `docker build -f Dockerfile.serviceB .`
        * Run the containers
            * `docker run -d --name serviceA —network app_network -p 8080:3000 serviceA`
            * `docker run -d —name serviceB —network app_network —mount source=serviceB_volume,target=/data serviceB`
    * Tear down
        * `docker stop serviceA serviceB`
        * `docker rm serviceA serviceB`
        * `docker rmi serviceA serviceB`
        * `docker volume rm serviceB_volume`
        * `docker network rm app_network`
* Discussion
    * 10 Docker commands
    * Relatively not too bad compared to VMs and bare metal
    * Lot of typing involved for a fairly regular task
    * And there could be even more services involved
* Can we do better?
    * Use scripts
    * Still need to know Docker commands and the sequence to put them in
    * Imperative paradigm
    * As an alternative, wouldn’t it be nice to have a declarative approach? And let the tooling figure out what needs to be done
    * Docker Compose!

#### Docker Compose
* Manage multi-containers applications

#### Docker Compose Parts
1. Compose file
2. CLI

#### Compose Files
* Declare services in your applications
* The same 10 commands can be done in 2 commands with docker-compose
    * `docker-compose -f composeFile.yml up -d`

#### Recap
* Challenging to manage multi-container apps
* Compose can help
    * Compose files
    * CLI

---

## Docker Compose Parts

### Part 3 - How to Create Docker Compose Files Using Yaml

#### Agenda
* YAML
* Root elements
* Special topics

#### YAML
* YAML: YAML Ain’t Markup Language
* Data serialisation
* .yaml or .yml file extension
* Extensive

##### YAML vs. JSON
* More concise as it is white-space sensitive
* Wrong space can break file but IDE support should prevent that
* Cleaner look due to lesser characters and see
* Superset of features of JSON
* Supports comments

#### Root elements
* version
* services
* volumes
* networks

##### Compose File Version
* Simple key value pair
    * Value must be a string
* Defines how to interpret the file

##### Compose File Services
* Services in your app
* Nested mapping of services under the services key
* Configuration options in a further nested mapping
* Configuration options are similar to `docker run` properties. There are corresponding service fields for docker run args
* Only required arg is image

##### Compose File Service Caveats
* Some configurations only apply to Swarm mode
* Some are specified on the command line

##### Compose File Dependencies
Because compose supports multi-container applications there are additional args that don’t correspond to docker run commands:
* `depends_on`: provides a way to list dependencies the service depends on
    * If you tell Compose to start a specific service instead of the entire application, Compose can also use the information to automatically start any dependencies of the service.
    * However, it’s important to note that Compose won’t wait for the dependencies to be ready before starting a service.
    * For e.g., Compose can start a db before a web service but it can’t account for the time it takes the database process to be ready to handle connections.
    * Because of this, it’s best practice to write your applications in a way that can tolerate connection failures. If that isn’t an option, you can use scripts that poll the dependencies to wait until they are ready. One such script is called wait-for-it.sh.
* `links`: Links correspond to the link parameter of docker run allowing you to grant access to a container to access an exposed port on a private interface and to provide aliases to reach containers.
* Generally networks are a better way to specify relationships

##### Compose File Volumes
* `volumes:`
* Allows naming and sharing of volumes
* Supports `external` volumes

##### Compose File Networks
* `networks:`
* One created by default using the bridge driver

##### Special Topics: Variable Substitution
* Use shell environment variables in Compose files
* Can use $VARIABLE or ${variable} syntax
* Substitutes an empty string if variable is not set

##### Special Fields: Extension Fields
* Reuse configuration fragments
* File version: 3.4+
* Root keys beginning with x-
* Uses YAML anchors

---

### Part 4 - Features and Commands of Compose CLI

#### Agenda
* Features
* Usage

#### Compose CLI Features
* Run multiple isolated environments
* Parallel execution model
* Compose file change detection (uses cache when necessary)
* Open source

#### Usage
* `docker-compose [OPTIONS] [COMMAND] [ARGS]`
* Connects to Docker Daemon on the host by default
    * Can connect to remote hosts with -H
    * Secure remote connections with —tls* options
* Looks for docker-compose.(yml|yaml)
    * -f to specify alternatives
* Projects to represent isolated apps

#### Commands
* Many generalise familiar `docker` commands
* Two commands introduced by Compose
    * `up`
        * Creates networks and named volumes
        * Builds, creates, starts and attaches to service containers
        * Performs change detection
    * `down`
        * Partial opposite of up
        * Removes service containers, named and default networks
        * Leaves volumes and images by default

---

## Managing Applications

### Part 5 - Deploying and Configuring a Web Application with Compose

#### Example
* Wordpress
* PHP + MySQL database
* Images by Docker

* Use Ctrl-z to exit out of compose without exiting the containers
* Recall that “depends” only guarantees starring order but doesn’t wait for the dependency to be ready to accept connections before starting the container that depends on it
* If not specified, Docker will use the default network to connect the containers specified in the compose file
* Use `restart: always` to ensure that failed containers will restart automatically
* `docker-compose down` removes the containers and network but keeps the images and volumes remaining by default. So when `up` is issued again, the app can resume from its previous state if the app state is stored in the volume
* `docker-compose -f wordpress.yml down —rmi all —volumes —remove-orphans`
    * Remove orphans will delete containers that are no longer specified in the compose file
    * The above command removes everything unlike the default `down` command

---

### Part 6 - Building in Compose

#### How to get a similar development experience as Docker with DC?
* Use Dockerfiles for image build instructions
    * Just like `docker build`
* Compose file `build` key
* `docker-compose up` and `build` can be used to build images

#### Compose File `build` key
* Builds image if present
* Two forms: short and long
    * Short: `build: ./dir`
    * Long:
        * ```
        * build:
        * context: ./dir
        * dockerfile: the.dockerfile
        * args:
            * buildno: 1
        * ```
* `docker-compose up` will build for any services without an image
    * Add `—build` to always build images

---

### Part 7 - How Compose handles and combines multiple files

#### How about when code is ready for production?
* How can we use it for both dev and prod?
* Do we need independent dev and prod compose files?

#### Multiple Compose Files
* Compose can combine Compose files (like kustomize)
    * Base config + overrides
    * Default files
        * `docker-compose.yml`
        * `docker-compose.override.yml`
    * `-f` to specify non-default overrides
    * `config` to view effective configuration (similar to `--dry-run -o` in k8s)
    * `-H` to connect to remote hosts

#### Production considerations
* Remove any `volumes` for source code
* Use `restart: always`
* Don’t specify specific host `ports`. Let Docker choose
* Production mode `environment` variables
* Additional `services`. E.g., monitoring and log aggregation services

---

## Summary

### Part 8 - Summary

* Why Compose? Simplifying the process of managing multi-container apps in Docker
* Anatomy of Compose files: 4 main root elements
    * version
    * services
    * volumes
    * networks
* The Compose CLI
    * `up` and `down`
* Running pre-build web apps - Wordpress example
* Building in Compose - `build`
* Composing for multiple environments - combining config files

---