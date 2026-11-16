# EcommerceHub BDD Automation Framework

Behavior driven UI automation for an eCommerce application using **Java, Selenium WebDriver, Cucumber, TestNG, and Maven**. The framework connects readable Gherkin scenarios to reusable Java step definitions and page objects, with browser lifecycle hooks and execution reports.

**Application:** [CloudBerry Store](https://cloudberrystore.services/)

## Framework capabilities

- Gherkin feature files describe application behavior using Given, When, and Then steps.
- Page Object Model separates page interactions from scenario definitions.
- Shared step definitions reuse common navigation and login actions.
- Cucumber hooks manage browser setup, cleanup, and failure screenshots.
- A ThreadLocal WebDriver supports separate driver instances for execution threads.
- TestNG supports execution across Chrome, Firefox, and Edge.
- Tags select which scenarios to execute.
- Cucumber HTML, JSON, and Extent Spark reports capture execution results.
- Log4j2 provides application logging.

## Scenario coverage

The supplied suite contains six feature files:

| Feature | Functional area |
| --- | --- |
| `TC01_LaunchApplication.feature` | Application launch |
| `TC02_Login.feature` | Login with valid credentials |
| `TC03_AddToCart.feature` | Adding a product to the cart |
| `TC04_CompletePurchase.feature` | Checkout and purchase completion |
| `TC05_AddToWishList.feature` | Adding a product to the wish list |
| `TC06_AddAffiliate.feature` | Affiliate functionality |

## Project organization

| Location | Purpose |
| --- | --- |
| `testBase.BaseClass` | Shared configuration, WebDriver lifecycle, and screenshot utilities |
| `pageObjects` | Page elements and reusable interactions |
| `src/test/java/stepDefinitions` | Java implementations of Gherkin steps |
| `src/test/java/hooks` | Scenario setup and cleanup |
| `src/test/java/runners/TestRunner.java` | Cucumber features, glue, tags, and report plugins |
| `src/test/resources/features` | Gherkin feature files |
| `src/test/resources/config.properties` | Application URL, execution environment, and test account settings |
| `src/test/resources/extent.properties` | Extent report and screenshot output settings |
| `src/test/resources/spark-config.xml` | Spark report presentation settings |
| `src/test/resources/log4j2.xml` | Logging configuration |
| `testng.xml` | Browser parameters and parallel suite execution |
| `pom.xml` | Dependencies and Maven build configuration |

## Technology stack

Versions reflect the current project POM.

| Technology | Version / configuration |
| --- | --- |
| Java compilation source and target | 17 |
| Selenium WebDriver | 4.23.1 |
| Cucumber Java and Cucumber TestNG | 7.18.1 |
| TestNG | 7.10.2 |
| ExtentReports | 5.1.1 |
| ExtentReports Cucumber 7 adapter | 1.14.0 |
| Log4j2 | 2.23.1 |
| Maven Surefire Plugin | 3.2.5 |

## Local setup

1. Install a JDK that supports the Java 17 compilation target and configure the Java runtime used to launch the tests. The recorded local run used JDK 25.0.5.
2. Install Maven and the browsers required for the selected execution mode.
3. Import the project into Eclipse using **File → Import → Maven → Existing Maven Projects**. Install the TestNG Eclipse plugin to launch the XML suite from the IDE.
4. Update `src/test/resources/config.properties` with the application settings and a dedicated test account:

   ```properties
   execution_env=local
   appURL=https://cloudberrystore.services/
   email=YOUR_TEST_ACCOUNT_EMAIL
   password=YOUR_TEST_ACCOUNT_PASSWORD
   ```

5. Save the configuration and update the Maven project to resolve dependencies.

Use placeholders in any publicly committed configuration. Keep actual account credentials private.

## Running the tests

### Three browser suite in Eclipse

Right click the root `testng.xml` file and select **Run As → TestNG Suite**.

The XML suite defines Chrome, Firefox, and Edge tests, each using `runners.TestRunner`. With `parallel="tests"` and `thread-count="3"`, the three browser runs can execute concurrently.

The runner currently selects `@regression` scenarios. For the supplied six scenarios, the three browser suite produces **18 scenario executions**.

### Single browser through Maven

Run from the project root:

```bash
mvn test -Dos=windows -Dbrowser=chrome
```

Chrome and Windows are the default browser and operating system values. To request another browser:

```bash
mvn test -Dos=windows -Dbrowser=firefox
```

The current POM discovers runner classes using `**/*Runner*.java`; it does not configure the root `testng.xml` as a Maven suite. Therefore, the commands above use the runner with the requested browser rather than launching all three XML browser tests.

### Scenario selection

`TestRunner.java` defines:

```java
features = "src/test/resources/features",
glue = {"stepDefinitions", "hooks"},
tags = "@regression"
```

Change the tag expression when a different scenario selection is needed. Tags applied to a feature also apply to its scenarios.

## Reports and execution evidence

| Output | Location |
| --- | --- |
| Cucumber HTML report | `target/cucumber-report.html` |
| Cucumber JSON results | `target/cucumber.json` |
| Extent Spark report | `target/ExtentReport/ExtentSpark.html` |
| Extent failure screenshots | `target/ExtentReport/screenshots/` |
| Eclipse TestNG output | `test-output/` |

After execution, refresh the project and open an HTML report in a web browser. Failed scenarios trigger screenshot capture through the hooks, provided the browser session is available.

Generated build output, reports, screenshots, and logs are excluded from Git tracking. Keep execution evidence separately when needed.

## Latest recorded local execution

**Date:** November 15, 2026  
**Execution:** Eclipse TestNG suite with Chrome, Firefox, and Edge  
**Java runtime:** JDK 25.0.5

| Result | Count |
| --- | ---: |
| Total scenario executions | 18 |
| Passed | 15 |
| Failed | 3 |
| Skipped | 0 |

The suite launched and completed. Purchase scenario failures were observed in the results; their cause has not yet been confirmed. This records the observed run and does not claim a fully passing suite.

## Acknowledgments

Based on the CloudBerry supplied hybrid to BDD framework. The existing page objects and shared framework core are reused with Cucumber features, step definitions, hooks, and a TestNG runner. This repository documents the local configuration and observed execution results.
