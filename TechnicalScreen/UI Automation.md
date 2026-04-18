Automation Framework Design 

Tech stack - Java, cucumber, appium 
It follows a hybrid BDD approach 
and build for both iOS and android platform 

Core tech stack 
  - uses Java 21 ()
  - Test NG for  execution management
  - Cucumber for business readable scenarios
  - Appium for cross platform testing
  - Maven for dependency management
  - Extentreports for reporting

---
TESTNG

  TestNG (Test Next Generation) is an open-source automated testing framework
  Core Features of TestNG:
  1. Annotations: Uses a rich set of annotations (e.g., @Test, @BeforeMethod, @AfterClass) to manage the execution lifecycle.
  2. Parallel Execution: Built-in support for running tests in parallel across multiple threads
  3. Data-Driven Testing: The @DataProvider annotation allows you to run the same test case multiple times with different sets of data
  4. Test Suites via XML: You can define which tests to run, group them, and set execution order using a testng.xml configuration file.
  5. Grouping: Allows you to categorize tests into groups like "Smoke," "Regression," or "Sanity,"

---
CUCUMBER

  Cucumber acts as a bridge between technical and non-technical stakeholders. It translates Gherkin steps into executable code.
  Feature Files (.feature): Where you write scenarios using Given, When, Then.
  Step Definitions: The underlying Java code that executes the logic for each Gherkin step.

  How to Integrate (The Java/Appium Stack)
  1. Add Dependencies: In your pom.xml, add cucumber-java, cucumber-testng
  2. Create a Runner Class: This is a TestNG class that tells Cucumber where to find the features and step definitions.

  Java
  @CucumberOptions(
      features = "src/test/resources/features",
      glue = "com.automation.stepdefinitions",
      plugin = {"pretty", "html:target/cucumber-reports.html"}
  )
  public class TestRunner extends AbstractTestNGCucumberTests { }


  1. ANNOTATION : @Given (and other Keywords)
  These are Annotations used in your Step Definition files to map a Gherkin step to a Java method.
  @Given: Describes the initial context or "precondition" (e.g., @Given("User is on the login page")).
  @When: Describes the action (e.g., clicking a button).
  @Then: Describes the expected outcome (e.g., verifying a dashboard appears).
  
  2. TAGS :  (@tags)
  Tags are used to organize and filter your tests. You can place them above a Feature or a specific Scenario.
  Example: @smoke, @regression, @ios.
  Usage: You can tell your Runner to only execute @smoke tests to save time during CI/CD.
  
  3. GLUE
  The Glue is a parameter in your TestRunner that provides the path to your Step Definition classes.
  Without the "glue," Cucumber won't know which Java code corresponds to the English steps in your feature file.
  Example: glue = {"stepdefinitions", "hooks"}.

---
APPIUM

 is an open-source, cross-platform automation tool used for testing native, hybrid, and mobile web applications on iOS, Android, and Windows desktop platforms.

Appium functions as a WebDriver server. It uses the same W3C WebDriver protocol

Appium allows you to write one test suite (in Java, for example) and run it against multiple platforms. It achieves this by using "Drivers" that translate your commands into the platform's native testing framework:
Android: Uses the UiAutomator2 driver.
iOS: Uses the XCUITest driver.

appium 2.0 vs 1.0 : 
- Decoupled Drivers:
- Plugins: You can now add functionality like the images plugin for visual testing or the device-farm plugin for managing multiple devices without changing the core server.

---
MAVEN
Maven is a powerful build automation and project management tool p

What is Maven?
At its core, Maven uses a POM (Project Object Model) file—the pom.xml—to describe the project's configuration, including its dependencies, build directory, source directory, and plugins.

Why the Industry Uses Maven
The software industry shifted toward Maven (and later Gradle) because it solved the "Dependency Hell" that plagued early Java development.

1. Centralized Dependency Management
Before Maven: Developers had to manually download .jar files (like the Appium Java Client), add them to the project path, and manually update them when a new version was released.

With Maven: You simply add a few lines of XML to your pom.xml. Maven automatically downloads the library and—critically—all the other libraries that library depends on (transitive dependencies).

2. Standardized Project Structure
Maven enforces a standard directory layout:
src/main/java: For application code.
src/test/java: For your automation scripts.
src/test/resources: For your .feature files and config properties.
Because this structure is universal, any developer can join your project and immediately know where the files are located.

3. Easy Integration with CI/CD
Maven is designed to be run from the command line (e.g., mvn test). This makes it incredibly easy to integrate with tools like Jenkins or GitHub Actions.

---
EXTENTREPORTS
ExtentReports is a popular choice for industry-standard automation frameworks because it transforms raw test data into a highly visual, stakeholder-ready format.

1. Rich Visual Dashboards
2. Built-in Multimedia Support
In mobile automation (Appium), seeing the failure is critical. ExtentReports allows you to:
Embed Screenshots: Attach a screenshot directly to the failed step so you don't have to hunt through folders.
Video Logs: Link screen recordings of the test execution.
3. Historical Data & Trends
   - Flakiness Trends
4. categorization (Tags & Authors)
Since you are using Cucumber Tags, ExtentReports can automatically categorize your results based on those tags.

