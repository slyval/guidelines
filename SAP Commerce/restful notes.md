RESTful API design notes 

##### key elements:
- resource - object or asset 
- collections - a set of resources 
- url(uri) - an identifying or locating path to a resouce or collection. This is different from an endpoint 
- endpoint - an interface to performing actions against resources and collections

##### endpoints:
 - endpoint URLs must only contain nouns(resources or collections) and/or indentifiers
 - no verbs or actions must make part of an endpoint URL
 - verbs come from the HTTP methods(GET, POST, PUT, DELETE)
 - the resource noun must always be plural e.g orders, entries, products
 - when acting on one object, the identifier e.g order number or pk or guid , is used in conjunction with the plural for resources e.g orders/{order number}
 - where a resource has children,  extend the endpoint to show the relationship  e.g orders/1/entries or classes/1/students/20

##### methods(actions)
 
 - these are the verbs that define the actions to be performed against an object
 - 4 of these must be used to perform the CRUD  actions. These are:
	 - GET - for retrieval or reading of a resource or a collection. No side effects must occur, if any use the other verbs. 
	 - POST - for creation of a resource. This results in side effects, if none use GET. The same request performed multiple times might result in different results( as duplicates might be rejected)
	 - PUT - for updating of a resource or creation when resource does not exist (insert or update). Side effects are expected. Multiple requests should return the same result due to insert/update nature
	 - DELETE - for the removal or deletion of a resource or collection. Side effects are expected and multiple requests will not have the same result as the deletion of an inexistent object would fail

##### response and response codes
- the response codes must communicate the success or failure of the request.
- failure can result from a wrong request or a malformed request or a state of the system
- HTTP status codes as response codes

###### status codes 
200 - should be used for GET and PUT requests
201 - should be used with POST requests that create
204 - should be used with DELETE requests

 




> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTcwNDcyMjA3NywxNjcwNTE2Mjg1LC0xMj
c5NDk3MjA5LC02MTkzOTYyMSwyMTQxNTYxMzU4XX0=
-->