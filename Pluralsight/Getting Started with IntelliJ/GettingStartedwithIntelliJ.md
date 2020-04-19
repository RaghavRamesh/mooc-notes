# Getting Started with IntelliJ - Pluralsight course by David Starr

### Module 1 - Intro

_Nothing much to note_

---

### Module 2 - Introducing IntelliJ CE

Orange coloured directories => not a part of the project

---

### Module 3 - IntelliJ CE Projects for Java

##### IntelliJ CE Module
* A module is a discrete unit of functionality that can be independently run in the IDE. 
* A project may contain many modules. 
* Includes src code, build scripts, unit tests, deployment descriptors
* Each module can use an SDK or inherit an SDK based on the project level
* A module can depend on other modules of the project

##### IDE project files
* <Name of Project>.iml in root of project
    * Created for each IntelliJ CE module
    * This file can go to version control
* .idea folder 
    * Created for each project
    * Collection of config files applied to entire project regardless of how many modules the project contains
    * A lot of details about the project is kept under this folder. 
    * None are appropriate for inclusion to version control except for the .idea/workspace.xml which contains the personal preferences for the project just for you. 

---

### Module 4 - Core IDE Tools

* Project window
    * Cmd + 1 - to hide and show
    * Multiple views
        * Project
        * Packages
        * Project files - less noise, based on folder structure
        * Problems
        * Production - files included for production build (no project files or test files)
        * Test
        * Open files
        * Changed files
* Navigation (Breadcrumbs)
    * Cmd + Up 
* Scratch files
    * Create one with Shift + Cmd + N

_To be continued_




















