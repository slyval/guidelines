# SAP Commerce Guidelines

## Purpose
The purpose of this document is to provide guidance in relation to the implementation of SAP Commerce Cloud. The benefit of these guidlines will be to provide a reference framework that allows for uniformity, ease of maintenance and adherence to best practices. 

This document will be updated whenever there are new guidelines. It is best to always refer to this during implementation of SAP Commerce Cloud projects.  SAP Commerce Cloud refers to both the on-premise and 

## Solution
	
##### Version
Unless there is a customer specification, the latest version of commerce released should always be utilized

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

- An addon must only be created for the purposes of overwritting or defining web based functionality. This includes the definition or extension of styling, javascript scripting, and  page, view or tag definition. An extension for webservices or REST APIs is ***not*** an addon

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

### Code Artefacts

#### Packages
Packages are containers and must group functionality accordingly. 

All package names must:
- Be prefixed with the below. Please check which of the above three has been adopted for the project: 
		
		com.<customer>.<project>.<extension>. or 
		com.<customer>.<extension> or 
		com.<project>.<extension>

- The name after the prefix must convey the intent of the package. As an example, a package utilities must contain the word utilities or utils in it's name
		
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

 When only one item type or model is to be retrieved,  the dao must inherit from **de.hybris.platform.servicelayer.internal.dao.DefaultGenericDao** , passing the concrete model as the parameter

	Example: public class DefaultAddressDao extends DefaultGenericDao<AddressModel>

The benefits of doing this are:
- Minimization of bugs as you can reuse tested methods such as find
- Ability to generate dynamic queries using the find methods
- Ability to write data access objects without explicitly using flexible search code

The name must:
- demonstrate intent
- contain the suffix ***Dao*** 
- user **Pascal** case

`Example: DefaultAddressDao`
		
#### Data Transfer Object (DTOs)
Data transfer objects are serializable objects used to pass specific data between different objects whether locally, such as  between a controller and a facade,  or remote, such as between a controller and a remote caller. 

DTOs contain a subset data from an item or a combination of items. 

The following conventions apply:
- They must always be defined in the spring-bean.xml and not manually created. This makes maintenance easier.
- They must be added into a relevantly named package as suggested under the **Packages section**
- They name of must
	- contain the suffix **Data** or **Dto**
	- contain only alphanumeric characters
	- be in **Pascal** case
- The attributes of a DTO must be in **Camel** case

		Example: 

#### Services 
A service is an object that is concerned with the execution of business logic or business rules. It, architecturally, sits just above the DAOs. It can utilize other services and DAOs to achieve it's business goals. 

The following conventions should apply:
- The service must be in package that is relevantly named as suggested under the **Packages section**
- They must be declared as beans either using annotations or xml configuration 
- The name must:
	- contain the suffix **Service**
	- contain only alphanumeric characters
	- be in **Pascal** case
- All attributes of a service must be in **Camel** case

		Example: 	Interface: SaleAreaDeterminationService
					Implementation: DefaultSalesAreaDeterminationService

#### Facades
As the name implies, this proxy represents a particular usage. As an example, you can have a services that retrieve different pieces of information. You can use these services to compose employee and customer information. You would then create 2 facades, one represention employee composition and the other representing customer composition. This is a better approach that having a single facade with 2 methods catering for the different scenarios. It stops being a facade and becomes a bloated service. 

Architecturally, the facades resides on the layer above the services layer. 

The following conventions should apply when creating a facade: 
- Packages into which the facade must be relevantly named
- Always create facades as beans that are declared using annotations or xml configuration 
- The name must:
	- contain the suffix **Facade**
	- contain only alphanumeric characters
	- be in **Pascal** case
- All attributes of a service must be in **Camel** case

			Example: 	Interface: CustomerDataCompilationFacade
						Implementation: DefaultCustomerDataCompilationFacade


#### Controllers
Controller a web request handlers. As such they are commonly used with RESTful web services or web applications. They should always be in thev web part of an extension. They sit above facades, architecturally speaking but there is nothing wrong with a controller calling a service unless there is a facade in place that the controller should utilize

The following should be adhered to when creating a controller: 
- The package naming should be representative
- The name of the controller must:
	- contain the suffix **Controller** e.g AccountPageController, PasswordResetPageController, ContractManagementController
	- contain only aplhanumeric characters 
	- be in **Pascal** case
- All attributes of a controller must be in **Camel** case, except for constants

#### Bean Definition
The bean definition for services, facades and data access objects is supposed to be done uniformly accross a probject and the decision to choose between these two mechanisms should be made for each project: 
- xml driven definition 
- annotation driven definition

It is advisable but not mandatory to use aliases. This makes it easier to override functionality without completely replacing some of the beans, especially where the overriden beans can still be used. 

When overriding a bean the following is mandatory: 
- the overriding bean must define a parent and use the overriden bean or the parent of the overriden bean. 
- parameters defined in the overriden bean must not be redeclared except in cases where new values are to be defined for the properties
- **Camel** case must be used to for bean ids, names and aliases. This naturally applies for the properties owing to the conventions for services, facades, daos and controllers

The rules for the definition of web specific and global beans must be understood so as not to define beans in the wrong place. Web specific beans must be defined in the <extensionname>-web-spring.xml file in the resources/\<extension-name>/web/spring folder. Global bean definitions are performed in the \<extension-name>-spring.xml file in the resources folder. 

Standalone bean example:

	<bean id="sendEmail" class="de.hybris.platform.acceleratorservices.process.email.actions.SendEmailAction"
          parent="abstractAction">
        <property name="emailService" ref="emailService"/>
    </bean>
 Overriding bean example
 
	<bean id="generateCustomerRegistrationEmail" parent="abstractGenerateEmailAction">
        <property name="frontendTemplateName" value="ShrCustomerEmailVerificationEmailTemplate"/>
    </bean>
  




<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEzNDYwNTc3MTcsMjA0MDI5NzYyMl19
-->