
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

#### Collections 
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

<!--stackedit_data:
eyJoaXN0b3J5IjpbOTIyODY5NzU5LDE5NjA3MjcwMDQsMTE1Nz
M1MTUwMl19
-->