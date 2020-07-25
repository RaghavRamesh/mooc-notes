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

_to be continued_
