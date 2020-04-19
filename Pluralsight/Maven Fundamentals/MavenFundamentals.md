# Maven Fundamentals - Pluralsight course by Bryan Hansen
### Module 1 - Course Overview
* Maven installation
* Transitive dependencies
* Building using Java 11 or newer
* Using Maven with existing code

---

### Module 2 - Introduction to Maven
* Introduction
* Key concepts - Convention over configuration
* Day-to-day coding
* Integration - IDE / standalone tool
* Complexities

Topics
* Intro to Maven
* Structure
* Dependencies - most basic use case of maven
* Repositories
* Plugins - actions in Maven
* IDE integration

Introduction
Outline
* High level overview
* Ant vs Maven vs IDE
* Installation best practices
* Hello World Application

High Level Overview
* Basically a build tool
    * Produces one output called Artifact
    * Helps manage dependencies
* Project Management tool
    * Handles versioning/ releases
    * Can specify meta info - source control, authors, documentation location
    * Produces Javadocs / site information

Who owns it
* Apache Software Foundation
* Open Source, free

Why use it
* Repeatable builds - recreate builds for any environment without having to change configuration. Can use with Docker
* Transitive dependencies - downloading a dependency, maven will help pull its dependencies - no. 1 reason to use Maven
* Environment
* Local repo - instead of keeping dependencies in a project, maven works from a local repo. Download once and reference from there
* IDE and Standalone - works very well with IDE or as a standalone tool (build from command-line)
* Preferred method for working with other tools like Jenkins and other automated build tools. Readily available plugins for these integrations

Ant vs Maven
Ant
* Replacement for Make
* Cross platform
* Java and XML based
* Procedural - hard to do inheritance or composition 
* No standards or consistency. Scripts may not be carried over to projects. Not a lot of reuse. No structure. 
Maven
* Full featured build tool
* Implicit
* Consistency
* Inheritance
* Transitive dependencies
* Versioned

Installation Best Practices


_To be continued_














