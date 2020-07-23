# GraphQL: The Big Picture - Pluralsight course by Adhithi Ravichandran
### Module 1 - Course Overview
Module Agenda
* Overview of GraphQL
* GraphQL core concepts
* Why GraphQL
* GraphQL ecosystem and tooling

---

### Module 2 - What is GraphQL

* Alternate approach to REST
* Query language to your API
* Gives clients the power to ask exactly what they need
* Apps using GraphQL are fast and stable because they control what they get
* Companies that use - Facebook, PayPal, Twitter, JPMC
* When is it a good fit?

##### History
* In 2012, Facebook started the GraphQL project to overcome data fetching issues in their native mobile platform
* Moved the focus of development to the client apps
* GraphQL was open-sourced in 2015. Ever since then, there has been huge community involvement
* It came into existence due to the need for better flexibility and efficiency in client-server communication
* Today, it has become comparable or as an even better alternative to REST APIs with a wide adoption among several Fortune 500 companies

##### What is GraphQL?
* GraphQL is a specification and a query language for your API. 
* It’s not a library, product or a database.
* Built with the goal of being a more flexible and efficient alternative to REST. 

##### Before GraphQL
* Hit an API endpoint, get a massive JSON. 
* With GraphQL, you can request for exactly the data you want and receive just that result - nothing more and nothing less. 
* _TODO: insert picture from Evernote_
* The response looks similar in structure to the request

##### GraphQL 
* Provides clients the power to ask for exactly what they need and nothing more
* GraphQL APIs get all the data your app needs in a single request
* Language agnostic - plenty of client and server libraries are available

##### Who is using GraphQL?
* Facebook. Heavily invested
* PayPal - one of the mainstream adopters. PayPal checkout uses this. 
* Twitter - APIs are publicly available. Saves 25% per request. Helps teams move faster, spend lesser resources in building endpoints. 
* Yelp 
* Shopify
* GitHub 

##### REST vs. GraphQL
* Example: get a Pluralsight author’s name, course titles, rating and the last three topic names
* GraphQL request. Response structure is similar to that of the request. 
```
{ 
  author (id : 2100) { 
    name
    courses { title }
    rating
    topics (last : 3) { name }
  }
}
```
* Corresponding REST requests
    * /ps/author/<id>
    * /ps/author/<id>/courses
    * /ps/author/<id>/rating
    * /ps/author/<id>/topics
* REST vs. GraphQL
    * REST
        * Multiple round trips to collect the info from multiple resources
        * Over fetching and under fetching data resources
        * Front-end teams rely heavily on backend teams to deliver the APIs - often leads to blockers
        * Caching is built into HTTP spec
    * GraphQL
        * One single request to collect the info by aggregation of data
        * You only get what you ask for. Tailor made queries to your exact needs
        * Front-end and back-end teams can work independently off mock-APIs
        * Doesn’t use HTTP spec for caching - it’s up to the developers to build it in - libraries like Apollo, Relay come with caching options. 
* _So, GraphQL can only replace GET requests? No, there are mutation types which allow you to modify or delete data from the API._

##### Is GraphQL right for my business?
* One of the biggest contributions - increases multi-team productivity. Front-end team doesn’t have to bug the back-end service developers for complete APIs. They don’t need to wait for new versions to be released and the teams can work in parallel - Leads to rapid product development 
    * _Hmm, can’t you just agree on API design upfront in the case of REST?_
* Improved performance for web and mobile apps due to lesser requests and getting exactly the data needed and not more than that. 
* Reduced cost in testing and deployment - only needs to be tested when there’s a change in the schema or when there’s a fresh schema -> Much less expensive for testing and deployments. 
    * _Hmm…_
* For cases with legacy APIs, GraphQL can be used to unify the systems and act as a facade hiding the complexities behind the GraphQL API. The new front-end apps can be used to simply talk to the GraphQL server. 
    * _Hmm, isn’t that just pushing the complexity from one place to another?_

---

### Module 3 - GraphQL Core Concepts

##### Types 
* GraphQL has a strongly typed schema and this schema acts as a contract between the client and the server
* Scalar Types
    * Int - A signed 32-bit integer
    * Float - A signed double-precision floating-point value
    * String - A UTF-8 character sequence
    * Boolean - true or false values
    * ID - Unique identifier. Used to re-fetch an object or as the key for a cache
* In addition you can create your own types that are composed of the Scalar types. 
* Also support Enumeration types - it allows you to validate that any arguments of this type are one of the allowed values.
* Query and Mutation Types
    * Every GraphQL service has a query type. It may or may not have a mutation type. They act as an entry point into the schema. 
    * Query and Mutation types are the same as any other GraphQL object type.
    * 
* Non-Nullable Type (!)
    * By default each of the core scalar types can be set to null. 
    * To override this default behaviour and ensure that a field cannot be null, the (!) is used.
    * You can even have a list of Non-Null items. 

##### Queries
* Keyword - query
* GraphiQL - GraphiQL is an in-browser IDE for writing, validating and testing GraphQL queries. 
* GitHub’s GraphQL Explorer - https://developer.github.com/v4/explorer
* GraphQL schema is self-documenting

##### Queries - Arguments
* Can pass args to fields 
* Every field and nested object can get its own set of args
* This gets rid of multiple API fetches

##### Queries - Alias
* You can’t query for the same field with different arguments. 
* Hence you need aliases. 
* They let you rename the result of a field with anything you want. 
* Supply the alias followed by colon - firstFollowers: followers { … }

##### Queries - Fragments
* Fragments are GraphQL’s reusable units (like functions). 
* They let you build sets of fields and then include them in multiple queries. 
```
fragment userInfo on User { 
  id 
  bio
  bioHTML 
  avatarUrl 
}
```
Then plug it in your query like so 
```
...
nodes {
  …userInfo
}
...
```

##### Queries - Operation name
* A meaningful and explicit name for your operation. Think of it like a function name in a programming language
* Queries and mutations are better organised when they are given unique names

##### Queries - Variables
* Arguments to fields can be dynamic. GraphQL uses variables to factor dynamic values out of the query, and pass them as a separate dictionary.

##### Mutations
* Mutations are used to changes to the data (Create, Update, Delete data)
* GraphQL assumes side-effects after mutations and changes the dataset after a mutation
* While query fields are executed in parallel, mutation fields run in series one after the other
* Keyword -  mutation 
 
```
mutation NewStatus($input: ChangeUserStatusInput!) {
  changeUserStatus(input:$input) {
    clientMutationId
    status {
      message
    }
  }
}
{
  “Input”: {
    “clientMutationId”: “101010”,
    “message”: “Demo for Pluralsight"
  }
}
```

---


### Module 4 - Why GraphQL?

##### Declarative Data Fetching. Benefits:
* Avoid round-trips to fetch data
* No more over-fetching or under-fetching of data
* You specify exactly what you need and you get exactly what you want

##### Strongly Typed Schema
* GraphQL is a strongly-typed language and its schema should have types for all objects that it uses
* The schema serves as a contract between client and server
* Relies on a query language with a type system
* Benefits
    * Code is predictable
    * Front-end and back-end teams work independently
    * Earlier detection of errors and speeds development
    * Facilitates building high quality client tools - like GraphiQL

##### Superior Developer Experience
* GraphQL offers a lot of flexibility
    * Teams work independently
    * Front-end no longer has to wait for back-end team to get the APIs ready
    * With strictly-typed schema, teams can work in parallel
    * Front-end can continue work with mocked version of API and also test it
* Evolve your API without versioning
    * Add new fields and types to GraphQL API without breaking changes
    * Provides apps continuous access to new features
    * Developers don’t have to maintain multiple versions of the API
* Modern Ecosystem and Tooling
    * Tools like GraphiQL IDE provide excellent developer experience
    * Hours of documentation time saved with in-built self-documentation in the schema
    * Plenty of GraphQL clients for every UI framework (Apollo, Relay)
    * Lots of server-side libraries, client libraries and services across several languages

##### Cost Effective
* Rapid product development lifecycle
    * Multiple teams work in parallel
    * Self documentation of API 
    * Save money with minimal training - easy to onboard

##### Growing Community
* Ever-growing
* GraphQL Facebook group - preferred venue for major announcements
* #GraphQL, @GraphQL
* StackOverflow - "graphql"
* Plenty of GraphQL conferences, meetups spread out across the globe
* Apollo platform and community - their blog is a great resource
* graphql.org/community

---

### Module 5 - GraphQL Ecosystem and Tooling
GraphQL is a specification that is language agnostic
* GraphQL Client - Layer that takes care of talking to the server and getting the response back
* GraphQL Server - Layer that takes care of creation of API and handling requests from clients and responding

##### What can a GraphQL Client do?
* Handles sending requests(queries) to the server and receiving the response(JSON) from the server
* Integrates with your view components and updates the UI
* Caching Query Results
* Error handling and schema validation of queries the client wants to send
* Local state management and caching
* Pagination

##### Popular GraphQL clients
* Apollo - most popular. Works with many clients. Go-to client. 
* Relay - developed by Facebook. Only works for React. 

##### What can a GraphQL Server do?
* Receives the query from the client and responds back. 
* Here’s where the real work is done. 
* Schema and Resolver Functions
    * Resolvers - Resolver Function is a function that resolves a value for a type/field in the GraphQL Schema 
* Network Layer - the queries from the client need to be transported to the server over HTTPS
* GraphQL Execution Engine
    * Parses query from client
    * Validate schema
    * Returns JSON response
    * GraphQL has an execution algorithm
        * Executes resolves for each field and puts them together
    * Batched Resolving

##### Popular GraphQL server libraries
* Apollo Server 
* Express GraphQL - works well with GraphQL
* GraphQL Yoga 

##### Database to GraphQL Server
* Prisma - bridges the gap between GraphQL server and the databases
* Supports both SQL and NoSQL
* Replaces traditional ORMs and simplifies database workflows
* Data access layer that makes it easy for API server to talk to DB through Prisma
* Type-safe database access
* GraphQL subscriptions
* Powerful visual data management available with Prisma admin
* Compatible with several back-end languages

##### Tools
* GraphiQL
    * An in-browser IDE for writing, validating and testing GraphQL queries
    * Write queries to retrieve data
    * Perform mutations
    * Live syntax and validation errors highlighted
* GraphQL Voyager
    * Represents any GraphQL API as an interactive graph
    * Perfect to explore complex GraphQL APIs
    * Great tool during design of data model
* GraphQL Faker
    * Mock your API with realistic data from faker.js
    * No additional coding required
    * Useful for testing and coding against future APIs
* GraphQL Visual Editor
    * Create schemas by joining visual blocks
    * GraphQL editor transforms them into code
    * Perfect to understand any GraphQL schema

Best time to start working on GraphQL due to the prolific community

---


 
















