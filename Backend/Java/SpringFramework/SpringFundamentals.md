# Spring Framework: Spring Fundamentals - Pluralsight course by Bryan Hansen
### Module 1 - Course Overview
_Nothing much to note here_

---

### Module 2 - What is Spring?

* Started out as an IoC container
* Removing hard-coded wiring in app, and using a framework to inject the dependencies
* Evolved from EJB - Enterprise Java Beans. 
* POJO Based / glorified hash map - application context, registry
* AOP / Proxies to get c-c-c out of the way which should make project smaller 
* Built around best practices so there are some design patterns we will end up using like Singleton, Factory, Abstract Factory, Template methods

Problem
Spring 
* Increases Testability
* Increases Maintainability
* Increases Scalability
* Reduces Complexity
* Brings back Business Focus

---

### Module 3 - Architecture and Project Setup
Spring was developed to make the existing tasks easier

Demo - check Projects/pluralsight/conference

Spring is all about removing configuration code from your application

---

### Module 4 - Spring Configuration using Java

Configuration is the place where all the hardcoded dependencies are defined so as to remove the clutter from business logic 

Setter Injection
* As simple as a method call
* Removes the mystery of Dependency Injection
*  Setter Injection simply setting an injection

---

### Module 5 - Spring Scopes and Autowiring

Scopes
* Scope != Pattern
* Spring uses them
* 5 scopes
    * Valid in any configuration
        * Singleton - default
        * Prototype - new Bean per request to container; usually still prefer the Singleton
    * Web-aware Spring projects
        * Request - Bean per every HTTP request. Slightly longer than Prototype. 
        * Session - Bean per HTTP session
        * Global - single Bean per application lifecycle - i.e., until the server is rebooted
* Singleton
    * Single instance per Spring container
* Prototype
    * Opposite of Singleton - one instance per request

Autowired
* This is where people start believing there is some magic going on 
* To reduce configuration of code
* Basically this is an implementation of "Convention over Configuration"

@Bean annotation cannot be used at a class level, only at a method level
@Component annotation, which is basically the same thing, is for class level

---


### Module 6 - Spring Configuration using XML
_Skipping for now_

---

### Module 7 - Advanced Bean Configuration

Bean Profiles
* Selectively apply beans based on profile / env

---








