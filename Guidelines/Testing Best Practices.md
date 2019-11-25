# Automated Testing Guidelines and Best Practices

## Purpose

The purpose of this document is to provide guidance in relation to the automated testing.  The benefit of these guidelines will be to provide a reference framework that allows for uniformity, ease of maintenance and adherence to best practices.

## Introduction

Maintainable and readable test code is crucial to establish a good test coverage which in turn enables implementing new features and performing refactorings without the fear of breaking something.

This document contains many best practices collected pertaining writing unit tests and integration tests in Java. We will start with a unit testing followed by integration testing. The testing framework of choice/referred therein  is JUnit.

## Unit testing
A unit test is simply a test that covers the smallest piece of our code base, a ‘unit’ if you will, in isolation from other parts of the system. In Java, this would normally refer to testing individual methods.

### A good unit test should be:

**Fast**. Having a unit test suite that takes ages to run removes most of the advantages. The longer it takes to run, the more developers will put off running them until just before commit time, or start skipping them all together.

**Isolated**. It can be tempting to have a bunch of common set-up tasks used by multiple unit tests, but where possible, try to limit this. It ties unit tests together in ways that can be harder to isolate in the future.

**Succinct**. If your unit tests are hard to set up and complicated to write, it’s a good sign that the code you are trying to test is either too complex, or badly structured. Either way, it’s a good sign that a refactor might be required.

**Easy to Understand**. Good naming of unit tests is vital. You should be able to read a test name and understand what the test is testing, what results the test is expecting, and why it’s failing. Do not be afraid to make unit test names long – the more information the better. After all, you should only see them when they are failing.

**Reliable**. Tests shouldn’t be bound to a given environment or rely on external factors to work. They should work 100% of the time, or they are not going to fulfill their purpose.

### General

#### Given, When, Then

 - **Given (Input)**: Test preparation like creating data or configure mocks.
 - **When (Action)**: Call the method or action that you like to test.
 - **Then (Output)**: Execute assertions to verify the correct output or
   behavior of the action.

#### Use the Prefixes “actual*” and “expected*“

Regarding variables in an equals assertion, prefix the variables with “actual” and “expected”. This increases the readability and clarifies the intention of the variable.

    // Do
    ProductDTO actualProduct = requestProduct(1);
    
    ProductDTO expectedProduct = **new** ProductDTO("1", List.of(State.ACTIVE, State.REJECTED))
    
    assertThat(actualProduct).isEqualTo(expectedProduct); // nice and clear.

#### Use Fixed Data Instead of Randomized Data

Avoid randomized data as it can lead to toggling tests which can be hard to debug and omit error messages that make tracing the error back to the code harder. Instead, use fixed values for everything.

    //Don’t
    UUID uuid = UUID.randomUUID();
    
    //Do
    UUID uuid = UUID.fromString("00000000-000-0000-0000-000000000001");

### Unit Testing best practises

 - **Tests must be small and specific** – heavily use helper functions,
   parameterized test  and avoiding one test for all corner cases.
 - **Self-contained** - by revealing all relevant parameters and prefer
   composition over inheritance. 
 - **Write dump tests** - avoiding the reuse of production code and focusing
   on comparing output values with hard-coded values.
 - **Test close to production** - focusing on testing a complete vertical   
   slide and avoiding in-memory databases.  
 - Invest in a testable implementation by avoiding static access, using 
   constructor injection, using Clocks and separating business logic   
   from asynchronous execution.
   
   In the following sections these will be explained more in detail.
   
#### Write Small and Specific Tests

##### Heavily Use Helper Functions

Extract details or repetitive code into methods and give them a descriptive name. This is a powerful way to keep the tests short and the essentials of the test easy to grasp at first glance.

##### Don’t Extend Existing Tests To “Just Test One More Tiny Thing”

Don’t add a corner case test to an existing (happy path) test as this test becomes bigger and harder to understand. Its hard to see what is broken in the case that the test fails.

    // Don't
    public  class UserControllerTest {
    @Test
    public  void testHappyScenario() {
    
         // a lot of code comes here...
       }
    }

Instead, create a new test method with a descriptive name that tells everything about the expected behavior.

    // Do
    public class UserControllerTest {
        @Test
        public void testMultipleUsersAreReturned() {}
        @Test
        public void testAllUsersAreReturned() {}
        @Test
        public void testFilterByRole() {}
        @Test
        public void testFilterByDateCreated() {}
}

Again, helper functions can reduce the typing effort

##### Don’t Overuse Variables

    // Don't
    @Test
    public void testVariables() throws Exception {
        String relevantCategory = "Office";
        String id1 = "4243";
        String id2 = "1123";
        String id3 = "9213";
        String irrelevantCategory = "Hardware";
        insertIntoDatabase(
                createProductWithCategory(id1, relevantCategory),
                createProductWithCategory(id2, relevantCategory),
                createProductWithCategory(id3, irrelevantCategory)
        );
    
        String responseJson = requestProductsByCategory(relevantCategory);
    
        assertThat(toDTOs(responseJson))
                .extracting(ProductDTO::getId)
                .containsOnly(id1, id2);
    }

Unfortunately, this significantly bloats the test code and hard to trace on error.

    // Do
    @Test
    public void testVariables() throws Exception {
        insertIntoDatabase(
                createProductWithCategory("4243", "Office"),
                createProductWithCategory("1123", "Office"),
                createProductWithCategory("9213", "Hardware")
        );
    
        String responseJson = requestProductsByCategory("Office");
    
        assertThat(toDTOs(responseJson))
                .extracting(ProductDTO::getId)
                .containsOnly("4243", "1123");
    }

#### Self-Contained Tests

##### Don’t Hide the Relevant Parameters (in Helper Functions)

Rule of thumb: You should see the essentials of a test by looking only at the test method.

Use helper functions for creating data and assertions, but you have to parameterize them. Define a parameter for everything that is important for the test and needs to be controlled by the test. Don’t force the reader to jump to a function definition in order to understand the test.

    // Don't
    insertIntoDatabase(createProduct());
    List<ProductDTO> actualProducts = requestProductsByCategory();
    assertThat(actualProducts).containsOnly(new ProductDTO("1", "Office"));
    
Note the difference.
    
    // Do
    insertIntoDatabase(createProduct("1", "Office"));
    List<ProductDTO> actualProducts = requestProductsByCategory("Office");
    assertThat(actualProducts).containsOnly(new ProductDTO("1", "Office"));

##### Insert Test Data Right In The Test Method

Everything needs to be right in the test method. It’s tempting to move reusable data insertion code to the `@Before` method, but this would force the reader to jump around in order to fully understand what’s going on. Again, helper functions for data insertion can help to make this repetitive task a one-liner.

##### Favor Composition Over Inheritance

Hierarchies are hard to understand and you likely end up extending a base test that contains a lot of stuff that the current test doesn’t need. This distracts the reader and can lead to bugs.

Instead, we recommend using composition. Write small code snippets and classes for each specific fixture work (start the test database, create the schema, insert data, start a mock web server). Reuse those parts in your tests in the `@BeforeAll`. The test class is self-contained because everything relevant is right in the test class.


#### Compare the Output with Hard-Coded Values

##### Don’t Reuse Production Code

Test should test the production code; not reuse it. If you reuse production code in a test, you might miss a bug that is introduced in the reused code because you don’t test this code anymore.

    // Don't
    boolean isActive = true;
    boolean isRejected = true;
    insertIntoDatabase(new Product(1, isActive, isRejected));
    
    ProductDTO actualDTO = requestProduct(1);
    
    // production code reuse ahead
    List<State> expectedStates = ProductionCode.mapBooleansToEnumList(isActive, isRejected);
    assertThat(actualDTO.states).isEqualTo(expectedStates);

Instead, think in terms of input and output when writing tests. The test sets the input and compares the actual output with hard-coded values. Most of the time, code reuse is not required.

    // Do
    assertThat(actualDTO.states).isEqualTo(List.of(States.ACTIVE, States.REJECTED));

##### Don’t Rewrite Production Logic

Rewriting logic defeats the whole purpose of testing as it most likely end up rewriting the production logic in the test code, which can contains bugs.

##### Don’t Write Too Much Logic

Again, testing is mostly about input and output: Providing input and compare the actual output with the expected values. Hence, we don’t need to code much logic in our tests and we shouldn’t.


## Integration Testing Best Practices
Integration testing is used to verify the performance of the complete system of software. Basically, units of software are added together to create modules.
Used to detect errors resulting from units interacting with one another.

### Do Integration Testing Before or After Unit Testing

Perform Integration tests after a successful execution of unit tests. However, in an agile setup this might not be the case.

### Separate Unit Testing Suites From Integration Testing Suites

Keeping test suites separate allows developers to run fast unit tests and saves the long integration testing process for the build server in another test suite.

### Log as Much as Possible

The scope and complexity of integration tests - usually spanning several modules and hardware components - identifying the cause of an integration failure is much more difficult. Because by definition an integration tests is based on many components and a specific data flow, identifying the failure cause is not always straightforward.

Still, be mindful that exhaustive logging can have a significant effect on performance, so it should be done only when needed.

### Don't mock results
Mocking must not be used in integration test,  otherwise this defeats the whole purpose of testing the system behavior.

## Naming conventions

 - Unit test classes are named with “name of class + **_Test_**” e.g.`UserTest.java`.
 - Integration tests however have a different naming convention. They
   are named as “name of class + ***IntegrationTest***.
e.g. `UserControllerIntegrationTest.java`.  
 - Prefix all test methods with "***test***" e.g. `testAllUsersAreReturned()`

This kind of naming allows an easy identification of unit and integration tests.


## Use a Good Mixture of Unit and Integration Tests

Ensure there is a good mixture of unit and integration tests given they compliment each other with the later testing the behavior. On the same note, there must be more unit tests than integration given that the former are fast and the later slow.

 - Traditional mock-based unit tests are testing classes in isolation.
   Drawbacks:
	 - We don’t test if the classes are working together correctly.
	 - Painful Refactorings.
 - Instead, focus more on integration tests in which we wire the real
   objects together and write a single test that hits all layers.
   Advantages:
	 - We test behavior instead of an implementation.
	 - We test much closer to reality because in production the application
   will also use the real objects.
	 - Higher test coverage as we test both the parts and everything as a
   whole.
	 - Refactorings in the internal structure are less likely to break your
   tests.
 - Test against the [real database (via testcontainers) instead of an
   in-memory-database to be even closer to production.

## Adopt a "test as your code" approach.

The more code you write without testing, the more paths you have to check for errors. Thus, it is vital that testing is not treated an an afterthought.

## General guidelines/tips

 - Review tests over time. Like code, unit tests rot as a code base
   evolves. Make sure to regularly review unit tests to ensure they are
   still fit for purpose.
 -   Use mocking tools where appropriate(only for unit tests). Tools such as Mockito and jMock are powerful tools that can make hard-to-test code easier to write tests for. However, it shouldn’t be a crutch for poor design. If your code base requires a lot of mocking to test, it might be time for a refactor.
 -   When fixing bugs, make sure the test suite is updated to cover the new failing conditions, so you can ensure it does not recur.
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTM3NjI5NjA3Ml19
-->