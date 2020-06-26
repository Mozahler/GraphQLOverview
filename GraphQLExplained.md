# GraphQL Explained

## GraphQL Enables Efficient and Flexible Client-Server Communication
 - GraphQL is an API technology designed to describe the complex, nested data dependencies of modern web applications. It is often considered as an alternative to SOAP or REST or to complement existing back end configurations
 - GraphQL works as a central data provider (aka. single endpoint for all data)
 - GraphQL is a query language that allows the client to describe the data it needs and its shape by providing a common interface for client and server. It has been designed with a flexible syntax that makes building client applications easier. This client/server interface also makes retrieving data and manipulations more efficient because it encourages using only the data needed, rather than retrieving a fixed set of data.
 - GraphQL is an alternative to REST endpoints for handling queries and database updates. It is a way to define a contract of what is provided by the server to a web application. GraphQL tries to improve how clients communicate with remote systems. It comes from a simple idea – instead of defining the structure of responses on the server, the client is given the flexibility to define what it wants in response to its queries.
 - With GraphQL, the UI gets the data it needs in a form that is useful for the UI. The core idea is that the code knows that the data that it needs are on the client, not the server. With GraphQL as an abstraction layer, we hide the complications of web services and let back-end developers create a menu of both available data structures and items available to retrieve (and how to retrieve them). Doing so allows the front-end application - and its developers - the ability to select from the items on the menu as they need. Because the server is handling all this, the front-end application doesn't need to worry about new items being added to the menu or where the data for that menu is coming from.

Highlights from the Apollo Blog:
https://www.apollographql.com/blog/graphql-explained-5844742f195e#.zdykxos6i
WRITTEN BY
Jonas Helfer

The QL in GraphQL stands for Query Language, and GraphQL’s query language is very easy to understand. 
Using GraphQL’s type system you'll find its advantages over traditional RESTful APIs become obvious quite quickly.

Summary Questions (You Should Know the Answers to these by the end of the Article)
What is a resolver?
What is a Schema?
What is Relay?

GraphQL Queries
### Query
{
  subscribers(publication: "apollo-stack"){
    name
    email
  }
}

### Response - the data returned
{
  subscribers: [
    { name: "Jane Doe", email: "jane@doe.com" },
    { name: "John Doe", email: "john@doe.com" },
    ...
  ]
}

Notice how the shape of the response is almost the same as that of the query. This makes the client-side of GraphQL practically self-explanatory.
(Yes, there are workarounds for when they need to be different...)

## Schema and Resolve
Every GraphQL server has two core parts that determine how it works: a schema and resolve functions.

The schema: The schema is a model of the data that can be fetched through the GraphQL server.
It defines what queries clients are allowed to make, what types of data can be fetched from the server, and what the relationships between these types are.

Query and Response

GraphQL servers are actually very easy to build, especially compared to a traditional RESTful API. All you need to do is define a schema — essentially a directory of the data types in your application — and resolve functions that tell the server where and how to fetch the data for each data type.

To put things in RESTful terms:

The schema is just like writing a Swagger documentation for your API, except that in GraphQL the schema is always enforced. 
The schema also completely removes the need for you to specify a router and parse url patterns. 
Having a strictly enforced schema ensures that documentation is always complete and up to date.

The resolve functions are similar to the code you would write inside your REST endpoints, except that each of them has a clear responsibility for fetching items of just one type.

GraphQL replaces REST “best practices” with a clear & simple structure
Because GraphQL gives you a logical structure in which to fit your code, building GraphQL servers is often much faster and easier than building a REST API.

Can I use GraphQL with My Favorite Language/Product?

There are many open-source GraphQL clients available
You can even use GraphQL without any client and send raw strings to the server, like you do for REST

Relay is the JavaScript client for GraphQL that Facebook uses. 
It adds many things on top of GraphQL, such as query merging, pagination and deferred queries. 
Relay is complex, tightly integrated with React and doesn’t (currently) support other view layers and can have a pretty steep learning curve.

So what alternatives are there?

If you’re looking for a client that does caching and integrates well with your view layer and state management, Apollo Client is probably a good choice: It’s easy to use and well-documented. It has integrations for React, Redux and Angular 2, and other integrations — for example React Native — are actively being worked on.
If you’re looking for a simple tool that just helps you send GraphQL queries and responses, take a look at Lokka. It clocks in at barely over 100 lines of code, which shows how simple a GraphQL client can be.


## How does a GraphQL server turn a query into a response?


## Server
GraphQL hides the backend complexity from clients: no matter how many backends your app uses, all the client will see is a single GraphQL endpoint with a simple, self-documenting API for your application. This is accomplished using a schema and resolve functions.

### Schema
A schema is a model of the data that can be fetched through the GraphQL server. It defines what queries clients are allowed to make, what types of data can be fetched from the server, and the relationships between these types. The schema doesn't explain where the data for each type comes from.

### Resolve Functions
 That’s what resolve functions are for. They are like little routers which specify how the types and fields in the schema are connected to various backends
 “How do I get the data for this field?” or “Which backend do I need to call with what arguments to get the data for Posts?”
 GraphQL resolve functions can contain arbitrary code, which means a GraphQL server can to talk to any kind of backend, even other GraphQL servers. 
 For example, the Author type could be stored in a SQL database, while Posts are stored in MongoDB, or possibly handled by a microservice.
 
 Here are the three high-level steps the server takes to respond to the query:

## Parse/Validate/Execute
 
 Step 1: Parsing the query

 First, the server parses the string and turns it into an AST — an abstract syntax tree. If there are any syntax errors, the server will stop execution and return the syntax error to the client.

 Step 2: Validation

A query can be syntactically correct, but still make no sense. The validation stage makes sure that the query is valid given the schema before execution starts.
Validation is performed automatically, can the named fields be retrieved, are the arguments correct, etc.

 Step 3: Execution

 If validation succeeds, the GraphQL server will execute the query.

Queries implicitly support asynchronous requests - if a resolve function returns a promise, the executor will wait until that promise is resolved.
Each GraphQL query has the shape of a tree. Execution begins at the root of the query. First, the executor calls the resolve function of the fields at the top level with the provided parameters. It waits until all these resolve functions have returned a value, and then proceeds in a cascading fashion down the tree. 

See xxx for illustrations of this concept using a diagram/table/video

## Using GraphQL with MongoDB
https://www.compose.com/articles/using-graphql-with-mongodb/


## Summary
 Using GraphQL is a powerful way for client applications to control what data they retrieve and how they manipulate it without requiring them to adapt to what data a server's API offers. 

Opinions
 Multiple requests on a RESTful-API for just one thing often indicates a lack in the API design, namely the needed resource was not available and therefore stuff needs to be gathered from different resources to compensate for this.

A REST-API that could be easily replaced by GraphQL indicates, that the API was in fact a CRUD-HTTP-API, what is considered an Anti-Pattern among REST-Evangelists.

Also worth noting is, that GraphQL puts responsibilty on the client, because the backing API is reduced to be a datastore that just needs to be queried. REST on the other hand enforces the behaviour of the client and therefore reduces responsibility on it. The client gets reduced to be something similar to a browser.

There are cases the one or the other approach would yield better results, but that greatly depends on your situation.

one should consider the Pros after its implementation :

Very flexible to support new items and update existing behaviour.
Easy to add conditions using arguments and custom ordering once implemented
Use a lot of custom filters and get rid of all the actions that needs to be created example a user can have id, name, etc as arguments and perform the filtering. Additionally the filters can be applied on the groups in the users as well.
Ease of testing API by creating files containing all the GraphQL queries and mutations.
Mutations are straightforward and easy to implement once understood the concept.
Powerful way to fetch multiple depths of data.
Support of Voyager and GraphiQL UI or Playground makes it easy to view and use.
Ease of documentation while defining the schema with valid description methods.

cases which can be counted as disadvantages:

boilerplate excessiveness ( by this I mean, for the creating for example new query you need to define schema, resolver and inside the resolver to explicitly say GraphQL how to resolve your data and fields inside, on the client side create query with exactly fields related to this data)
error handling - I need to say that it's more related to comparison with REST. It's possible here with apollo but at the same time it's much more complicated than in REST
authentication and authorization - but as I said community is increasing with outstanding speed and there are already couple of solutions for this goal.
To sum up, GraphQL is just a tool for specific goals and for sure it's not a silver bullet to all problems and of course not a replacement for REST.

I have found some important concerns for anyone considering using GraphQL, and up until now the main points are:

Query In Indefinite Depth: GraphQL cannot query in indefinite depth, so if you have a tree and want to return a branch without knowing the depth, you’ll have to do some pagination.

Specific Response Structure: In GraphQL the response matches the shape of the query, so if you need to respond in a very specific structure, you'll have to add a transformation layer to reshape the response.

Cache at Network Level: Because of the commonly way GraphQL is used over HTTP (A POST in a single endpoint), cache at network level becomes hard. A way to solve it is to use Persisted Queries.

Handling File Upload: There is nothing about file upload in the GraphQL specification and mutations doesn’t accept files in the arguments. To solve it you can upload files using other kind of APIs (like REST) and pass the URL of the uploaded file to the GraphQL mutation, or inject the file in the execution context, so you’ll have the file inside the resolver functions.

Unpredictable Execution: The nature of GraphQL is that you can query combining whatever fields you want but, this flexibility is not for free. There are some concerns that are good to know like Performance and N+1 Queries.

Super Simple APIs: In case you have a service that exposes a really simple API, GraphQL will only add an extra complexity, so a simple REST API can be better.

Disadvantages:

You need to learn how to set up GraphQL. The ecosystem is still rapidly evolving so you have to keep up.
You need to send the queries from the client, you can just send strings but if you want more comfort and caching you'll use a client library -> extra code in your client
You need to define the schema beforehand => extra work before you get results
You need to have a graphql endpoint on your server => new libraries that you don't know yet
Graphql queries are more bytes than simply going to a REST endpoint
The server needs to do more processing to parse the query and verify the parameters
But, those are more than countered by these:

GraphQL is not that hard to learn
The extra code is only a few KB
By defining a schema, you will prevent much more work afterwards fixing bugs and enduring hairy upgrades
There are a lot of people switching to GraphQL so there is a rich ecosystem developing, with excellent tooling
When using persistent queries in production (replacing GraphQL queries with simply an ID and parameters), you actually send less bytes than with REST
The extra processing for incoming queries is negligible
Providing a clean decoupling of API and backend allows for much faster iteration on backend improvenments

## Errors
here are a few different types of errors:

GraphQL Errors: errors in the GraphQL results that can appear alongside successful data
Server Errors: server internal errors that prevent a successful response from being formed
Transaction Errors: errors inside transaction actions like update on mutations
UI Errors: errors that occur in your component code
Apollo Client Errors: internal errors within the core or corresponding libraries

What is a resolver?
The schema tells the server what queries clients are allowed to make, and how different types are related.
They don't tell you where the data for each type comes from. That’s what resolve functions are for.
Resolve functions are like little routers. They specify how the types and fields in the schema are connected to various backends, answering the questions “How do I get the data for Authors?” and “Which backend do I need to call with what arguments to get the data for Posts?”.

GraphQL resolve functions can contain arbitrary code, which means a GraphQL server can to talk to any kind of backend, even other GraphQL servers. For example, the Author type could be stored in a SQL database, while Posts are stored in MongoDB, or even handled by a microservice.

Perhaps the greatest feature of GraphQL is that it hides all of the backend complexity from clients. No matter how many backends your app uses, all the client will see is a single GraphQL endpoint with a simple, self-documenting API for your application.

What is a Schema?
The type system is often declared in schema (IDL) files which contain entities, their attributes, interfaces, and enumerations, as well as mutations and subscriptions. 
The schema can be extended with custom directives and types that allow specific extensions of the core language.

What is Relay?
Relay is the first product that worked with GraphQL. It was developed by Facebook and is part of a suite of products that provide their back/front end connectivity support


Some Approaches to Adding Custom Logic to the GraphQL API

From Neo4j
GraphQL is a specification for querying a slice of an application graph, retrieving a tree of data that perfectly matches a front-end view, regardless of where that data was pulled from. It covers tree-based read queries, mutations for updates, and subscriptions for live updates.
Queries in GraphQL form a tree structure, based upon entities and relevant attributes. Any of those can take parameters (e.g for filtering or pagination) and directives.

At a high level, GraphQL builds a contract between front-end and back-end, based on the agreed-upon type system, which forms an application data model in the form of a graph.

It decouples the front-end from the back-end data source(s), which allows you to change either piece independently while the type system stays consistent. Each query from the front-end is using that type system to precisely define which data it is interested in.

There is no prescription to restrict where the backend data comes from or how it is returned – it can be anything from databases to APIs, from third party systems to in-memory representations, and even from/to code or static assets.

GraphQL queries return data in a tree form that perfectly matches the frontend view hierarchy. If the application data is a graph, then the perfect backend is a graph database with native support for resolving GraphQL queries – for example: Neo4j

## What Deficiencies does it address?
-  A REST API typically delivers all the data a client UI might need about a resource and leaves it up to the client to extract the bits of data it actually wants to show. If that data is not a resource it already has, then the client needs to go off to the server and request some more data from another URL. Over time, the link between front-end applications and back-end servers can become pretty rigid. As data gets more complex in structure, it gets harder and slower to query with traditional tools.

