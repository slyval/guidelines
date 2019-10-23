
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

### En
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTQyMDQ1MzI4NywxOTYwNzI3MDA0LDExNT
czNTE1MDJdfQ==
-->