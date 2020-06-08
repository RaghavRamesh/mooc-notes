# Spring WebFlux: Getting Started - Pluralsight course by Esteban Herrera

### Module 1 - Course Overview
Reactive web applications is the solution for growing number of internet users in this day and age

##### Topics covered
* Reactive programming
* How to work with annotated controllers
* How to work with functional endpoints
* How to execute requests
* How to setup integration tests with web client
By the end of the course you’ll understand the basics of Spring WebFlux and reactive programming to create web applications

---

### Module 2 - Introducing Spring WebFlux

##### Course Overview 
* Introducing Spring WebFlux
    * What is Reactive Programming, its concepts and how S WF is based on those concepts
    * Architecture of S WF and how it is related to other Spring projects
    * Difference between traditional and reactive approaches
    * Back-pressure, async, non-blocking programming 
* Learning Reactive programming with Reactor
    * Core interfaces of the Reactive Streams spec, implementation provided by Reactor - used by S WF
    * Mono and Flux - core classes of Reactor
    * Operators like: map, “flatMap”, take and subscribe
    * Sample project - a REST API
* API client with WebClient
* Testing with Web TestClient

##### What is Reactive Programming?
Usually means 2 things:
* Architecture and design of reactive systems
* Reactive programming

You can build a part of your application using reactive pogromming but that doesn’t make the system reactive. What makes a system reactive depends on the traits it follows as defined in the Reactive Manifesto.
* Responsive - should respond to all users in all conditions
    * Resilient 
    * Scalable 
* Message-driven architecture

Reactive programming
* Event-driven
* Data flow

In imperative programming, the ability of a program to react to change in events is not implicit. It needs to be defined explicitly. For e.g., if var b depends on value of var a. If a changes, b doesn’t automatically change unless explicitly programmed. 

One way to achieve this is via the Observer pattern - that is with a Producer and a Consumer. 
One drawback of the Observer pattern is that the Consumer doesn’t have the ability to tell the Producer to send events at a different rent if it is unable to keep up with the rate at which the producer is sending messages. This ability to be able to handle the rate of event production at a different rate or only at the rate at which the Consumer can handle is called backpressure.  

##### Reactive programming model
* Non-blocking
* Asynchronous
* Functional / Declarative 

##### Non-Blocking 
Blocking is another work for synchronous. 
Web server has a thread pool. Each request is handled by a thread and the maximum number of requests that a server handle at the same time depends on the # of threads in the thread pool. If each thread uses a memory of 1MB, then the program needs 500MB to start. 
This is inefficient use of memory when there is a blocking call performed as part of a request because of the wait without performing any other task. Also an inefficient use of CPU cycles because server doesn’t perform anything else while waiting. 

Evented web servers are more efficient because request coming in is split into multiple events. If there is a blocking operation is to be performed, they are taken care of by worker threads while the threads are freed up to handle additional requests. Once the worker thread completes, the event in the event loop queue is due to be processed. 

##### Reactive Streams
An initiative to provide a standard for asynchronous stream processing with non-blocking back pressure. Have no relationship with Java Stream API. 

##### Java Stream API vs Reactive Streams
* Even though have a declarative syntax, they work in different ways. The Stream API connects a source, generally a list, it _pulls_ values out of it and it can be used only once. 
* In a Reactive Stream, the values are not pulled. They are pushed through a pipeline of operators and finally subscribed. The back pressure feature is only present in Reactive Streams. 

##### Spring WebFlux
New Reactive Web Framework that comes with Spring Framework 5. It doesn’t replace Spring MVC. You can even use both at the same time. 

##### Spring MVC vs Spring WebFlux
Spring MVC
* Based on Servlet API
* Blocking API
* Synchronous programming model
* One request per thread
* spring-web-mvc which uses Servlet API which runs in a Servlet Container

Spring WebFlux
* Based on Reactive Streams
* Non-blocking API (Servlet 3.1+)
* Async model
* Concurrent connection with few threads
* spring-web-reactive which uses HTTP/ Reactive Streams and uses Netty (default), Tomcat, Jetty, Undertow. 

In most cases, it’s the Servlet API that forces to use the blocking model, not the server

##### Reactive all the way
_TODO: insert image from Evernote_
Remember to use reactive extensions in all layers of the stack

Project Reactor is the default Reactive library to work with for Spring WebFlux. 
##### Things to remember
* Reactive Programming 
    * Different from Reactive systems and Reactive Manifesto (which refers to the architecture and high-level design of the system and are driven by messages). Reactive programming is driven by events. 
    * It focuses on flow of data in
        * Non-blocking 
        * Asynchronous way
        * And using a Functional/declarative syntax
* Spring WebFlux
    * Alternative to Spring MVC, but doesn’t substitute it
    * Annotation and Functional model
    * If you choose to work with WebFlux - all the layers of the stack must be reactive - from the db to the web server. 
    * Reactive support in WebFlux comes from Reactive Streams specification - in particular the implementation provided by Reactor. 

---

### Module 3 - Learning Reactive Programming with Reactor

_to be continued…_




 












-- INSERT --
