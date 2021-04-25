# ![GitHub Actions](https://miro.medium.com/max/750/0*InaeVdy44dc0JczI.jpg)

[![CI/CD](https://github.com/ozlemgulp/create-pipeline/actions/workflows/blank.yml/badge.svg)](https://github.com/ozlemgulp/create-pipeline/actions/workflows/blank.yml)

This repo cloned from [kotlin-http4k-realworld-example-app](https://github.com/alisabzevari/kotlin-http4k-realworld-example-app) to purpose of create a pipeline and learn about GitHub actions.

# Overview
* Project build with [Gradle](https://gradle.org/)
* Code with [Kotlin](https://kotlinlang.org/)
* Test with [Kotest](https://github.com/kotest/kotest/)
* Code static analysis performed with [SonarCloud](https://sonarcloud.io/dashboard?id=ozlemgulp_create-pipeline)
* Dependency checks performed with [OWASP Dependency-Check](https://owasp.org/www-project-dependency-check/)
* Code coverage performed with [Jacoco](https://www.jacoco.org/jacoco/trunk/doc/)

## Pipeline Structure

Basically, the application has four main parts:
1. dependency-check: OWASP Dependency-Check identifies project dependencies on open-source code and checks if there are known vulnerabilities associated with that code.<br/>
2. test: Unit tests and Integration tests executed and results send to artifacts.
    1.Test Coverage: Code coverage calculated with Jacoco.
    2.Integration Tests:
  
3. sonarcloud: Code static analysis performed
    1.Test Coverage results published to the sonarCloud
    2.Integration test result published to the sonarCloud. (SonarCloud Kotlin Integratin Test [Bug](https://jira.sonarsource.com/browse/SONARSLANG-353) reported via Jira, After reported bug fixed, task expected to import results successfully.)
    
4. build: gradle task build

## Application structure

Basically, the application has four main parts:
* `main` function which instantiates all the services and handlers and connects them together, then starts the server.
* `Router` class which handles the translation of 1. http requests to request handler calls and 2. handler results to http responses. Authorization logic is also implemented in this class.
* `handler` package which contains a class for every request handler. Handlers are classes with one method (`invoke`) which handles the request. This package contains the whole business logic of the application. Handlers have access to data layer.
* `ConduitRepository` class which is responsible for building database queries. In order to communicate to database, a class called `ConduitTransactionManagerImpl` is needed. this class has one method (`tx`) which connects to database and opens a transaction to communicate with database. `tx` accepts an anonymous function while provides an instance of `ConduitRepository` as a receiver.

```
+ config/
    Application config as simple data classes
+ handler/
    All handler classes which describe business logic of the application
+ model/
    domain model and dtos
+ repository/
    table definitions, typesafe database creation scripts and classes to communicate with database
+ util/
    utility classes such as request filters, serialization/deserialization functions and jwt utility functions
+ Main.kt
    File containing main function
+ Router.kt
    File containing Router class responsible for handling http server communication
```

## Database

The application currently uses H2 embedded database. The connection is defined in `config/local.kt`. If you want to change the database you need to provide the correct dependency for the driver and change the configuration. 

## Tests

You can run `./gradlew test` to run all the tests. A test logger gradle plugin (`com.adarshr.test-logger`) has used to render the test beautifully in console.
There are a couple of unit tests for handlers but not for all of them (contributions welcome!).
There are integration tests to cover all the cases of the postman test file.

# Getting started

You need Java 11 installed.

**Build and run tests:**
```
./gradlew clean build
```

**Start the server:**
```
./gradlew run
```
The server will be available on `http://localhost:9000`
