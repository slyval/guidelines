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
 - 




> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbNDQzMzA0NDcsLTYxOTM5NjIxLDIxNDE1Nj
EzNThdfQ==
-->