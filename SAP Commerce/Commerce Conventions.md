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
		

## Implementation Guidelines

### General Architecture

### Extensions
Extensions are containers for common logic that achieves a particular purpose. They must not be created for every single piece of functionality but must be created to logically group functionality.  

Besides functionality the general purpose grouping should consist of the following for the core areas of a project:

- Services extensions (in an accelerator these might have the suffix **core** )
- Facades extensions 
- Data Access Objects extensions

If the separation between above is too granular for certain functions, then packages can be used to separate the facades, services and daos. 

#### Naming  Conventions
- All extension names must: 
	- Only contain alphanumeric characters
	- Only contain lowercase characters
	> Example: customordermanagementservices
	
- An addon must have the suffix ***addon***
	> Example: customb2baccelratoraddon
	
- When overriding a standard extension, the prefix ***custom***  or **\<project name>** must precede the name of the standard extension. 
	> Example: When overriding the extension sapintegrationservices, the name can be customintegrationservices or testprojectintegrationservices

- All extension names must convey intent. Therefore, 
	- An extension for services must contain the suffix ***services***
	- An extension for facades must contain the suffix ***facades***
	- An extension for data access objects must contain the suffix ***daos*** 
	
	> Examples: sapordermanagementservices, commercefacades

#### Generation 
- When generating an extension, a template extension is required. The correct template must be used and the following guidelines will help:
	- Addons 
	Addons are only created when intending to define or overwrite web content in the form of content pages, views, tags, javascript, css among others. 
		
		The **yaddon** template must be used
	- Web Service Extension 
		Web service extensions must use the **ywebservices** (1st preference ) or **ycommercewebservices** extensions as templates
	- Other Extensions
		All other extensions must use a corresponding template if there is one, e.g., ***ybackoffice*** for backoffice extensions. Otherwise, all other extensions must use the **yempty** template

- A base package for an extension is also required when  generating an extension. The name of the extension must be structured as follows:

	**<project_base_package_name>.<extension_name>**
	
	 Refer to the **Packaging** section for guidelines.

### Packaging
All package names must:
- show intented use as a container
- be in lowercase 

### Data Model
All data models or items must be created in the *-items.xml file in the resources folder of an extension. 
#### Items, Relations and Enums
- For items, relations and Enums; the names must:
	- Contain only alphanumeric characters
	- Must use **Pascal** case
		 
- Except where necessary, the name of the deployment table must be the same as the name of the item or relation
- For item and relation attributes, **Camel** case must be utilised
	
		Example: code, deliveryDate

- For Enums, the names of the value codes must be in **Upper** case
		
		Example: SUNDAY 

- Except when aboslutely necessary, do not specify the jaloclass attribute for an item type

#### Item Attributes

- Where an attribute can contain a static list of values, an Enum must be created and used to type the attribute

- Where multiple values are possible for an attribute, a relation to an Enum or another item must be created
-  Avoid creating circular references where an attribute is typed to the item it belongs to
- Collection types must not be used for any attribute. A relation of one-to-many cardinality must be created instead.

### Classes and Interfaces
		
#### Interfaces 
An interface defines a contract with the outside world. It defines all the publicly accessible methods that external dependents can interact with. 

- An interface should be created for all services, facades and data access objects. The exception is only when a service is inheriting an existing implementation and not defining new publicly accessible methods
- For naming conventions, interfaces should : 
	- convey intent and must not contain the any special prefixes or suffixes to indicate that it's an interface
	- be in **Pascal** case
	- contain only alphanumeric characters
		
			Examples: AddressService, AddressDao, AddressFacade
- The method names should:
	- be in **Camel** case
	- only contain alphanumeric characters
	
#### Implementations
Inmplementations are classes that define behavior for a given contract/interface. 
Classes can directly or indirectly - extending an existing implementation - implement  an interface. 

- The naming conventions for interfaces also apply for implementations. 
- The first implementation of an interface must be the interface name prefixed with **Default** e.g ***DefaultAddressService*** for an interface named ***AddressService***
- If inheriting from or overriding a standard implementation, the prefix **Custom** must be appended to the name of the standard implementation e.g ***CustomAddressService*** inheriting from ***DefaultAddressService***
- The prefixing with Default or Custom can be adapted to use the customer or project name if so desired but only if this is adopted for global use

### Common Object Types
The following are the common object types that form the three common architectural layers namely; Facades, Services and Data Access layers. Following are the guidelines for these objects.

#### Data Access  Objects

For any item or group of items that is created, it advisable to create data access objects that contain the different types of queries that can be used to retrieve the objects from the database. 

 When only one item type or model is to be retrieved,  the dao must inherit from **de.hybris.platform.servicelayer.internal.dao.DefaultGenericDao** , passing the concrete model as the parameter

	Example: public class DefaultAddressDao extends DefaultGenericDao<AddressModel>

The benefits of doing this are:
- Minimization of bugs as you can reuse tested methods such as find
- Ability to generate dynamic queries using the find methods
- Ability to write data access objects without explicitly using flexible search code

As DAOs are interfaces and classes, the conventions for the same apply. Additionally, the name of a DAO interface or implementing class must contain the suffix ***Dao*** 

`Example: DefaultAddressDao`
		
#### Data Transfer Object (DTOs)
Data transfer objects are serializable objects used to exchange data between different objects whether locally, such as  between a controller and a facade,  or remotely, such as between a controller and a remote caller. 

Data transfer object are also commonly referred to as beans or pojos. They contain a subset of attributes from one or more models or items. 

It is imperative that as a convention, all DTOs must be declared in the *-beans.xml file of the relevant extension. It's common to find these defined as part of the service layer. 

As DTOs are classes, the conventions for the same apply. Additionally, the name of a DTO must contain the suffix **Data** or **Dto**
	
#### Services 
A service is an object that is concerned with the execution of business logic or business rules. It can utilize other services and DAOs to achieve it's business goals. 

 As services are interfaces and implementing classes, the conventions for the same apply. Additionally, the names of service interfaces and classes must contain the suffix **Service**

		Example: 	Interface: SaleAreaDeterminationService
					Implementation: DefaultSalesAreaDeterminationService

#### Facades
A facade is a lean and focused proxy service catering for a specific use case. Whereas a service can cater for multiple use cases as a generalized implementation, a facade makes use of these services and utilizes on the required functionality and flow. Data transfer objects find a greater use here that in services as focus transfer objects are utilized. 
 
As facades are interfaces and implementing classes, the conventions for the same apply. Additionally, the names of facade interfaces and classes must contain the suffix **Facade**


			Example: 	Interface: CustomerDataCompilationFacade
						Implementation: DefaultCustomerDataCompilationFacade

#### Controllers
Controller are essentially web request handlers. As such they are commonly used with RESTful web services or web applications. They are created in the web part of an extension. 

As controllers are classes and - sometimes - interfaces, the conventions for the same apply. Additionally, the names of controller classes must contain the suffix **Controller** e.g AccountPageController, PasswordResetPageController, ContractManagementController
	

### Bean Definition
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
eyJoaXN0b3J5IjpbMzQ4MTY1MjQsLTMzMjk0MDIsMTUzNzI3MD
kxMCwtMTgzMzM1MjY3MCwtMTQ4ODkxNjg4LC02MTI4NDQ5MzUs
MTY3Njc2OTM4LC0xODY2OTkyNTcsLTk3NzA0NjI2NiwtMTE2ND
U4NjM1OSwtMzM0MzQ1NzM2LDg2ODMwNDgxOCwtMTgzMzE3OTA4
NCwtMTk5OTQxNzcxMSwxMDY1NjE3MTI2LC04NDgyMTI1MjYsLT
gwOTQ4NTAxMiwxNjUyMTY4OTI0LC0xMTE3Njc0NjY0LDIwNDAy
OTc2MjJdfQ==
-->