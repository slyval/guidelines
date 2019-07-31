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
	- Be descriptive of the larger purpose for which it is being created
	> Example: customordermanagementservices
	
- An addon is a special type of an extension and must have the suffix ***addon***
	> Example: customb2baccelratoraddon

- When overriding a standard extension, the prefix must be either ***custom*** or a name aggreed 
<!--stackedit_data:
eyJoaXN0b3J5IjpbNDY4NTM5NTk4LDE3NzU0NzU0MSwtODE4Nz
QzNzY3XX0=
-->