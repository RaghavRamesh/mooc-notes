# Spring Boot Fundamentals - Pluralsight course by Kesha Williams

### Module 1 - Course Overview

_Nothing much to note_

---

### Module 2 - Introducing Spring Boot and its Benefits

* Extension of Spring Framework with less code by removing much of the boilerplate code and configuration that characterises Spring

##### Overview
* Features of Spring Boot
    * Spring Boot Starters that give you a jump start
    * Auto configuration
* Bootstrapping an application
    * Spring Initializr
* Data access
* Spring MVC
* REST API dev
* GraphQL servers
* Spring Boot Actuator
* Testing

##### Spring Boot Overview
* Game changer for the Spring framework
* Makes it super easy to run simple web apps
* _TODO: insert picture from Evernote_
* Configuration challenges
    * Complaints of too much configurations
    * The structure of a web application is pretty standard - in other words boilerplate. Spring Boot helps in eliminating this boilerplate
    * _TODO: insert picture from Evernote_
    * Assists with config and helps with dependency management
    * Emphasises convention over configuration
    * Smart enough to choose dependencies for you and auto configure it for you
    * No longer need to download separate web servers and deploy WAR files

##### Features
A lot of magic happens in the background
* Automatic configuration
    * Time saving feature. 
    * Configures application based on libraries. 
    * Maven dependencies configured automatically.
* Starter dependencies
    * One stop shop or a starter pack that includes all of the dependent libraries we need
    * E.g. "spring-boot-starter-web” will configure Spring MVC, REST, Tomcat and Jackson. Similarly for other starters
* Spring Boot CLI
    * Command line interface
    * Entire app can be written using Groovy. Just need to create a groovy file with config
    * Useful for rapid prototyping
* Actuator
    * Allows to see what’s going on 
    * Monitor running application 
    * Manage via HTTP endpoints or JMX
    * Health status, metrics, loggers, audit events, HTTP trace

---

### Module 3 - Bootstrapping a Simple Application 

##### Overview
* Spring Initializr
* Automatic configuration
* Annotations
* Profiles

##### Spring Initializr 
* “pom.xml” dependencies of a SB project
    * All Spring Boot projects have the spring-boot-starter-parent parent dependency
        * Parent “pom s" allow you to manage versions of multiple child modules at once
    * “spring-boot-starter" - core starter which includes auto configuration support, logging, yam support, etc.
    * “spring-boot-starter-test” - “spring-test’, “junit", mockito, ham crest and other useful testing frameworks
    * “spring-boot-starter-web” - if added. 
* Main 
    * Launch class for SB app 
    * Delegates to SpringApplication class by calling run
    * @SpringBootApplication 
        * Bootstraps application by starting Spring 
        * Creates an instance of Spring’s application context, cmd line properties, loads spring beans, etc. 

_Follow along code_














