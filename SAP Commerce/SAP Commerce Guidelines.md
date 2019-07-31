# SAP Commerce Guidelines

## Purpose
The purpose of this document is to provide guidance in terms of structure and conventions to be adhered to as best as possible when implement  SAP Commerce on-premise or SAP Commerce Cloud. 

This is a living document and will be updated from time to time. Ensure that you always reference this from the source to keep abreast of any updates. 

## Solution
	
##### Version
The commerce utilized should always be the latest version unless the customer has a specific choice

##### Initialisation 
When starting a commerce project:
- A development configuration must be used that will generate the **config** folder
- When doing a B2C or B2B solution with a webshop or storefront:
	- The initial generation of the projects artifacts must be done through the corresponding recipe or must be manually generated using the ***modulegen*** ant task
	- When generating manually, the tempate that must be used is ****accelerator****
	- A good name must be chosen for the project as that will affect names of extensions. A good starting point will be  to use the project name
	- A root package must be chosen carefully and must loosely follow the following convention:
		> com.\<customername>.\<projectname>
		
- Similar guidelines as above must be used for other projects as advised in SAP Commerce help documentation

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

- When overriding a standard extension, the prefix ***custom*** must be included. An exception is when a customer requests otherwise
	> Examples: customsapintegrationservices

- Names are important. The name must convey intent. Therefore, 
	- An extension for services must contain the suffix ***services***
	- An extension for facades must contain the suffix ***facades***
	- An extension for data access objects must contain the suffix ***daos*** if applicable
	> Examples: sapordermanagementservices, commercefacades

##### Generation 
- All extensions must be genarated using the **extgen** ant target/task. 
- The following template extensions must be used:
	- ****yempty**** should be used to generate all extensions except the ones below
	- ****yaddon**** should be used to generate all addons
	- ****ywebservices**** or ****ycommercewebservices**** should be used for all web service (RESTful) extensions
	- ****ybackoffice**** should be used to generate all backoffice related extensions
	
- All other template extensions not listed above should be used in their specific scenarios as they are not generic in nature

### Data Model

#### Naming Conventions
The names of items, relationa and Enums must:
- Contain only alphanumeric characters
- Must use **Pascal** case e.g., Order

<!--stackedit_data:
eyJoaXN0b3J5IjpbODY5NzY5MDM2LC0yMDI5NzQ3NjUwLC02Mz
AxNTM3ODAsMTYyMDE3MDA5MCw5NDg2OTE5NDAsMTc3NTQ3NTQx
LC04MTg3NDM3NjddfQ==
-->