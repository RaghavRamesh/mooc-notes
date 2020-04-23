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


### Module 3 - Structure

Overview
* Folder structure
* POM file basics
* Basic commands and goals
* Dependencies
* Local Repo

Folder Structure
* By default maven 
    * looks for a src/main/java folder 
    * Compiles to a target folder
    * Based on what’s in “pom.xml"

“src/main/java"
* All Java code goes here
* Package declaration starts here
* Other Languages - src/main/groovy or src/main/resources
* Testing - “src/test” - can have them reference the same package structure. Specifically for unit tests

“target"
* Where everything gets compiled to
* Tests - run from here
* Package contents

“pom.xml”
* Lot of things are assumed here - convention over configuration
* Project info - <project> - the meat
    * <groupId> - often the same as the package - com.pluralsight
    * <artifactId> - name of application. This is how the artifact will be name as well
    * <version> - application version; 
    * <modelVersion> - is almost always 4.0.0 - the author hasn’t seen anything else used in general
    * <packaging> - how we want to distribute our app - jar is the default

Dependencies
* Naming convention
* “groupId”, “artifactId”, “version” - you need to know these of your libraries. Good thing is it pulls in the transitive dependencies
* <dependencies>

Goals
* “clean” - deletes the target directory and any of its generated sources
* “compile” - compile all source code and generate files for say, web services like skeletons and resources get copied over to target
* “package” - Runs “compile" first. But we can daisy chain goals. Runs unit tests depending on the type we have defined in POM
* “install” - Runs “package" and then it will install it in the local repo. Places the jar or war in the repo.
* “deploy” - does not deploy to an app server. Runs “install" and then deploys it to a corporate or remote repository. It’s the remote equivalent of “install"

Defaults
* Typically maven will have to be used to work with a legacy project where the defaults don’t work
* Build section is used for overriding defaults 
* Maven storage
    * ~/.m2/repository
    * Stores artifact using info <groupId>/<artifactId>/<version>
    * This avoid duplication

---















