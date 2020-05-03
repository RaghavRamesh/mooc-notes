# Microservice Fundamentals - Plurasight course by Mark Heath

### Module 1 - Course Overview
* What microservices are
* Building microservices
* Defence in depth principle

---

### Module 2 - Introducing Microservices

Microservices is an architectural style that has rapidly grown in popularity over the last decade. And it has the potential to solve problems in modern distributed software applications like being resilient to errors, having a flexible architecture that can adapt to new requirements, having a good performance and to keep data secure. 

#### Overview
* What are “microservices”? Its benefits, the problems it solves
* Limitations of “monoliths"
* Challenges of microservices
* Sample application 

#### Later in the course
* Architecting microservices
* Practicalities in building microservices - how to debug and test
* Communicating between microservices - synchronous and asynchronous communications and how they relate to overall system reliability and performance
* Securing microservices
* Delivering microservices - how to deploy them and monitor them effectively

#### What are microservices?
An architectural style where autonomous, independently deployable services collaborate to form an application / software system. 

#### Why do we need microservices? The Problem of Monoliths
* Monolith applications typically have all of its code in a single code base where all developers collaborate on the same code base
* Single process for building artifacts and deployment
* Single host
* Single database
* Consistent technology across the code base
* Benefits of monoliths
    * Simplicity 
    * One code base
        * Easy to find things
    * Deployment
        * One application to replace
    * Monoliths are not inherently wrong. If it’s working well for you, that’s probably a sign that you don’t need microservices. Or you don’t need microservices yet. 
* Problems
    * Scale
        * Monoliths can work well for small applications with small teams working for a small period of time - for e.g. a small website that handles a modest number of visitors. 
        * 20 - 30 people serving large number of users manipulating massive amounts of data, starts to become a problem. Larger codebases become harder to maintain. Susceptible to growing tech debt and entangled modules. 
    * Difficult to deploy - even a single line change will require building the entire artefact which is risky, requires downtime - something that’s becoming unacceptable in cloud services era that are expected to maintain high availability
    * Difficult to scale - scaling to large amounts of data is also difficult. Unless great care is ensured to make your monoliths stateless, you can’t scale it out horizontally. Which means the only option is to scale vertically which is expensive because monoliths require the entire application to be scaled up. Rather than being able to scale up the individual components that require greater processing power. 
    * Wedded to legacy technology - find it easier to be stuck with legacy tech. Whatever tech stack you originally built the code base with is going to get very hard to get away from as you have to upgrade the entire application to move to a newer framework. This reduces the agility to adopt new patterns and practices and deters the ability to take advantage of innovation such as new tools and frameworks
    * Distributed monoliths - Btw, just because we have an architecture based on several services doesn’t mean that you’re using microservices. The services could be tightly coupled requiring that they all need to be changed and deployed together. For e.g., sharing a database. This is a big problem because they combine all the down sides of monoliths and the down sides of microservices. 

#### The Benefits of Microservices

* Small services
    * By breaking our monolith into smaller pieces, each service can be owned by a team
    * Much easier to understand and work on
    * Can be rewritten
* Technology Choice
    * Adopt new technology
    * Flexibility to use the right tool
    * Standardise where it makes sense
* Individual deployment
    * Lower risk 
    * Minimize downtime
    * Frequent updates
* Scaling
    * Scale services individually
    * Cost-Effective than scaling a monolith
* Agility
    * Adapt rapidly
    * Easier reuse - light-weight and de-coupled

##### The Challenges of Microservices
* Developer Productivity
    * How can we make it easy for developers to be productive working on the system?
* Complex interactions
    * Makes it hard to understand the system as a whole
    * Take care to avoid inefficient, chatty communications between microservices. Easy to become less performant
* Deployment
    * Many microservices => you will need to automate the process
* Monitoring
    * We need a centralised place to check logs and monitor for problems

Good news is that there is a growing body of tools and processes to help succeed with microservices and overcome the above challenges

---

### Module 3 - Architecting Microservices

##### Overview
* Evolving towards microservices
* Microservices are autonomous
* Microservices own their own data
* Microservices are independently deployable
* Identifying suitable microservice boundaries

##### Evolving towards microservices
* Augment a monolith
    * Add new microservices
* Decompose a monolith
    * Extract microservices
* You don’t need to start with microservices - in fact some may argue that you shouldn’t start with microservices
    * It’s hard to get service boundaries right
* Defining microservice responsibilities
    * Public interface

##### Microservices own their own data
* Avoid sharing a data store - each should have its own data store. 
* Any other microservice needing to access this data should do so via the public API exposed by the microservice that owns it
* Limitations
    * Database joins are not longer possible. Will have to make separate calls to dbs
    * Can’t update data using a single transaction. Will either have to use distributed transactions which are very complex to implement or more commonly design our system to working with eventual consistency
* Mitigating data ownership limitations
    * Define service boundaries well
        * Minimise the need to aggregate data
    * Caching 
        * Cache data owned by another microservice
        * Improved performance
        * Improved availability
    * Identify “seams” in the database
    * Duplication of data across microservices may not necessarily be a bad thing (denormalisation)

##### Components of a microservice
* Microservices can consist of more than one process
* _TODO: insert image from Evernote_
* There may be several different hosts and processes involved in a microservice. Conceptually they form a single microservice. 
* The important thing is a microservice should have a clearly defined boundary and a defined public interface and its data can only be accessed via this interface. 

##### Microservices are independently deployable
* Microservices can be upgraded without their clients needing to upgrade
* This means that you should avoid making breaking changes to the public interface. 
* Microservice contracts
    * Make additive changes
        * New endpoints
        * New properties on DTOs
    * Introduce version 2 API
        * Version 1 clients must still be supported
    * Easily forgotten in development
* Avoiding upgrade issues
    * Team ownership of microservices
        * Clients must ask for a new capability, then add it
        * Then, deploy updated microservice
        * Later, update clients to make use of the new feature
    * Create automated tests
        * Ensure that older clients are supported
        * Run as part of a CI build process
    * Beware of shared code
        * Can result in tight coupling

##### Identifying Microservice Boundaries
* Getting it wrong can be costly - so it’s worth to spend some time 
    * Poor performance
    * Hard to change 
* Start from an existing system - most likely there will be some existing modularisation
    * Identify loosely coupled components
    * Identify database seams (are there databases and tables that conceptually belong together?)
    * Organise microservices around business capabilities - domain driven design
* Domain driven design
    * Identify “bounded contexts"
        * Break up large domains
        * “Ubiquitous language” and relating to one specific business case with models applicable within the bounded context
        * Microservices do not share models
        * e.g. OrderItem and CatalogItem 
    * Sketch your ideas on a whiteboard
        * Run them through real-world use cases
        * Identify potential problems 
* Microservice Boundary Pitfalls
    * Don’t turn every noun into a microservice
        * Anaemic CRUD microservices
        * Thin wrappers around databases
        * Logic distributed elsewhere
    * Avoid circular dependencies
    * Avoid chatty communications due to clusters of ms that need to constantly communicate with one another

##### Summary
* Evolving towards microservices
* Microservices own their own data
* Microservices may consist of multiple processes
* Microservices should be independently deployable
* Avoid breaking changes
* Identify “bounded contexts” for microservice boundaries 
* Getting boundaries right is important

---












