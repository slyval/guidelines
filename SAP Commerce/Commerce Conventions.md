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

#### Localization
Localization allows for models and model attributes to be internationalized by providing names and descriptions. These names are seen in the backoffice when a user is viewing data. This helps as it provides meaningful context that is not provided when technical names are used. 

To localize attributes

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

#### Converters and Populators
A converter is an object that is used to convert data from one format to another. It populates all or a subset of attributes of a target object from a source object.

Sometimes converters and populators are misunderstood to mean the same thing. However, a populator is a coordinating or container object for converters. When converting from one form to another a populator calls upon a collection of converters which each populate a subset of the attributes of  a target object from a source object. 

All converters should implement the **import de.hybris.platform.servicelayer.dto.converter.Converter** interface 

All populators should implement the **de.hybris.platform.converters.Populator** interface

The common conventions when doing object are as follows:
- Converter must be created and then injected into a populator which should populate by calling all injected converters. The populator can then be injected in the object that require the data conversion 
- It might be necessary in some circumstances to use the converter directly where only one converter is envisioned

As converters and populators are classes, all class conventions apply. Additionally all converters must be suffixed with **Converter** and all populators must be suffixed with **Populator**. 

### Dependency Management
All controllers, facades, services, converters, populators and data access objects are declared/defined as beans. By default, all beans run as singletons unless specified. Beans can depend on other beans as long as the depended upon bean is declared in the same extension or an extension that is depended upon. 

Dependencies between extensions are declared in the **extensioninfo.xml** of an extension. 

All bean declarations are either **xml** based definitions done in the ***-spring.xml**  or in the ***-web-spring.xml** files or as **annoation** based done in the class definition. 

Any definition mechanism can be adopted depending on needs. It is advisable to adopt a single mechanism for uniformint. Controller beans are defined almost exclusively as annotation based beans except when overriding. This needs to be understood as the sequencing of the definitions can impact expected behavior. 

It is advisable but not mandatory to use aliases. This improves flexibility in overriding beans  as it allows overriding without complete replacement. The overriden beans can still be used, if needed, by using the id rather than the alias.

When defining a bean, the following naming conventions should be followed:
- **Camel** case must be used to for bean ids, names and aliases as well as properties

When overriding a bean, there are two approaches to use:
 - An overriding bean can be defined as a completely independent bean and use the same id or the same alias as the bean being overriden. In this case, only the generic conventions apply
 - An overriding bean can be defined inheriting from the bean that it overrides. In this case, the parent attribute should be defined and all properties defined in the parent bean should not be redefined in the overriding bean unless those properties are being adapted

It should be understood that there is a difference between a web-context specific bean and a global-context specific bean

When defining bean properties or inheritance(parenting), be careful that circular dependencies are not introduced

Dependencies are injected in 2 main ways:
- **Property/Constructor** based injection - this is the preferred way when xml-based definitions are commonly adopted 
- **Annotation** based injection- autowiring can be achieved by using the the @Autowired or @Resource annotations. This should be used mainly when using annotation based bean definitions

### Essential and Sample Data Management
Almost all non-trivial solutions have some core data that all functionality depends on. This is data that is usually known at the onset of a project. Examples of this data are user roles, permissions, product categories, product catalogs and titles among many other examples.

Sample data is also important when creating a test environment

To manage this data, there are a couple of places where this data can be placed so that whenever the system is updated or initialialized, this data is imported via IMPEX. The following options should be used:

- If the data is created only once and rarely changes, consider adding it in the **essentialdata-<name>.impex** or **projectdata-<name>.impex** file under the **resources/impex** of the relevant extension. Alternatively, the **resources/<extension_name>/import/common/** folder structure of data extensions such as the  ***initialdata** extensions
- If data is part of a content or product catalog, it should be added under the relevant files in the **resources/<extension_name>/import/coredata** folder structure 
- If data is considered as test data, then it should be created under the **resources/<extension_name>/import/coredata** folder structure if the ***initialdata** data extensions






<!--stackedit_data:
eyJoaXN0b3J5IjpbOTEzMzIxMDk2LC0xNjUzOTY4MjI5LC0xMj
YxMjM1NzcxLDk4OTc1NTcwMCwxNTE0Njk0NjEwLC0xNTc5Mzg1
MjczLDM4NDY5NTUxOSwxNjI5NDE3MTc4LC0xNzYxNjI2MjQ3LD
M0ODE2NTI0LC0zMzI5NDAyLDE1MzcyNzA5MTAsLTE4MzMzNTI2
NzAsLTE0ODg5MTY4OCwtNjEyODQ0OTM1LDE2NzY3NjkzOCwtMT
g2Njk5MjU3LC05NzcwNDYyNjYsLTExNjQ1ODYzNTksLTMzNDM0
NTczNl19
-->