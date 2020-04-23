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

_To be continued..._














