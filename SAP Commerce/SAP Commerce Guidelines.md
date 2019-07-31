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
#### Naming  Conventions
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

#### Generation 
- All extensions must be genarated using the **extgen** ant target/task. 
- The following template extensions must be used:
	- ****yempty**** should be used to generate all extensions except the ones below
	- ****yaddon**** should be used to generate all addons
	- ****ywebservices**** or ****ycommercewebservices**** should be used for all web service (RESTful) extensions
	- ****ybackoffice**** should be used to generate all backoffice related extensions
	
- All other template extensions not listed above should be used in their specific scenarios as they are not generic in nature


### Data Model

#### Items, Relations and Enums
- For items, relations and Enums; the names must:
	- Contain only alphanumeric characters
	- Must use **Pascal** case
	 
			 Examples: OrderTypes as an enum, Order2InvoiceRel for a relation, Order for an item type
		 
- Except where necessary, the name of the deployment table must be the same as the name of the item or relation
- For item and relation attributes, **Camel** case must be utilised
	
		Example: code, deliveryDate

- For Enums, the names of the value codes must be in **Upper** case
		
		Example: SUNDAY 

- Except when aboslutely necessary, do not specify the jaloclass attribute for an item type

		Incorrect Example: <itemtype code="Order" ....  jaloclass="com.test.jalo.components.order">
	
		Correct Example: <itemtype code="Order" .... >

#### Item Attributes

- Where an attribute can contain a static list of values, an Enum must be created and used to type the attribute

- Where multiple values are possible for an attribute, a relation to an Enum or another item must be created
-  Avoid creating circular references where an attribute is typed to the item it belongs to
- Collection types must not be used for any attribute. A relation of one-to-many cardinality must be created instead.

### Classes 

#### Packages
Packages are containers and must group functionality accordingly. 

All package names must be prefixed with: 
		
		com.<customer_name>.<project>.<extension>. or 
		com.<customer_name>.<extension> or 
		com.<project_name>.extension
		
#### Interfaces 
An interface defines a contract with the outside world. It defines all the publicly accessible methods that external dependents can interact with. Therefore: 

- An interface shall be created for all services, facades and data access objects when there are publicly accessible methods to be created. The exception is only when a service is inheriting an existing implementation and not define new publicly accessible methods
- When creating an interface, the name: 

	- Should convey intent and must not contain the any special prefixes or suffixes to indicate that it's an interface
	- Should be in **Pascal** case
	- Should be alphanumeric 
	
			Examples: AddressService, AddressDao, AddressFacade
			Incorrect Examples: IAddressService, AddressServiceInt, AddressServiceIf. These can be perfectly legal names elsewhere. 

#### Implementations
After defining an interface, implementations should be created to implement the contract defined by the interface. 
Impentations can also be created without directly implementing an interface but extending or inheriting from an existing implementation.
When creating an implementation the following must be followed:

- The name must convey intent
-  Should use **Pascal** case
- The first implementation of an interface must be the interface name prefixed with **Default** e.g ***DefaultAddressService*** for an interface named ***AddressService***
- If inheriting from or overwriting a standard implementation, the prefix **Custom** must be appended to the name of the standard implementation e.g ***CustomAddressService*** inheriting from ***DefaultAddressService***


#### Data Access  Objects

For any item or group of items that is created, it advisable to create data access objects that contain the different types of queries that can be used to retrieve the objects from the database. 

#### Generic Dao

Each data access object must inherit from **de.hybris.platform.servicelayer.internal.dao.DefaultGenericDao** 

	Example: public class DefaultAddressDao extends DefaultGenericDao<AddressModel>

The benefits of doing this are:
- Reduction of code size as well as minimizing bugs
- Ability to generate dynamic queries using the find methods
- Ability to write data access objects without explicitly using flexible search code


#### Naming Conventions 
- The name must demonstrate intent and must contain the suffix ***Dao*** 

		Example: DefaultAddressDao
		

### 
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTgyNzAzNTE2OSw5MTMwODU1NDksMTYzMj
I5NjYyMiwtNDc4MzQ0MTQ5LC0yMDI5NzQ3NjUwLC02MzAxNTM3
ODAsMTYyMDE3MDA5MCw5NDg2OTE5NDAsMTc3NTQ3NTQxLC04MT
g3NDM3NjddfQ==
-->