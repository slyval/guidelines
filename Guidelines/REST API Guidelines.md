
# REST API Guidlines

## Purpose
The purpose of this document is to provide guidelines that are helpful when designing REST based APIs or web services. 

## What?
REST stands for  REpresentational State Transfer. It is an archtectural style to developing web based API. Other alternative mechanisms include the SOAP based RPC and GraphQL. 

There are several characteristics that help to describe REST based APIs. Some of the detail on these will be covered in the **Principles** section. The basic characteristics are that REST APIs are:

 1. resource based rather than action based
 2. stateless rather than stateful
 3. based on the client-server design approach 
 4. cacheable 
 5. based on the transfer of resource representations ( complete state of resources) over HTTP. These representations can be serialized in different formats - more commonly JSON. 

## Design Guidelines

When designing  REST APIs or web services, there are multiple principles and design considerations that need to be followed and understood. The sections below these considerations as well as some important information that needs to be understood. 

To foster better understanding, an ordering system that deals with orders and order entries will be used in the examples that will be provided

### Core Elements

There are a few elements whose meaning is important and should be understood well in order to successfully design REST APIs. They are:

#### Resource
A resource is a key concept in REST. It represents the entities or objects (**things**) that users interact with. 

    Examples of resources are: Order, Entry

#### Collection
A collection is a set of resources. As an example, a set of order resources is referred to as a collection of orders

####  URI (Uniform Resource Identifier)
A URI is used to indicate the path that when followed leads to the location or identification of a resource or collection. URIs are often used in defining links in the response body

#### Endpoint 
An endpoint refers to the an interface through which users interact with the API. An endpoint is formed out of the actio resources, identifiers and parameters.  An full API is composed of all the different endpoints.

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


### Actions or Methods
As explained in the **endpoints** section, only nouns must be used when creating an endpoint. One of the main mistakes that are made when designing REST APIs is to used a verb to indicate an action to be performed against a resource. The following are wrong examples of endpoints:

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

Other headers may be used where necessary.

### Response
When a request is sent to an API endpoint, a response is expected. A response is expected to communicate the success or failure of a request. 

Two elements are involved in a response.  These are the response status, communicated using status codes, and the response payload. A wide array of HTTP status codes should be used rather than just 200 (success), 400 (failure) and 500 (error) as is often the case with poorly designed APIs. 

#### Status Codes
HTTP status codes should be used in the response to indicate the status of the processing of the request. 
The following is a list of some of the codes and when they should be used. 
##### 2xx - Success 

 1. 200 - OK - this should be used with GET and PUT methods to indicate that the request has been successful
 2. 201 - Created - this should be used with the POST method to indicate that the requested resource has been created 
 3. 204 - No Content - this should be used with the DELETE method to indicate that the deletion has been successful and no content exists anymore

##### 3xx - Redirection
1. 304 - Not  Modified - this should be used with a GET method to indicate that the resource has been cached by the client and that the resource has not changed since the last time it was requested

##### 4xx - Client Error 

 1. 400 - Bad Request - this should be used to indicate that the request sent through is not formatted as expected 
 2. 401 - Unauthorized - this should be used to indicate that the client has not been successfully authenticated. This results when either no credentials have been provided or the provided credentials are incorrect
 3. 403 - Forbidden - this is ised to indicate that the client has is not authorized to perform the requested action for the given resource
 4. 404 - Not Found - this should be utilized to indicate that the resource under consideration cannot be found 
 5. 405 - Method Not Allowed - this should be utilized to indicate that the requested action against a resource is not supported for the endpoint
 6. 410 - Gone - this can be used to indicate that though the resource used to exust, it's no longer available

##### 5xx - Server Error 

 1. 500 - Server Error - this should be used to indicate that an unexpected error has occured while performing the requested action 
 2. 501 - Not Implemented - this should be used to indicate that the desired action is not yet implemented or functioanal
 3. 503 - Service Unavailable - this should be used to indicate that the server could not process the requested action at the time of request. This could be provided as a response when server's capacity to handle additional request is not available
 4. 504 - Gateway Timeout - this can be used to indicate that a timeout occured when communicating to an upstream service

The above status codes are mainly meant to provide guidelines when impementing the server side. It should be noted that other status codes may be raised by the hosting systems and must be handled accordingly when developing a client. 

#### Payload
As REST is a a state tranfer mechanism, the response payload is used for that purpose and contains either resources/collections or error messages.
 
The representations can be serialized in multiple formats that include JSON and XML. Usually the conversion is achieved by converting an object representation in a particular language to these formats. 

When designing the resources or response objects for error messages, it is advisable to follow a naming convention that ensures that all atrributes are *camelCased*. 

Additionally, JSON should be prefereed over XML, which should be provided as a fallback. 

##### Successful Requests ( Success Responses )
When a request has been successful, a payload is normally expected for most of the methods. The following should be used as a guideline: 

 1. GET - a response payload is expected to contain the retrieved resources or collection. 
 2. POST - a response is optional. If provided, it should ideally contain the created resource
 3. PUT - a response is optional. If provided, it should ideally contains the updated resource
 4. DELETE - a response payload is not expected

##### Error Messages
When an error has a occured, resulting in request failure indicated by the status code, a response body must include an error payload. The structure of the error payload should be as follows. :

			    "error":{
    					"code":"000"
    				    "message": "the error message"
    				    "type": "TheErrorType"
	    				}
Additional attributes for the error may be added as is necessary. Some might like to include an internal error message that is technical and additional contextual information.

### Searching, Filtering, Sorting and Paging
Searching, filtering, sorting and paging are additional requirements that all non-trivial APIs should cater for. These enable ease of finding resources and also seek to meet performance and navigation requirements. 

To achieve these requirements, usage of HTTP parameters is essential as is discussed below. 

It is possible to combine searching, filtering , sorting and paging parameters as desired. When handling these in a controller, it is advisable to use optional parameters.

#### Searching 
Searching is an essential requirement that is often incorrectly achieved by using verbs and malformed endpoints. Also, searching should not be confused with filtering which is covered in the next section. 

In order to handle searching, a parameter named **query** or **q** for short should be used. Only if the resource under consideration has a similarly named attributed should a different name be used. The parameter should be in the form **?q=\<seach string>**

An example of a searching endpointis shown below.

    /orders?q=123
 
 This implies that the string "123" will be searched for accross the attrbutes of the Order resource and matching results will be returned. 

#### Filtering 
Filtering is also essential when clients want a specific set of results that have a particular attribute or set of attributes. 

To handle filtering, predetermined parameters whose names similar to the attributes of a resource should be used. The formatting of the parameters should resemble **?\<attribute_name>=<attribute_value>**

An example is as follows

    /orders?customer=test&status=new

The above implies that all "new" orders placed by "test" will be retrieved. 

#### Sorting 
Sorting is essential when the order of results is important. 

To handle sorting, predetermied parameter values whose names either equate to or are mapped to resource attribute names should be used in  comobination with a the format **?sort=\<attribute_name>** or **?sort=\<attribute_name>&order=<ASC/DESC>**
With the first option,  the optional paramter *order* is not specified because a default value of either ASCending or DESCending can be assumed. 

An example is as follows

    /orders?sort=code
    /orders?sort=code&order=DESC

The first example returns all orders sorted by code in ascending order as ASC is assumed while the second example returns the same collection but in descending order.

#### Paging
There are instances when the results are needed in a paged manner. This is usually the case where performance, payload size or client characteristics dictate the need to limit collection sizes. 

To achieve paging, two parameters shoud be used and these should be named **limit** and **offset**.  The format is **?limit=\<value>&offset=\<value>**. 

The **limit** parameter is used to reflect the number of records in the expected collection. The parameter **offset** represents the starting index. 

An example would be:

    /orders?limit=50&offset=0
 
 The above example returns a collection of the first possible 50 orders from a possible collection of all orders. 

### Caching
One of the key characteristics of a RESTful API is that responses should be cacheable. 

Explicit caching should be exercised where the server denotes to the client how long the response or resource should be cached for (max-age) after which the client can make another request and get a possibly updated response. 

Caching can be exercised on the server side in order for multiple requests from different clients to be served with the same response without expensing resources on processing. 

### State - Statelessness
REST APIs should be stateless by design. A request must not depend on what happened before or be depended upon by subsequent requests. 

This means that a request must have all the information that it needs to carry out the required server side processing and successfully terminating. 

The session for a request being when the request is issues and ends when the response is returned. 

### Versioning 
When external clients are allowed to access an API, it is important that a versioning system is put in place. The benefit of versioning is that backwards compatibility can be adherred to when necessarry. The old version will allow clients to keep using older APIs until they are ready to upgrade to the latest newer version. 

A good practice would be to use the version id e.g **v1, v2, v3** before the endpoint. A good example is ads follows:

    /ourapi/v1/orders

 
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTA4ODQwNzA4OCwtNTA1MDQ2NDI2LDE0NT
c5MzU0OTQsLTE1MzYyNjE4MTQsLTE2OTk3NTUzODQsLTI5MzQ0
MTUyMCwxNTgyNTg0NDU0LC0xODEwOTc5NjA5LC0xMTIwNjY3ND
kxLC0xMzU3MzU2NjQ2LDE5NjA3MjcwMDQsMTE1NzM1MTUwMl19

-->