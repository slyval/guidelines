# SAP Commerce Guidelines

## Purpose
The purpose of this document is to provide guidance in relation to the implementation of SAP Commerce Cloud. The benefit of these guidlines will be to provide a reference framework that allows for uniformity, ease of maintenance and adherence to best practices. 

This document will be updated whenever there are new guidelines. It is best to always refer to this during implementation of SAP Commerce Cloud projects.  This document applies to both on-premise and cloud implementations. 

## Solution
	

#### Initial Technical Considerations
When starting a commerce project, the following should considerations must be made:

- Development and Release Management
	There are 3 aspects to consider in this regard and they are:
     - Source Code Management 
     This relates to the exact source code management tool such as git or subversion among other examples. Unless the customer has a tool that exists already and is compatible with expectations, git based tools must be preferred. The first preference should be **GitHub**. 
     - Release Management and Versioning 
     There is a guide for release management for all projects. You should use that as the reference for implementation. To summarize, the branhing strategy resembles GitFlow and two permanent branches - **master** & **develop** must be used. These are supported by temporary ***feature***, ***release***, ***bugfix***(not always necessary) and ***hotfix*** branches. 
     - Version System 
     A versioning system must be derived and that should consist of a **major** version, a **minor** version as well as a **patch** version. This can be prefixed with a name that relates to the project. 
			     
			     An example is Testv0.1.0

- Base Package
A base package that will be used accross all packages should be determined. The customer can have a preference based on previous implementations. If that is not the case, the base package name should be formed as follows:
	> com.\<customername>.\<projectname> 
	An example would be: com.testcustomer.testproject

	The creation of a base package implies that an package in any extension that is part of the project must be prefixed with the base package name
	
- Integrated Development Environment
	Without being prescriptive, any IDE should be used as long as it supports usage of SonarLint and other tools we might use. Having said that, the IDEs of choice are **Spring Tool Suite, Eclipse and IntellijIDEA**
	SonarLint must be installed and (if feasible), connected to the central SonarQube server for rules
- Static Code Analysis
**SonarQube** is the defacto standard for static code analyses for SAP Commerce. A central server and project should be set up for any commecerce project. This allows for adherence to good coding practices and will help preempt technical checks performed by SAP

- Continuous Integration and Deployment
The defacto standard CI tool for our projects is **Jenkins**. For all projects, with the exception of cloud implementations - at the moment, all project must have a CI environment setup to manage builds and deployments. A guide for this will also be prepared to provide directed guidance. 

- Local Development Environment 
	- Database - Where relevant, the database system that is used in the customer's runtime should be used to avoid surprises. When it's not possible, it is advisable to use an external database that you can access even when commerce is not running. This helps make troubleshooting better.
	- Application Server - Customers might wish to use alternative application servers and where that is the case, the same should be used in local environments to help make troubleshooting easier as, in some cases, debugging may not be allowed in the development runtime system.

#### Solution Initialization
A decision on whether to bootstrap the solution from and accelerator or to create a different structure should be made based on the requirements. An accelerator is used when there is one provided by SAP and the solution to be implemented is aligned to the accelerator. An example is a B2B Accelerator provided for B2B solutions. 

For any of the accelerators,  a module is provided that will help bootstrap the project. Therefore, the  ***modulegen*** ant task must be used together with the corresponding module. The project name and the base package are critical as they are used in this case to generate representative extensions and packages. 
		

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
eyJoaXN0b3J5IjpbMjA5OTc4Nzk2NywtODQ4MjEyNTI2LC04MD
k0ODUwMTIsMTY1MjE2ODkyNCwtMTExNzY3NDY2NCwyMDQwMjk3
NjIyXX0=
-->