# SAP Commerce Guidelines

## Purpose
The purpose of this document is to provide guidance in terms of structure and conventions to be adhered to as best as possible when implement  SAP Commerce on-premise or SAP Commerce Cloud. 

This is a living document and will be updated from time to time. Ensure that you always reference this from the source to keep abreast of any updates. 

## Solution
	
##### Version
The commerce utilized should always be the latest version unless the customer has a specific choice

## Development Environment

##### Integrated Development Environment
- The IDEs of choice are Eclipse, Spring Tool Suite (STS), IntelliJ IDEA.
-  Install a decompiler for use when debugging or inspecting code if your IDE does not have an out of the box decompiler
- Install SonarLint for static code checking. This should be integrated to a central SonarQube for rules

## Development Artifacts

### Extensions
##### Naming  Conventions
- An extension name must: 
	- Only contain alphanumeric characters
	- Only contain lowercase letters
	> Example: customordermanagementservices
	
- An addon is a special type of an extension and must have the suffix ***addon***
	> Example: customb2baccelratoraddon

- When overriding a standard extension, the prefix must be either ***custom***. As an exception a customer request can be accomodated if there is a request to use a specific prefix
	> Examples: customsapintegrationservices

- Names are important. The name must convey the intent. The purpose of the extension must be conveyed by the name
	- An extension for services must contain the suffix ***services***
	- An extension for facades must contain the suffix ***facades***
	- An extension for data access objects must contain the suffic ***
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQwOTM2NTU5LDE3NzU0NzU0MSwtODE4Nz
QzNzY3XX0=
-->