
# REST API Guidlines

## Purpose
The purpose of this document is to provide guidelines that are helpful when designing REST based APIs or web services. 

## Why
There are, broadly, two styles for designing web based APIs - SOAP and REST. 
SOAP is not only a style, but also a protocol. It uses various protocols, including HTTP, for the transfer of data. 
REST is simply an architectural style for designing web based APIs that relies on the HTTP protocol for the transfer of data. It stands for Representational State Transfer.REST is more flexible, architecturally sound, scalable and web friendly than SOAP.  
When designing web APIs, it is advisable to first consider using REST and only resort to using SOAP when necessary. 

## Principles
Outlined below are different principles that should be considered and followed when designing REST APIs

To foster better understanding, an ordering system that deals with orders and order entries will be used in the examples that will be provided

### Understanding Key Elements
There are a few elements whose meaning is important and should be understood well in order to successfully design REST APIs. They are:

#### Resource
A resource is a key concept in REST. It represent the entities or objects that users interact with. 

    Examples of resources are: Order, Entry

#### Collection
A collection is a set of resources. As an example, a set of order resources is referred to as a collection of orders

#### URL (Uniform Resource  Locator) or URI (Uniform Resource Identifier)
These are often used interchangeably to indicate the path that when followed leads to the location of a resource or collection. URLs are often used in defining links in the response body 

#### Endpoint 
An endpoint refers to the an interface through which users interact with the API. An API is composed of all the different endpoints.

### Endpoints 
When designing REST API endpoints, it is important to note the following:

 - Endpoint must only use **nouns** and not *verbs*. The nouns must only relate to the resources or collections under consideration. 	
 - The nouns pertaining to the resources must be in **plural** form. As an example when dealing with the Order resource, the noun must be *Orders*
 - An endpoint for a specific instance of a resource must include a uniquely identifying attribute for the resource. An example can be the order number or primary key. It is usually better to use a human-readable identifier
 - Where relationships between resources exist, these must be reflected in the endpoint by first identifying the parent and then the child

Below are examples to illustrate the above :

    /orders
    /orders/1000
    /orders/1000/entries
    /orders/1000/entries/1


### Methods
As explained in the "endpoints" section, only nouns must be used when creating an endpoint. One of the main mistakes that are made when designing REST APIs is to used a verb to indicate an action to be performed against a resource. The following are wrong examples of endpoints:

    /getAllOrders
    /getOrder/1000
Though the above reads intuitive, it does not follow best practice and leads to the creation of redundant APIs and is difficult to maintain. 

With the above said, it is still necessary to represent an action or verb that represents the desired interaction. As REST APIs use HTTP for data transfer, the verbs are already taken care of as a rfesult of the utilization of HTTP methods. Those methods communicate the intent. 

As APIs essentially represent CRUD operations, the GET, POST, PUT/PATCH and DELETE methods should suffice for all required interactions. 

 1. GET - The method should be used for retrieval of a resource or a collection. When using a GET, no side effects on the server side must occur as this is simply pulling information. If a side effect is desired, a different method such as POST should be used. Unless the state of the system has changed, the same request should have the same result.
 2. POST - This method is used when creating a resource e.g when creating an order. This results in side effects as the state of the system is changed with the addition of a resource. The same request sent multiple times might result in different results as creation of duplicates might be forbidden.
 3. PUT - This method should be used for updating a resource and results in side effects. The same request sent multiple times should essentially have the same result.
 4. DELETE - The delete method should be utilized when removing a resource. Side effects are expected and the different results should be expected when the same request is sent multiple times as the first request would have deleted the resource

### Headers 
As REST used HTTP for data transfer, request headers should be utilized for at least the following purposes:

 1. Authentication and Authorization -  Authorization header  - should be used whether using basic or oAuth authentication, the header should be used. Passing of username and password parameters as part of the endpoint URL should be avoided. 
 2. Serialization Format - Accept and Content-Type headers - should be used to specify the desired data format to be returned to the client or the data format used in the request body. Common formats are JSON and XML. 
 3. Caching - Cache-Control and ETag headers - should be used in the response to communiciate caching instructions to the client 

Other headers can be use

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTcwMjkwMTU5OCwxNTgyNTg0NDU0LC0xOD
EwOTc5NjA5LC0xMTIwNjY3NDkxLC0xMzU3MzU2NjQ2LDE5NjA3
MjcwMDQsMTE1NzM1MTUwMl19
-->