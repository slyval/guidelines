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
2XX Codes represent Success
200 - OK  - should be used for GET and PUT requests
201 - Created - should be used with POST requests that create
204 - No Content- should be used with DELETE requests

4XX Codes represent client error
400 - bad request - means that the request sent through is malformed
401 - unauthroized - means the client could not be authenticated 
403 - forbidden - means the client could not be authorized
404 - not found - means what the client requested is not available 
405  - method not allowed - means the client is using an incorrect method
410 - gone - implies that the resource the client is looking for is no longer available

5XX Code represent server errors
500 - internal server error - represents an exceptional issue encountered by the systemm
502 - 
503 - service unavailable - represents that the system is not able to process the request at the moment
##### Error handling
An error payload must be used in conjunction status codes where errors have occured in order to provide context and detail. The structure should be as follows and additional attributes can be added:
			{
				"errors":[
					"code":"000"
				   "message": "the message"
				]
			}
##### Searching, Filtering, Sorting, Pagination
This should only be used with GET methods that also 
###### Searching 
A parameter can be provided that provides the search query - e.g ?search=xxxxxxxx
###### Filtering
This should use actual attributes in the resource being searched to return a specific collection or resource and should be in the format ?\<attribute>=\<value> 
###### Sorting
Like filtering, the actual attributes in the resource being searched should be used to sort. The format should be in the format ?sort=\<attribute> & order=ASC/DESC
###### Paging
Paging should be used to limit the package size of the collection returned so as to provide performant implementations. The format should be ?page=\<page number> & size=\<page size>

##### Versioning 
In order to accomodate future changes without breaking existing clients, it is important to consider versioning where external clients are expected. To specify a version, a versioning id must prefix the api endpoints .eg v1/orders/1


> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyNTc1ODE5OTIsODIwMjg1NTkxLDk2Mj
g2MDQ1MywxMTgyNTcyMjczLC00OTIyMzcwOSwtOTY3OTMwNDU5
LC0xMTE0MzE3MTcxLDE0MjE4MjE0NTksMTcwNDcyMjA3NywxNj
cwNTE2Mjg1LC0xMjc5NDk3MjA5LC02MTkzOTYyMSwyMTQxNTYx
MzU4XX0=
-->