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

***

### Module 3 - Domain-centric Architecture

##### Overview 
- Domain-centric architecture and how it varies from database-centric architecture
- Types of Domain-centric architectures
- Pros and Cons

##### Domain-centric Architecture

DBCA - architecture revolved around the database. Lately, there has been shift towards making the Domain the center and considering the db as just an implementation detail

“The first concern of the architect is to make sure that the house is usable, it is not to ensure that the house is made of brick.” - Uncle Bob

What is essential vs. detail? 

E.g. while building a house:
- Space is essential
- Usability is essential
- Building material is a detail
- Ornamentation is a detail

Similarly, we need to look at what is essential for the inhabitants of the architecture
- Domain is essential - without it the system will not represent the mental models of the users
- Use cases are essential 
- Presentation is a detail - JS vs [ASP.NET](http://ASP.NET) MVC is just a detail
- Persistence is a detail - SQL vs NoSQL db is just a detail. They are important yes, but not more than the essentials which are about solving the problems of the users. 

##### Domain-centric architectures

Hexagonal Architecture

[Hexagonal archiecture](img.png)

Onion Architecture

[Onion Architecture](img.png)

No inner layer knows about the outer layer. 

The Clean Architecture

[The Clean Architecture](img.png)

All 3 architectures have roughly the same benefits. Essentially, they put the domain model at the center, wrap it in an application layer which embeds the use cases, adapts the application to implementation details and all dependencies should point inwards towards the domain. 

##### Pros and Cons
Pros
- Focus on domain =&gt; puts inhabitants’ needs at the center =&gt; cost reductions in maintaining the software
- Less coupling between domain logic and implementation detail =&gt; more flexible and adaptable =&gt; more easily evolved
-  Allows for Domain Driven Design (DDD) - which is a great set of strategies for handling high degree of complexity

Cons
- Change is difficult - most are taught only the traditional 3-layered db-centric architecture 
- Requires more thought to implement a domain-centric design - it requires thinking about what classes belong in the domain layer, vs. what classes belong in the application layer rather than placing everything in the business logic layer
- Initial higher cost compared to a db-centric architecture but eventually pays off. 

* * *

### Module 4 - Application Layer

##### Overview
- Application Layer - learn about architectures with an application layer and how it differs from the traditional 3-layered architecture
- Dependency Inversion Principle - which helps make arch more flexible and maintainable
- Pros and Cons

##### What are Layers?

Boundaries and Vertical partitions of an application designed to 
- Represent different levels of abstraction
- Maintain the single-responsibility principle
- Isolate developer roles and skills
- Help support multiple implementations
- Assist with varying rates of change

Essentially, layers are how we slice the application into manageable units of complexity

##### Classic Three-layer Architecture

Suitable for basic CRUD applications. 

##### Modern Four-layer Architecture

##### Application layer 
- Implements use cases as executable code - e.g., shopping cart code
- High-level application logic - e.g., command that adds an item to a shopping cart
- Knows about the domain layer
- No knowledge of other layers like presentation, persistence or infrastructure layers. 
- Contains interfaces for dependencies
- Then we use an IoC (Inversion of control) framework and dependency injection to wire up all the interfaces and their implementations at runtime

##### Layer dependencies

Dependency inversion - abstractions should not depend on details, rather, details should depend on abstractions. Hence the arrow from infrastructure to application and persistence to application instead of the other way around. 

Inversion of control - that is, the dependencies oppose the flow of control in our application, which provides several benefits like
- Independent deployability - i.e., we can replace the implementation in product without affecting  the dependencies 
- Flexible and maintainable - for e.g., we can swap out our persistence and infrastructure dependencies without having negative side effects ripple throughout the application and domain layers

Sometimes we may need to add an additional dependency from the persistence to the domain when using an Object Relational Mapper. This is necessary for the ORM to map the domain entities into tables in the database since the persistence layer needs to know about the domain entities. But using an ORM is optional but saves a tremendous amount of time. 

##### Why use an Application Layer?

Pros
- Focus on use cases
- Embed use cases as high-level executable code - easy to understand
- Follows DIP - makes code more flexible and lets us decide implementation details until later

Cons
- Additional layer cost - generally we want to keep the number of layers small
- Requires extra thought on what belongs in application layer and what belongs in domain layer rather than just throwing it all in business logic layer
- IoC is counter-intuitive initially 

***