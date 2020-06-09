# Applying Concurrency and Multi-threading to Common Java Patterns - Pluralsight course by Jose´ Paumard

### Module 1 - Understanding Concurrency, Threading and Synchronisation

##### Course Overview
* Concurrency: “the art of doing several things at the _same_ time"
* What does _correct code_ mean in the concurrent world?
* How to improve your code by leveraging multi-core CPUs
* Writing code, implementing patterns
* Race condition, synchronisation, volatility
* Visibility, false sharing, happens-before

##### Pre-requisites
* Basic Java knowledge 
* No prior knowledge about concurrency is needed

##### Course Agenda
1. Understanding concurrency, threading and synchronisation
2. Implementing the producer / consumer pattern using wait / notify
3. Ordering reads and writes operations on a multicore CPU
4. Implementing a thread safe singleton on a multicore CPU

##### What is a Thread?
* A thread is defined at the Operating System level
* A thread is a set of instructions 
* An application can be composed of several threads
* Different threads can be executed “at the same time"
* The JVM works with several threads (threads for GC, JIT, …)

##### What does “at the same time” mean?
* Example: Writing a text doc + running a spell check + print elements of the document + receiving emails. All these 4 are happening “at the same time"
* 1st Case: CPU with 1 core
    * Implies the CPU can do only 1 thing at a time
    * 
    * Nothing is really happening at the same time
    * But it *feels* like it is happening at the same time because the task switching is happening very fast within a span of milli seconds so we perceive it as they are all happening at the same time
* 2nd Case: CPU with multiple cores
    * 
    * On a multicore CPU are things are really happening “at the same time” because the CPU is capable of doing multiple things at the same time

##### Who is responsible for the CPU sharing?
* A special element called a thread scheduler which will evenly share the CPU timeline divided into time slices across all the tasks that need to be run
* There are 3 reasons for the scheduler to pause a thread:
    * The CPU should be shared equally among threads
    * The thread is waiting for some more data
    *  The thread is waiting for another thread to do something for e.g. to release a resource

##### Race condition
* Accessing data concurrently may lead to issues
* It means that two different threads are trying to *read* and *write* the *same variable* at the *same time*
* This concurrent reading and writing is called a race condition
* “Same time” does not mean the same thing on a single core and on a multicore CPU

##### Example: Singleton Pattern
* What is happening if two threads are calling `getInstance()`?
    * If T1 is paused when inside the if condition, before creating an instance and T2 starts and finishes creating an instance, then the scheduler resumes T1, then T1 will create another instance and replaces the instance created by T2. 
* How to prevent that?
    * Synchronisation - prevents a block of code to be execute by more than one thread at the same time
    * For the Singleton example, we just need to add the “synchronized” keyword in the getInstance method declaration - `public static synchronized Singleton getInstance()`

##### Synchronization
* Under the hood there is a lock and a key for each block of code protected by the synchronized keyword. 
* When one thread wants to enter this block of code, it requests for the key, uses it to enter the block of code and holds on to it until it finishes executing that block of code. 
* If another thread wants to enter the same block of code, it also makes the same request and if the key is not available, it has to wait until it is. 
* Once the thread that has the key finishes executing the block of code, it returns the key to the lock
* Once the key is available the thread that is waiting can use the key to execute the block of code
* This simple method will prevent more than 1 thread from executing the block of code at the same time

* So for synchronization to work, we need a special technical object that will hold the key
* In face, every Java object can play this role. This key is defined in the Object class thus making it available for all the Java objects we define in our apps
* This key is also called a monitor

##### How can we designate this synchronization object?
* In the above example, since the synchronized keyword is assigned on a static method, the key is held by the Singleton class
* If the synchronized keyword is assigned on a non-static method, then the key is held by an instance of the class. 
* A third possibility is to use a dedicated object to synchronize. It is always a good idea to hide an object used for synchronization. 
    * 
    * This is probably the best way to do because it’s always better to hide the object used for synchronization whether we are in a static context or not.

##### Synchronizing More Than One Method
Corner case
* If you assign synchronized to 2 non-static methods in a class, then when one thread is executing one of the sync methods, another thread cannot execute the other sync method because the same key is being used by the first thread. 
    * If we want to synchronize the methods independently then we need to create two different lock objects in the class and sync the block of code inside the methods on those methods. 
To understand how synchronization works, you need to identify which object is used as a lock and what are the keys used in your application. 
Using a synchronized keyword on a method declaration uses  an implicit lock object - class object in case of a static method and the instance object in the case of a non-static method. 

_to be continued..._


