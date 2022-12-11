# API Design

# Resource
- [Best practices for REST API design](https://stackoverflow.blog/2020/03/02/best-practices-for-rest-api-design/)
- [Best Practices in API Design](https://swagger.io/resources/articles/best-practices-in-api-design/)
- [RESTful web API design](https://learn.microsoft.com/en-us/azure/architecture/best-practices/api-design)

# RESTful web API design

## A well-designed web API should aim to support:
- Platform independence
- Service evolution

## What is REST?
- Representational State Transfer
- style for building distributed systems based on hypermedia
- REST is independent of any protocol and is not necessarily tied to HTTP

### main design principles of RESTful APIs using HTTP
- designed around resources(object, data, service)
- resource has an identifier, which is a URI that uniquely identifies that resource
- Clients interact with a service by exchanging representations of resources
    - exchange format
        - JSON
- use a uniform interface
    - helps to decouple the client and service implementations
- use a stateless request model
    - HTTP requests should be independent and may occur in any order
        - keeping transient state information between requests is not feasible
        
        
## Organize the API design around resources
Focus on the business entities that the web API exposes
- resource URIs should be based on nouns (the resource) and not verbs (the operations on the resource)
- resource doesn't have to be based on a single physical data item
    - resource might be implemented internally as several tables in a relational database,but presented to the client as a single entity
- Avoid creating APIs that simply mirror the internal structure of a database
    - The purpose of REST is to model entities and the operations that an application can perform on those entities
    - A client should not be exposed to the internal implementation

### Collections and Entities
- collection is a group of entities
- collection is a separate resource from the item
- collection should have its own URI

### Relations of resources
- consider the relationships between different types of resources and how you might expose these associations. 
    - For example, the /customers/5/orders might represent all of the orders for customer 5
    - But, extending this model too far can become cumbersome to implement
    - A better solution is to provide navigable links to associated resources in the body of the HTTP response message. 
        - Use HATEOAS to enable navigation to related resources
    - more complex systems, it can be tempting to provide URIs that enable a client to navigate through several levels of relationships
        - such as `/customers/1/orders/99/products`
        - However, this level of complexity can be difficult to maintain and is inflexible if the relationships between resources change in the future. 
        - try to keep URIs relatively simple
        - Once an application has a reference to a resource, it should be possible to use this reference to find items related to that resource
        - The preceding query can be replaced with the URI 
            - /customers/1/orders to find all the orders for customer 1
            - then /orders/99/products to find the products in this order

### load
- all web requests impose a load on the web server
    - try to avoid "chatty" web APIs
        - large number of small resources.
        - may require a client application to send multiple requests to find all of the data that it requires
        - fix: denormalize the data and combine related information into bigger resources that can be retrieved with a single request
- need to balance this approach against the overhead of fetching data that the client doesn't need
    - Retrieving large objects can increase the latency of a request and incur additional bandwidth costs

### Avoid introducing dependencies between the web API and the underlying data sources
EX:
- your data is stored in a relational database, the web API doesn't need to expose each table as a collection of resources
    - probably a poor design
- think of the web API as an abstraction of the database
- introduce a mapping layer between the database and the web API
    - client applications are isolated from changes to the underlying database scheme


### might not be possible to map every operation implemented by a web API to a specific resource
can handle such non-resource scenarios through HTTP requests that invoke a function and return the results as an HTTP response message

## Define API operations in terms of HTTP methods
- GET
    - 200 (OK)
    - 204 (No Content)
    - 404 (Not Found)
- POST
    - 201 (Created)
    -  does some processing but does not create a new resource, the method
        -  200
        -  if there is no result to return
            -  204 (No Content)
    - 400 (Bad Request) 
- PUT
    - 200 (OK)
    - 204 (No Content)
    - 409 (Conflict)
- PATCH
    - specification for the PATCH method (RFC 5789) doesn't define a particular format for patch documents
    - The media type for JSON patch is application/json-patch+json
- DELETE
    - 204 (No Content)
    - 404 (Not Found)

common conventions adopted by most RESTful implementations using the e-commerce example

| **Resource** | **POST** | **GET** | **PUT** | **DELETE** |
| --- | --- | --- | --- | --- |
| /customers | Create a new customer | Retrieve all customers | Bulk update of customers | Remove all customers |
| /customers/1 | Error | Retrieve the details for customer 1 | Update the details of customer 1 if it exists | Remove customer 1 |
| /customers/1/orders | Create a new order for customer 1 | Retrieve all orders for customer 1 | Bulk update of orders for customer 1 | Remove all orders for customer 1 |

## Conform to HTTP semantics
- Media types
    - formats are specified through the use of media types -> MIME types
        - JSON (media type = application/json)
        - XML (media type = application/xml)

:::info
Content-Type header in a request or response specifies the format of the representation
A client request can include an Accept header that contains a list of media types the client will accept from the server in the response message
If the server doesn't support the media type, it should return HTTP status code 415 (Unsupported Media Type).
:::

## Asynchronous operations
sometimes operation might require processing that takes a while to complete
- POST
- PUT
- PATCH
- DELETE 

consider making the operation asynchronous
- Return 202 (Accepted)
    - indicate the request was accepted for processing but is not completed
- the client sends a GET request to this endpoint
    - the response should contain the current status of the request
    - it could also include an estimated time to completion or a link to cancel the operation
- asynchronous operation creates a new resource, the status endpoint should
    - return status code 303 (See Other)
    - Location header that gives the URI of the new resource

### Empty sets in message bodies(Any time the body of a successful response is empty)
- 204 (No Content)
- 200 (OK)

### Filter and paginate data
suppose a client application needs to find all orders with a cost over a specific value
supporting query strings that specify the maximum number of items to retrieve and a starting offset into the collection

consider imposing an upper limit on the number of items returned

### Support partial responses for large binary resources
A resource may contain large binary fields, such as files or images
- to improve response times, consider enabling such resources to be retrieved in chunks
  - web API should support the Accept-Ranges header for GET requests for large resources
  - header indicates that the GET operation supports partial requests
- The response message indicates that this is a partial response by returning HTTP status code 206

## Use HATEOAS to enable navigation to related resources
- One of the primary motivations behind REST is that it should be possible to navigate the entire set of resources without requiring prior knowledge of the URI scheme
- Each HTTP GET request should return the information necessary to find the resources related directly to the requested object through hyperlinks included in the response
- This principle is known as HATEOAS, or Hypertext as the Engine of Application State
- The system is effectively a finite state machine, and the response to each request contains the information necessary to move from one state to another; no other information should be necessary

## Versioning a RESTful web API
- As business requirements change new collections of resources may be added
- The primary imperative is to enable existing client applications to continue functioning unchanged while allowing new client applications to take advantage of new features and resources
- Versioning enables a web API to indicate the features and resources that it exposes, and a client application can submit requests that are directed to a specific version of a feature or resource

several different approaches, each of which has its own benefits and trade-offs
- No versioning
- URI versioning
- Query string versioning
  - `https://adventure-works.com/customers/3?version=2`
  - has the semantic advantage that the same resource is always retrieved from the same URI
  - but it depends on the code that handles the request to parse the query string and send back the appropriate HTTP response
- Header versioning
  - Rather than appending the version number as a query string parameter, you could implement a custom header that indicates the version of the resource
  - `Custom-Header: api-version=1`

## Media type versioning
- When a client application sends an HTTP GET request to a web server it should stipulate the format of the content that it can handle by using an Accept header
- purpose of the Accept header is to allow the client application to specify whether the body of the response should be XML, JSON, or some other common format that the client can parse
- it is possible to define custom media types that include information enabling the client application to indicate which version of a resource it is expecting

> When you select a versioning strategy, you should also consider the implications on performance, especially caching on the web server
> The URI versioning and Query String versioning schemes are cache-friendly inasmuch as the same URI/query string combination refers to the same data each time.

> The Header versioning and Media Type versioning mechanisms typically require additional logic to examine the values in the custom header or the Accept header.
> In a large-scale environment, many clients using different versions of a web API can result in a significant amount of duplicated data in a server-side cache. 
> This issue can become acute if a client application communicates with a web server through a proxy that implements caching, and that only forwards a request to the web server if it does not currently hold a copy of the requested data in its cache.

## Open API Initiative
- The OpenAPI Specification comes with a set of opinionated guidelines on how a REST API should be designed. That has advantages for interoperability, but requires more care when designing your API to conform to the specification.
- OpenAPI promotes a contract-first approach, rather than an implementation-first approach. Contract-first means you design the API contract (the interface) first and then write code that implements the contract.
- Tools like Swagger can generate client libraries or documentation from API contracts

# Next steps
- [Microsoft REST API guidelines](https://github.com/Microsoft/api-guidelines/blob/master/Guidelines.md). Detailed recommendations for designing public REST APIs.
- [Azure REST API guidelines](https://github.com/microsoft/api-guidelines/blob/vNext/azure/Guidelines.md). Design guidelines for Azure REST APIs.
- [Web API checklist](https://mathieu.fenniak.net/the-api-checklist/). A useful list of items to consider when designing and implementing a web API.
- [Open API Initiative](https://www.openapis.org/). Documentation and implementation details on Open API.
# Q
- what's the difference between
    - resource
    - collection
    - entity