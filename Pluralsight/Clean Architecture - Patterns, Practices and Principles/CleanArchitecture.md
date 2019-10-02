# Clean Architecture - Patterns, Practices and Principles

### Module 1 - Course overview

Course overview
- Clean Architecture
- Domain-centric Architecture
- Application Layer
- Commands and Queries
- Functional Organization
- Microservices
- Testable Architecture
- Evolving the Architecture

* * *

### Module 2 - Introduction

Purpose of the course
- Philosophy of architectural essentialism - way of designing an architecture around what’s essential vs. what’s just a detail
- Set of patterns, practices and principles 
- Alternative to traditional architecture (3 layered db architecture)

Focus
- Enterprise applications
- Agile architecture
- Top 7 ideas

Audience
- Developers
- Architects

What is Clean Code?

What is Software Architecture?
- High-level (at least higher level than the code we write)
- Structure - how things are organised. 
- Layers - vertical partitions of the system
- Components - horizontal partitions within each layer 
- Relationships - how they are all wired together

Levels of Architectural Abstraction

This course will be covering mostly layers and components and bit of sub-systems when covering microservices

Messy vs. Clear Architecture - Spaghetti vs. Lasagna

What is Bad Architecture?
- Complex - accidental instead of essential complexity
- Incoherent - parts don’t seem like they fit together
- Rigid - resists change or difficult to evolve
- Brittle - touching one part of code here, breaks somewhere else
- Untestable - architecture fights you when you try to write unit and integration tests even though you want to write them
- Unmaintainable - all the above leads to this

What is Good Architecture?
- Simple - or at least only as complex as it is necessary
- Understandable - easy to reason about the software as a whole
- Flexible - easy to adapt the system to meet changing requirements
- Emergent - evolves over the life of the project
- Testable - makes testing easier
- Maintainable - ultimately all of the above make the software more maintainable over the life of the project

What is Clean Architecture?
Architecture that is designed for the inhabitants of the architecture (users, developers building and maintaining the system)… not for the architect (put aside his or her own desires and wishes and design what’s best for the inhabitants of the system)… or the machine (optimise first for the inhabitants and only optimise for the machine when necessary, i.e., we want to avoid premature optimisation). Clean architecture is a philosophy of architectural essentialism. It’s about focusing on what’s truly essential to the software’s architecture instead of the implementation detail. 

Why invest in Clean Architecture?
- Cost / benefit 
    - Minimise cost (of creation and maintenance)
    - Maximise (business) value 
- Overall goal thus, is to maximise ROI 
- Focus on the essential - mirrors use cases of the mental models of the users by embedding them in both arch and code 
- Build only what is necessary, when necessary - only build features and corresponding architecture that are necessary to solve immediate need to avoid accidental complexity, unnecessary features, premature performance optimisations and arch embellishments
- Optimise for maintainability - enterprise apps last a long time and a significant amount of time and resources are spent in maintaining, so it makes sense to optimise for maintainability. 

Decisions, decisions, decisions…
- Answers to most questions to an architect is “it depends"
    - Context is king
    - All decisions are a tradeoff
    - Align with business goals