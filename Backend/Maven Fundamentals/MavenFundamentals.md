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

### Module 4 - Dependencies

Overview
* Versions
* Types
* Transitive dependencies
* Scopes - test, provided, etc.

* 3 items are required to list a dependency
    * groupId
    * artifactId
    * version

Versions
* Release number of the dependency
* SNAPSHOT
    * All of your internal development should start of as a snapshot
    * You should also be aware of using snapshots from other 3rd-party libraries
    * It allows us to push new code up to a repository or a development team, every time. 
    * If you use a library with SNAPSHOT specified in the version, then you pull the code of the library every time
    * Doesn’t work if it is lower case
    * You never want to release to production using “SNAPSHOT” because we cannot reproduce or recreate the code because the next time you compile it the version could be different underneath for us
* Release
    * Doesn’t have to be a specific naming convention
* Industry common terms
    * These don’t affect maven but helps in conceptualising 
        * “M” milestone. myapp-1.0-M1.jar
        * “RC” release candidate. myapp-1.0-RC1.jar
        * “RELEASE” - to look similar to the SNAPSHOT convention
        * “Final” 
        * Can be used when you can't use SNAPSHOT cuz production but at the same time not fully stable

Types
* Refers to Packaging Types
    * “pom”, “jar”, “war”, “ear”, maven-plugin, “rar”, “par”. “*ar” are all just glorified zip files
    * “pom” - dependency POM. Will download all the dependencies mentioned in the POM onto our application

Transitive Dependencies
* Without a doubt the main reason people use Maven
* The feature where maven automatically pulls in dependencies of the dependency listed in your POM

Scopes
* 6 scopes
* compile
    * default
    * Means all my resources are available everywhere inside of my application
* provided
    * A lot like “compile" - artifact is going to be available throughout the build cycle. 
    * Included in all phases / goals but not going to package it up in the end in the final artifact. E.g., servlet APIs which are available in the container we are deploying our app to. 
* runtime
    * Not needed for compilation but needed for runtime. e.g., jdbc jars or whatever that’s loaded in runtime. 
    * We need this bundled in our application. So it’s kind of the opposite of “provided”. No need for compiling, testing, packaging but need it for app to run
* test
    * Available only in the test, compilation and execution phase. 
    * It does nothing for compiling, does nothing for packaging. Also not included in final artifact. e.g., jUnit, 
* system
    * Never use this. This is for hardcoding a path in the file system. So it deters distribution. Only included in maven for the use case where you want to “mavenize” existing legacy apps. 
* import
    * Advanced topic for sharing dependencies across multiple POMs. Not covered in this course. 

---

_To be continued..._













