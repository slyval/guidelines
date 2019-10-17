RESTful API design notes 

key elements:
- resource - object or asset 
- collections - a set of resources 
- url(uri) - an identifying or locating path to a resouce or collection. This is different from an endpoint 
- endpoint - an interface to performing actions against resources and collections

endpoints:
 - endpoint URLs must only contain nouns(resources or collections) and/or indentifiers
 - no verbs or actions must make part of an endpoint URL
 - verbs come from the HTTP methods(GET, POST, PUT, DELETE)
 - the resource noun must always be plural e.g orders, entries, products
 - when acting on one object, the identifier e.g order number or pk or guid , is used in conjunction with the plural for resources e.g orders/{order number}
 - where a resource has children,  extend the endpoint to show the relationship  e.g orders/1/entries or classes/1/students/20
 
 methods(actions)
 - these are the verbs that define the actions to be performed against an o
 




> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIwNzUzNTU4NTIsLTYxOTM5NjIxLDIxND
E1NjEzNThdfQ==
-->