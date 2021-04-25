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

Basically, the application has four main jobs:
1. dependency-check: OWASP Dependency-Check identifies project dependencies on open-source code and checks if there are known vulnerabilities associated with that code.<br/>
2. test: Unit tests and Integration tests executed and results send to artifacts.<br/>
    2.1. Test Coverage: Code coverage calculated with Jacoco.<br/>
    2.2. Integration Tests:<br/>
  
3. sonarcloud: Code static analysis performed<br/>
    3.1. Test Coverage results published to the sonarCloud<br/>
    3.2. Integration test result published to the sonarCloud. (SonarCloud Kotlin Integratin Test [Bug](https://jira.sonarsource.com/browse/SONARSLANG-353) reported via Jira, After reported bug fixed, task expected to import results successfully.)<br/>
    
4. build: gradle task build<br/>

## dependency-check job

dependency-check task generate OWASP dependency check report under the path:`./build/reports` in **ALL** format
* `./gradlew --stacktrace dependencyCheckAnalyze` command created the report
* Then created reports uploaded to the artifact. *github.com/user/repo/artifacts/latest*

```
  dependency-check:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
      # Checks-out your repository under $GITHUB_WORKSPACE, so your job can access it
      - uses: actions/checkout@v2

      # Runs a single command using the runners shell
      - name: OWASP checker
        run: ./gradlew --stacktrace dependencyCheckAnalyze
    
      - name: OWASP Report
        uses: actions/upload-artifact@v2
        with:
          name: dependency-check-reports
          path: ./build/reports/
```

## test

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
