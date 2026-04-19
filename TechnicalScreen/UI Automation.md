Automation Framework Design 

Tech stack - Java, cucumber, appium 
It follows a hybrid BDD approach 
and build for both iOS and android platform 

Core tech stack 
  - uses Java 21 
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
---

LAYERED ARCHITECTURE

1. Feature layer - gherkin scenario

2. step defination layer - cucumber glue code - step def classes, logging , hooks, test context
   hooks - prepare the run context, reporting, device allocation, driver lifecycle , logs and evidence handling.

3. page object model(design pattern) - UI interation encapsulation
   Each app screen is represented as a class having page elemnets(locators) + actions that can be performed on that how
   

4.Utilities - 

---
FEATURE LAYER - GHERKIN  - CUCUMBER

Feature
The purpose of the Feature keyword is to provide a high-level description of a software feature, and to group related scenarios.

Rule
The (optional) Rule keyword has been part of Gherkin since v6.
The purpose of the Rule keyword is to represent one business rule that should be implemented. It provides additional information for a feature. A Rule is used to group together several scenarios that belong to this business rule. 

Steps
Each step starts with Given, When, Then, And, or But.
GIVEN - Precondition
WHEN- user action
THEN - Expected result
AND , BUT - 

Background - repeat steps before each scenario

Scenario outline : 
The Scenario Outline keyword can be used to run the same Scenario multiple times, with different combinations of values.
The keyword Scenario Template is a synonym of the keyword Scenario Outline.
Copying and pasting scenarios to use different values quickly becomes tedious and repetitive:
We can collapse these two similar scenarios into a Scenario Outline.
Scenario outlines allow us to more concisely express these scenarios through the use of a template with < >-delimited parameters:

Data tables = sending table of data 
DOC strings= large piece of text to step defination  
tags = group/organize fature
"Mocha" = passing argument

```
@Retail @CoffeeShop
Feature: Coffee Ordering System
  As a coffee lover,
  I want to order different types of coffee through the app
  So that I can enjoy my drink without waiting in line.

  Background:
    Given the coffee shop "The Daily Grind" is open
    And I am logged into the mobile application

  Rule: Users must have sufficient balance to place an order

    Scenario: Successful coffee order
      Given I have a balance of $10.00 in my account
      When I select a "Latte" which costs $4.50
      And I click the "Confirm Order" button
      Then the order should be placed successfully
      And my account balance should be $5.50

    Scenario Outline: Ordering various sizes
      Given I have a balance of $20.00
      When I choose a <size> <drink>
      Then the final price should be <price>

      Examples:
        | size   | drink     | price |
        | Small  | Espresso  | $3.00 |
        | Medium | Cappuccino| $5.00 |
        | Large  | Cold Brew | $6.50 |

  Scenario: Bulk order details
    Given I want to order for my team:
      | drink      | quantity |
      | Americano  | 2        |
      | Flat White | 3        |
    When I submit the group order
    Then the receipt should show a total of 5 drinks
    But no discount should be applied for orders under 10 drinks

  Scenario: Custom message on cup
    Given I order a "Mocha"
    When I add the following instruction:
      """
      Please write "Happy Birthday Agastya!" 
      on the side of the cup in green ink.
      """
    Then the barista should see the custom note
```
---
STEP defination layer 

### 1. Feature Layer (`login.feature`)
The Gherkin remains focused on the user's journey.

```gherkin
@Regression
Feature: User Authentication

  Background:
    Given the mobile application is launched

  Scenario: Successful login
    When I enter valid username and password
    And I tap on the "Login" button
    Then I should be redirected to the "Home" screen
```

---

### 2. Step Definition Layer (`LoginSteps.java`)
the steps interact directly with the **Page Objects** initialized in the page classes.

```java
public class LoginSteps {
    private static final Logger logger = LogManager.getLogger(LoginSteps.class);
    LoginPage loginPage = new LoginPage(ServiceHooks.getDriver());
    HomePage homePage = new HomePage(ServiceHooks.getDriver());

    @When("I enter valid username and password")
    public void enterCredentials() {
        logger.info("Step: Entering credentials");
        loginPage.enterUsername("naincy_gupta");
        loginPage.enterPassword("pass123");
    }

    @Then("I should be redirected to the {string} screen")
    public void verifyScreen(String screenName) {
        Assert.assertTrue(homePage.isHeaderDisplayed());
        logger.info("Step: Verified " + screenName + " screen");
    }
}
```
Step defination layer interacts with many files 
For page class object - page class - function to call locators are written here or any methods specific to page
locators are in json file 
JSON utility - to read locator json file 
<Read locators.json -> identify platform from system config -> retrun accessibility id>

### 1. The Locator JSON (`locators.json`)
You can store both platforms in one file, organized by page and platform.

```json
{
  "LoginPage": {
    "android": {
      "usernameField": "accessibility:username-field",
      "passwordField": "id:com.lingo.app:id/password",
      "loginButton": "xpath://android.widget.Button[@text='LOGIN']"
    },
    "ios": {
      "usernameField": "predicate:label == 'username'",
      "passwordField": "accessibility:password-field",
      "loginButton": "classChain:**/XCUIElementTypeButton[`label == 'Login'`]"
    }
  }
}
```

---

### 2. The JSON Utility (`JsonUtils.java`)
You need a simple helper to read the JSON based on the current platform.

```java
public class JsonUtils {
    public static String getLocator(String pageName, String elementKey) {
        // Implementation logic: 
        // 1. Read locators.json
        // 2. Identify platform (Android/iOS) from a config or System property
        // 3. Return the string (e.g., "accessibility:username-field")
        return "accessibility:username-field"; // Placeholder for logic
    }
}
```

---

### 3. The Page Object Layer (Interacting with JSON)
Instead of `@AndroidFindBy`, the page class now asks the utility for the locator string and uses a dynamic `By` object.

**`LoginPage.java`**
```java
public class LoginPage {
    private AppiumDriver driver;
    private String platform;

    public LoginPage(AppiumDriver driver, String platform) {
        this.driver = driver;
        this.platform = platform;
    }

    // Encapsulated UI Interaction
    public void login(String user, String pass) {
        getWebElement("usernameField").sendKeys(user);
        getWebElement("passwordField").sendKeys(pass);
        getWebElement("loginButton").click();
    }

    // Helper to find element dynamically
    private WebElement getWebElement(String elementKey) {
        String locatorValue = JsonUtils.getLocator("LoginPage", elementKey);
        // Logic to split "accessibility:value" and return By.AccessibilityId(value)
        return driver.findElement(MobileBy.AccessibilityId(locatorValue.split(":")[1]));
    }
}
```
BY OBJECT

In Selenium and Appium, a **By object** is a mechanism or "locator strategy" used to find and identify elements within a mobile app or webpage. Think of it as the **address format** you give to the driver so it knows exactly where to look.

#### The Logic Flow:
1.  Your **JSON** stores a string: `"id:com.lingo.app:id/password"`.
2.  Your **JsonUtils** splits that string into the type (`id`) and the value (`com.lingo.app:id/password`).
3.  Your code then creates a **By object** dynamically:
    ```java
    // This creates the 'By' object that Appium understands
    By myElement = By.id("com.lingo.app:id/password"); 
    
    // The driver uses that By object to find the element
    driver.findElement(myElement).click();
    ```



---

### 3. Hooks Layer (`ServiceHooks.java`)
This class now acts as the central manager for **Device Allocation**, **Driver Lifecycle**, and **Evidence Handling**.

```java
public class ServiceHooks {
    private static AppiumDriver driver;
    private static final Logger logger = LogManager.getLogger(ServiceHooks.class);

    // Global getter for Step Definitions to access the driver
    public static AppiumDriver getDriver() {
        return driver;
    }

    @Before
    public void prepareRunContext(Scenario scenario) throws MalformedURLException {
        logger.info("Initializing run for: " + scenario.getName());

        // 1. Device Allocation & Driver Lifecycle
        UiAutomator2Options options = new UiAutomator2Options()
                .setDeviceName("Android_Emulator")
                .setApp(System.getProperty("user.dir") + "/apps/lingo.apk");

        driver = new AndroidDriver(new URL("http://127.0.0.1:4723"), options);
        logger.info("Appium Driver started successfully.");
    }

    @After
    public void logsAndEvidenceHandling(Scenario scenario) {
        // 2. Reporting & Evidence Handling
        if (scenario.isFailed()) {
            final byte[] screenshot = ((TakesScreenshot) driver).getScreenshotAs(OutputType.BYTES);
            scenario.attach(screenshot, "image/png", "Failure_Evidence");
            logger.error("Scenario FAILED: " + scenario.getName() + " - Screenshot captured.");
        } else {
            logger.info("Scenario PASSED: " + scenario.getName());
        }

        // 3. Driver Lifecycle Cleanup
        if (driver != null) {
            driver.quit();
            logger.info("Appium session terminated.");
        }
    }
}
```

---

