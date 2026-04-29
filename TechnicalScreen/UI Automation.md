# Automation Framework Design 

## Tech stack 
Java, cucumber, appium 
It follows a hybrid BDD approach 
and build for both iOS and android platform 

## Core tech stack 
  - uses Java 21 
  - Test NG for  execution management
  - Cucumber for business readable scenarios
  - Appium for cross platform testing
  - Maven for dependency management
  - Extentreports for reporting

-----------
## JAVA

-----------
## APPIUM

### DEF
It is cross-platform test automation framework
Appium operates on a Client-Server Architecture. 
Client-Server Architecture: Your test script uses appium client library to send commands to the Appium server (HTTP server).
Appium java client defines "Desired Capabilities" (like device name, platform version, and app path).
The server interprets these commands and uses platform-specific drivers (e.g., XCUITest for iOS, UIAutomator2 for Android) to execute actions on devices

### Why appium ?
It acts as a bridge so you don't have to write platform-specific code.
It uses the WebDriver protocol
The WebDriver Protocol (specifically the W3C WebDriver Protocol) is a standardized, platform-independent REST API that allows a client (your test script) to communicate with a remote server (a browser or a mobile device) to control its behavior.
In simpler terms, it is the "common language" that automation tools use to tell a piece of software to "click this," "type that," or "go to this URL."

### System Requirements
Node.js: Appium is a Node.js application.
Android: Android SDK, Android Studio/emulators, or real devices.
iOS: Xcode, macOS, Simulators/real devices.
Appium Client: A driver/client library specific to your chosen language

### Things associated with Appium
Apple maintains an iOS automation technology called XCUITest. The Appium driver that supports iOS app automation is called the XCUITest Driver because ultimately what it does is convert the WebDriver protocol to XCUITest library calls.

### Appium is an HTTP server
It must run as a process on some computer for as long as you want to be able to use it for automation
The Appium server and the Appium client do not need to be running on the same computer. You simply need to be able to send HTTP requests from the client to the server over some network.

### APPIUM GRID ?

### COMMON APPIUM CAPABILITIES: 
appium:automationName  -> The name of the Appium driver to use
appium:udid -> The unique device identifier of a particular device to automate
appium:app -> The path to an installable application

combine all capabilities under appium:options 

XCUITest driver recommends that at least one of browserName, appium:app, or appium:bundleId
If you use a lot of appium: capabilities in your tests, it can get a little repetitive. You can combine all capabilities as an object value of a single appium:options capability instead
{
    "platformName": "iOS",
    "appium:options": {
        "automationName": "XCUITest",
        "platformVersion": "16.0",
        "app": "/path/to/your.app",
        "deviceName": "iPhone 12",
        "noReset": true
    }
}


### JavascriptExecutor
JavascriptExecutor jsDriver = (JavascriptExecutor) driver;
jsDriver.executeScript("return arguments[0] + arguments[1]", 3, 4);

1. Interacting with Hybrid Apps (WebViews)
When your mobile app has a WebView (a browser window inside the app), Appium treats it like a mobile browser. You use JavascriptExecutor to:
- Scroll to elements that are not yet in the viewport.
- Click hidden elements that the native driver cannot "see" or interact with due to overlay issues.
- Retrieve page data like the page title or URL that might not be exposed via native selectors.

```
public void testHybridWebViewInteraction() {
        // 1. Identify and Switch to WebView Context
        // Mobile apps start in NATIVE_APP context. You must switch to interact with HTML.
        Set<String> contextNames = driver.getContextHandles();
        for (String contextName : contextNames) {
            if (contextName.contains("WEBVIEW")) {
                driver.context(contextName); // Switching to WebView
                break;
            }
        }

        // 2. Locate an element within the WebView
        WebElement hiddenElement = driver.findElement(AppiumBy.id("secret-input"));

        // 3. Use JavascriptExecutor to Change Attribute Values
        // Example: Changing a 'hidden' field to 'text' so we can interact with it
        JavascriptExecutor js = (JavascriptExecutor) driver;
        
        // Changing 'type' attribute from 'hidden' to 'text'
        js.executeScript("arguments[0].setAttribute('type', 'text')", hiddenElement);
        
    }
```

### HOW TO USE APPIUM STEP BY STEP 
Step 1: Add Maven Dependencies
You need the Appium Java Client. Add this to your pom.xml

Step 2: Set Up Desired Capabilities (Options)
XCUITestOptions options = new XCUITestOptions();
options.setDeviceName("iPhone 15");
options.setApp("/path/to/your/app.ipa");
options.setPlatformVersion("17.0");

Step 3: Initialize the Driver
The driver is the heart of your script. It sends your commands to the Appium Server (usually running at http://127.0.0.1:4723).

Step 4: Write the Test Logic


### What are Desired Capabilities?
These are key-value pairs sent by the client to the server to define the mobile session. Examples include platformName (Android/iOS), deviceName, app (path to .apk/.ipa), and automationName.

### What are the limitations of Appium?
- Setup can be complex (Node.js, JDK, Android SDK/Xcode).
- Tests can be slower than native framework tests.

### What is appPackage and appActivity in Android?
appPackage: Unique identifier for the Android app (e.g., com.example.app).
appActivity: The specific screen or activity to start within the app (e.g., .MainActivity).

### Key Appium Objects for iOS (XCUITest)
1. IOSDriver
IOSDriver driver = new IOSDriver(new URL("http://127.0.0.1:4723"), options);

2. XCUITestOptions
Replaces the old DesiredCapabilities. This is where you define the specific environment for an iPhone or iPad.
  Key Capabilities for Apple:
    setDeviceName("iPhone 15 Pro")
    setPlatformVersion("17.4")
    setBundleId("com.apple.example") (The unique ID for iOS apps)
    setAutomationName("XCUITest")

3. AppiumBy (iOS Specific Strategies)
AppiumBy is the specialized locator class in Appium (Java-Client 8.x and above) that replaces the older MobileBy. It extends the standard Selenium By class to include mobile-specific selection strategies.
AppiumBy.iOSClassChain("**/XCUIElementTypeButton[label == 'Login']")

5. AppiumFieldDecorator
Used in your Page Object Model. Supports Multi-Platform Annotations.
```
import io.appium.java_client.pagefactory.AndroidFindBy;
import io.appium.java_client.pagefactory.AppiumFieldDecorator;
import io.appium.java_client.pagefactory.iOSXCUITFindBy;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.support.PageFactory;

public class LoginPage {

    // AppiumFieldDecorator allows these platform-specific annotations to work
    @AndroidFindBy(accessibility = "user-id-android")
    @iOSXCUITFindBy(accessibility = "user-id-ios")
    private WebElement usernameField;

    public LoginPage(AppiumDriver driver) {
        // Initialize elements using AppiumFieldDecorator
        PageFactory.initElements(new AppiumFieldDecorator(driver), this);
    }
}
```

### Write a sample code to launch a mobile application using Appium in Java with AppiumOptions.
Answer: Here’s an updated example to set up and launch a mobile application using Appium in Java:

```
import io.appium.java_client.MobileElement;
import io.appium.java_client.android.AndroidDriver;
import io.appium.java_client.remote.options.AppiumOptions;
import java.net.URL;

public class AppiumTest {
   public static void main(String[] args) {
       AppiumOptions options = new AppiumOptions();
       options.setPlatformName("Android");
       options.setPlatformVersion("10.0");
       options.setDeviceName("Pixel_3a");
       options.setApp("/path/to/your/app.apk");

       try {
        URL serverUrl = new URL("http://127.0.0.1:4723");
         IOSDriver driver = new IOSDriver(serverUrl, options); 
           // Your test code here
           driver.quit();
       } catch (Exception e) {
           e.printStackTrace();
       }
   }
}
```

### Web-Based Version: A browser version is available at inspector.appiumpro.com.
Note: To use the web version with a local server, you must start the Appium server with the --allow-cors flag: appium --allow-cors.

### XCUITestOptions and UiAutomator2Options
In modern mobile automation, the enterprise standard has shifted away from the generic DesiredCapabilities class toward Platform-Specific Options (like XCUITestOptions and UiAutomator2Options).
As of Appium 2.0, using platform-specific options is the highly recommended practice because it provides type safety, better discovery of capabilities, and follows the latest W3C WebDriver standards.

```
XCUITestOptions options = new XCUITestOptions();

// Set common Appium options
options.setPlatformName("iOS");
options.setAutomationName("XCUITest");
options.setDeviceName("iPhone 15 Pro");

// Set iOS-specific options
options.setBundleId("com.apple.Maps");
options.setWdaLaunchTimeout(Duration.ofSeconds(30));
options.setNoReset(true);

// Initialize the driver with these specific options
IOSDriver driver = new IOSDriver(new URL("http://127.0.0.1:4723"), options);
```


### How do you handle system permissions in your framework?

I utilize XCUITestOptions to set **setAutoAcceptAlerts(true)** during the driver initialization. This ensures that the WDA automatically handles iOS system-level pop-ups like Location or Camera permissions, preventing the tests from hanging.


###  ⭐ How do you handle gestures in Appium?
gestures like swiping, scrolling, and pinching are handled using the W3C Actions API.

Appium treats a gesture as a series of "events" performed by a "finger" (the Pointer).
PointerInput: Represents the virtual finger.
Sequence: A list of actions (Move, Pointer Down, Move, Pointer Up).
VERTICAL SWIPE
// 1. Create a Finger (Touch) input
PointerInput finger = new PointerInput(PointerInput.Kind.TOUCH, "finger");

// 2. Create a Sequence
Sequence swipe = new Sequence(finger, 1);

// 3. Define the steps individually
// Start at (x, y)
swipe.add(finger.createPointerMove(Duration.ZERO, PointerInput.Origin.viewport(), 500, 800));
// Press down
swipe.add(finger.createPointerDown(PointerInput.MouseButton.LEFT.asArg()));
// Move to end position (Duration controls speed)
swipe.add(finger.createPointerMove(Duration.ofMillis(600), PointerInput.Origin.viewport(), 500, 200));
// Lift finger
swipe.add(finger.createPointerUp(PointerInput.MouseButton.LEFT.asArg()));

// 4. Execute the gesture
driver.perform(Collections.singletonList(swipe));



### The "Mobile" Command Shortcut (iOS Specific)
you can use the mobile: executeScript command

Common iOS Mobile Commands:
mobile: scroll
mobile: swipe
mobile: pinch
mobile: tap

```
Map<String, Object> params = new HashMap<>();
params.put("direction", "down");
params.put("elementId", ((RemoteWebElement) element).getId());

driver.executeScript("mobile: swipe", params);
```


✅  In your interview, explain that you prefer W3C Actions for custom, complex gestures (like drawing a pattern) but use Mobile Commands for standard OS gestures because they are more stable and optimized for iOS.


### What are some common issues faced during Appium testing?
1. WebDriverAgent (WDA) Failures
The WDA is the "engine" that Appium installs on the iOS device to communicate with XCUITest.
The Issue: The WDA can frequently crash, hang, or fail to install due to code-signing issues or version mismatches between Xcode and Appium.
The Fix: Use XCUITestOptions to set **setUseNewWDA(true)** if the session is stuck, or manually rebuild the WDA project in Xcode to ensure the provisioning profiles are correct.

2. Element Visibility & "Quiescence"
iOS is very strict about whether an element is "interactable."
The Issue: XCUITest often waits for the app to be completely "idle" (quiescent) before performing an action. If your app has a looping animation (like a loading spinner), the test might hang and timeout.
The Fix: Use the capability **setWaitForQuiescence(false)** in your XCUITestOptions to allow the test to proceed even if animations are running.

3. Handling System Alerts and Permissions
iOS frequently interrupts tests with "Allow 'App' to use your location?" or "App would like to send you notifications."
The Issue: These alerts are not part of the app's UI; they are part of the iOS System UI. Standard driver.findElement calls will fail because the alert blocks the app.
The Fix: Use **options.setAutoAcceptAlerts(true)** for generic handling, or use mobile: acceptAlert via executeScript for mid-test alerts.

4. I use **setShouldTerminateApp(true)** to ensure every test starts from a clean state, which prevents 'leftover' data from one test affecting the next."

### Imp Appium links - 
https://www.testmuai.com/learning-hub/appium-interview-questions/

### Application Crash Handling
Manage app crashes using autoLaunch and noReset capabilities, monitor current activity, and restart apps with driver.resetApp() during execution.

1 - The "Clean State" Strategy - 
setShouldTerminateApp(true): Ensures that if the app was previously hung or crashed in a "zombie" state, it is killed before the new test begins.
setFullReset(true): (Use sparingly) Deletes the app and all its data, ensuring a 100% clean install to avoid crashes caused by old cache.


2  - The Recommended Framework Sequence
You should implement this logic across three layers: the Initialization, the Execution Wrapper, and the Listener.
1. Pre-Test: The "Clean Slate" (BaseTest.java)
Before any test runs, you must ensure the environment is stable.
```
public void setup() {
    XCUITestOptions options = new XCUITestOptions();
    options.setDeviceName("iPhone 15");
    options.setBundleId("com.apple.example");
    
    // 1. Terminate any zombie processes from previous failed runs
    options.setShouldTerminateApp(true); 
    
    // 2. Clear app state if the test requires a fresh login
    options.setNoReset(false); 

    driver = new IOSDriver(new URL("http://127.0.0.1:4723"), options);
}
```

2. During Test: The "State Check" (Utility Layer)
Don't just wait for a TimeoutException. Create a utility method to check the app health before critical actions.
Java
```
public void performSafeAction(WebElement element) {
    // Check if app is still in foreground before interacting
    ApplicationState state = driver.queryAppState("com.apple.example");
    
    if (state != ApplicationState.RUNNING_IN_FOREGROUND) {
        // Log the crash and fail the test immediately with a clear message
        throw new RuntimeException("App crashed or backgrounded! Current State: " + state);
    }
    element.click();
}
```

4. Post-Failure: The "Recovery & Evidence" (TestListener.java)
This is where you handle an actual crash. Use a TestNG Listener to capture logs specifically for iOS.
```
@Override
public void onTestFailure(ITestResult result) {
    // 1. Capture Screenshot
    File srcFile = driver.getScreenshotAs(OutputType.FILE);
    
    // 2. Capture iOS System Logs (Crucial for Apple Devs)
    LogEntries logs = driver.manage().logs().get("syslog");
    
    // 3. Attempt Recovery (Optional: Restart app for next test)
    Map<String, Object> params = new HashMap<>();
    params.put("bundleId", "com.apple.example");
    driver.executeScript("mobile: launchApp", params);
}
```


### NETWORK HANDLING 

Simulating "No Internet" (Airplane Mode)
Using Mobile Commands
In iOS, the most reliable way to toggle connectivity is via **mobile: setNetworkConnection.**
```
// Define the parameters for Airplane Mode
Map<String, Object> params = new HashMap<>();
params.put("type", "airplane-mode"); // This cuts off both WiFi and Data
driver.executeScript("mobile: setNetworkConnection", params);
```


2. Simulating "Poor/Slow Internet" (Latency)
Apple's XCUITest doesn't have a direct "Slow Network" capability like Android's networkSpeed. Instead, you use Network Conditioning.
Using XCUITestOptions
You can set a network profile during the driver initialization. This is useful for testing "Limited" internet (e.g., 3G or Edge).
```
XCUITestOptions options = new XCUITestOptions();
options.setDeviceName("iPhone 15");
// Simulates specific network profiles
// Common values: 'Very Bad Network', 'LTE', '3G', 'Edge'
options.setCapability("appium:networkCondition", "3G");
IOSDriver driver = new IOSDriver(new URL("http://127.0.0.1:4723"), options);
```

### iOS ssl and security scenarios and handling summarize with example
```
options.setAcceptInsecureCerts(true);
System Security Alerts
// Automatically clicks "Allow" or "OK" on all iOS system privacy alerts options.setAutoAcceptAlerts(true);
BIOMETRIC
// 1. Ensure the simulator is capable of biometrics
options.setCapability("appium:allowTouchIdEnroll", true);

// 2. Mid-test: Simulate a successful FaceID match
Map<String, Object> params = new HashMap<>();
params.put("type", "faceId"); // or "touchId"
params.put("match", true);    // true for success, false for failure
driver.executeScript("mobile: sendBiometricMatch", params);
```
----------------------------------------------------------------------
# TESTNG

  TestNG (Test Next Generation) is an open-source automated testing framework
  
  ### Core Features of TestNG:
  1. Annotations: Uses a rich set of annotations (e.g., @Test, @BeforeMethod, @AfterClass) to manage the execution lifecycle.
  2. Parallel Execution: Built-in support for running tests in parallel across multiple threads
  3. Data-Driven Testing: The @DataProvider annotation allows you to run the same test case multiple times with different sets of data
  4. Test Suites via XML: You can define which tests to run, group them, and set execution order using a testng.xml configuration file.
  5. Grouping: Allows you to categorize tests into groups like "Smoke," "Regression," or "Sanity,"

### @Test
The @Test annotation marks a method as a test method. It can be configured with attributes to control its behavior.
  Attributes
  priority: Defines the execution order of tests.
  enabled: If set to false, the test method will be skipped.
  dependsOnMethods: Specifies dependencies on other test methods.
  groups: Assigns the test method to one or more groups.
  dataProvider: Links the test method to a data provider method for data-driven testing.

### @BeforeSuite and @AfterSuite
@BeforeSuite and @AfterSuite are executed before and after all tests in the suite, respectively. They are ideal for setting up and tearing down global test configurations.
Initializing and closing a database connection or setting up and cleaning up environment variables before and after the entire test suite runs.

### @BeforeTest and @AfterTest
These annotations are executed before and after the <test> tag in the TestNG XML file.
Setting up a web driver instance before starting the test execution and quitting the web driver instance after completing the test execution defined under a specific <test> tag.

### Data Management & Extensibility
@DataProvider: Returns a Object[][] or Iterator<Object[]> to feed multiple data sets into a single test.

@Parameters: Pulls simple values (like browserName) directly from the testng.xml file.

@Listeners: Used to implement ITestListener or IRetryAnalyzer. This is how you automate screenshots on failure or retry flaky tests.


### 1. IAnnotationTransformer (The Dynamic Modifier)
This is used to **modify TestNG annotations at runtime**. Its most common use case is automatically adding a **Retry Analyzer** to every `@Test` without manually typing `retryAnalyzer = ...` on hundreds of methods.

**Enterprise Use Case:** Automatically retrying flaky tests.

```java
import org.testng.IAnnotationTransformer;
import org.testng.annotations.ITestAnnotation;
import java.lang.reflect.Constructor;
import java.lang.reflect.Method;

public class MyTransformer implements IAnnotationTransformer {
    @Override
    public void transform(ITestAnnotation annotation, Class testClass, 
                          Constructor testConstructor, Method testMethod) {
        // Automatically set the Retry Analyzer for all tests
        annotation.setRetryAnalyzer(RetryAnalyzer.class);
    }
}
```


### 2. ITestListener (The Observer)
This is the most widely used listener. It monitors the "health" of your tests in real-time.

**Enterprise Use Case:** Taking screenshots exactly when a test fails or logging results to Extent Reports.

```java
import org.testng.ITestListener;
import org.testng.ITestResult;

public class MyListener implements ITestListener {
    @Override
    public void onTestFailure(ITestResult result) {
        System.out.println("Test Failed: " + result.getName());
        // Logic to capture screenshot using the driver instance
        captureScreenshot(result.getName());
    }

    @Override
    public void onTestSuccess(ITestResult result) {
        System.out.println("Test Passed: " + result.getName());
    }
}
```



### 3. IRetryAnalyzer (The Self-Healer)
While not a "Listener" by name, it works alongside them to decide if a failed test should be executed again.

**Enterprise Use Case:** Reducing "noise" in CI/CD pipelines caused by environment instability.

```java
import org.testng.IRetryAnalyzer;
import org.testng.ITestResult;

public class RetryAnalyzer implements IRetryAnalyzer {
    private int count = 0;
    private static final int MAX_RETRY = 2; // Retry twice

    @Override
    public boolean retry(ITestResult result) {
        if (count < MAX_RETRY) {
            count++;
            return true; // Tells TestNG to run the test again
        }
        return false;
    }
}
```


### 4. ISuiteListener
Runs before and after the entire `<suite>` in your `testng.xml`.

**Enterprise Use Case:** Starting/Stopping a Docker container or an Appium server programmatically.

```java
public class SuiteManager implements ISuiteListener {
    @Override
    public void onStart(ISuite suite) {
        System.out.println("Starting Appium Server for Suite: " + suite.getName());
    }
}
```


```xml
<listeners>
    <listener class-name="com.qa.listeners.MyListener" />
    <listener class-name="com.qa.listeners.MyTransformer" />
</listeners>
```

\
---
# CUCUMBER

  Cucumber acts as a bridge between technical and non-technical stakeholders. It translates Gherkin steps into executable code.
  Feature Files (.feature): Where you write scenarios using Given, When, Then.
  Step Definitions: The underlying Java code that executes the logic for each Gherkin step.

  How to Integrate (The Java/Appium Stack)
  1. Add Dependencies: In your pom.xml, add cucumber-java, cucumber-testng
  2. Create a Runner Class: This is a TestNG class that tells Cucumber where to find the features and step definitions.

  ```
  @CucumberOptions(
      features = "src/test/resources/features",
      glue = "com.automation.stepdefinitions",
      plugin = {"pretty", "html:target/cucumber-reports.html"}
  )
  public class TestRunner extends AbstractTestNGCucumberTests { }
```

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

### Feature
The purpose of the Feature keyword is to provide a high-level description of a software feature, and to group related scenarios.

### Rule
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
tags = group/organize feature
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

### What is BDD (Behavior Driven Development)?

Answer: A software development process that encourages collaboration between developers, QA, and non-technical business stakeholders. It focuses on the behavior of the application rather than just the implementation.

### What is the difference between Scenario and Scenario Outline?

Answer: A Scenario is a single test case. A Scenario Outline is used for Data-Driven Testing, allowing you to run the same scenario multiple times with different sets of data provided in an Examples: table.

### What is the purpose of the Background keyword?

Answer: It is used to define steps that are common to all scenarios in a feature file (e.g., "Given the user is logged in"). It runs before every scenario in that file.

### Can you pass parameters from Gherkin to Java?

Answer: Yes, using Cucumber Expressions or Regular Expressions in the Step Definition. For example: Given I have {int} items in my cart.

---------------------

# MAVEN
Maven is a powerful build automation and project management tool 

### What is Maven?
At its core, Maven uses a POM (Project Object Model) file—the pom.xml—to describe the project's configuration, including its dependencies, build directory, source directory, and plugins.

### Why the Industry Uses Maven
The software industry shifted toward Maven (and later Gradle) because it solved the "Dependency Hell" that plagued early Java development.

###  Centralized Dependency Management
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
# EXTENTREPORTS
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

# TEST AUTOMATION 

LAYERED ARCHITECTURE

1. Feature layer - gherkin scenario - Business readable scenario

2. step defination layer - cucumber glue code -
   Translates gherkin step to business actions

 3. Page classes and utilities - encapsulate UI interation
    POM - Page object model - design pattern
   Each app screen is represented as a class having page elemnets(locators) + actions that can be performed on that layer

4. Utilities -
      driver mgmt - Factory design - creational design pattern
      centralized driver creation , threadlocal instance of driver

      config mgmt - env specific property files are kept

      screenshot utility - (TakeScreenshot)

      Log4j utility -

   5. hooks - prepare the run context, reporting, device allocation, driver lifecycle , logs and evidence handling.

    6. test context


### KEY FEATURE

1. 
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
UTILITY LAYER

## Factory patter layer - creational pattern - process of object creation
1- DriverFactory 
createDriver() -> create driver according to configreader platform (browserstack/local driver)
getdriver() -> if the driver is null -> throw illegastateexception -> driver not initialised
quitdriver() => quit driver 


2- Localdriver manager
Checks config reader and create ios/android specific driver
set the capabilities according to the platform

3- Browserstackdriver manager 
same as above

4- appium server manager
startserver() stopserver()


## Hooks

@beforeall
- runcontext
- extentmanager
- start appium server

@before
- set platform context from cmd line
- set devicecontext - browserstack/local/devicepool
- extent manager.setTest
- driverfactory.createdriver()

@after
- driver.quit()
- extentmanager.flush()
- devicecontext.clear()
- scenarioartifacts.clear
- platformcontext.clear

## Listeners
- deviceexecution summary
  ItestListener, Isuitelistener
  onTestSuccess(ITestResult result) -> add passed label to the test
  onTestSkipped(ITestResult result) -> add skipped label to the test
  onFinish(ISuite suite) -> suite execution is finished 

## main/Utils
- Configreader
read properties file to get env, platform
- device context
- run context
- platform context
- device pool
LinkedBlockingQueue - offer , poll
- log

screenshot, recording

## test/Utils

Mobile and Scroll utility 

SWIPE ELEMENT
### Core Swipe Methods Summary
1. Android Swipe Method - mobile: swipeGesture
// ANDROID: Uses UiAutomator2 native command
Map<String, Object> params = new HashMap<>();
params.put("elementId", elementId);     // Target element ID
params.put("direction", "left");        // "left", "right", "up", "down"
params.put("percent", 0.8);            // How far to swipe (0.1 - 0.95)
params.put("speed", 2600);             // Swipe speed (pixels/second)
js.executeScript("mobile: swipeGesture", params);

2. iOS Swipe Method - mobile: swipe
// iOS: Uses XCUITest native command  
Map<String, Object> params = new HashMap<>();
params.put("elementId", elementId);     // Target element ID
params.put("direction", "left");        // Swipe direction
js.executeScript("mobile: swipe", params);

3. W3C Actions Fallback - Cross-Platform
// W3C Actions: Works on both Android and iOS
PointerInput finger = new PointerInput(PointerInput.Kind.TOUCH, "finger");
Sequence sequence = new Sequence(finger, 1);

sequence.addAction(finger.createPointerMove(Duration.ZERO, Origin.viewport(), startX, startY));
sequence.addAction(finger.createPointerDown(MouseButton.LEFT.asArg()));
sequence.addAction(finger.createPointerMove(Duration.ofMillis(500), Origin.viewport(), endX, endY));
sequence.addAction(finger.createPointerUp(MouseButton.LEFT.asArg()));

driver.perform(Collections.singletonList(sequence));

```
In mobile automation, **W3C Actions** (specifically the **Actions API**) are the industry-standard way to simulate complex user gestures that go beyond simple clicks. They are platform-agnostic, meaning the same logic works for both Android and iOS.

These actions are represented in Java by the `PointerInput` and `Sequence` classes.

---

### Core W3C Gesture Components
To build any gesture, you use these four basic building blocks:
1.  **PointerMove:** Moves the "finger" to a specific $(x, y)$ coordinate.
2.  **PointerDown:** Presses the "finger" onto the screen.
3.  **PointerMove (with duration):** Drags the "finger" to a new coordinate over a set time.
4.  **PointerUp:** Lifts the "finger" off the screen.

---

### 1. Swipe / Scroll (Vertical or Horizontal)
This is the most common action. It mimics a finger pressing down, moving across the screen, and lifting up.

```java
public void swipe(int startX, int startY, int endX, int endY) {
    PointerInput finger = new PointerInput(PointerInput.Kind.TOUCH, "finger");
    Sequence sequence = new Sequence(finger, 1);

    // 1. Move to Start Position
    sequence.addAction(finger.createPointerMove(Duration.ZERO, Origin.viewport(), startX, startY));
    // 2. Press Down
    sequence.addAction(finger.createPointerDown(PointerInput.MouseButton.LEFT.asArg()));
    // 3. Move to End Position (The actual swipe)
    sequence.addAction(finger.createPointerMove(Duration.ofMillis(600), Origin.viewport(), endX, endY));
    // 4. Lift Up
    sequence.addAction(finger.createPointerUp(PointerInput.MouseButton.LEFT.asArg()));

    driver.perform(Collections.singletonList(sequence));
}


### 2. Long Press
Used for opening context menus or activating specific UI states. The key here is adding a **Pause** between `PointerDown` and `PointerUp`.

```java
public void longPress(WebElement element) {
    Point location = element.getLocation();
    PointerInput finger = new PointerInput(PointerInput.Kind.TOUCH, "finger");
    Sequence sequence = new Sequence(finger, 1);

    sequence.addAction(finger.createPointerMove(Duration.ZERO, Origin.viewport(), location.x, location.y));
    sequence.addAction(finger.createPointerDown(PointerInput.MouseButton.LEFT.asArg()));
    // The "Long" part: Pause for 2 seconds
    sequence.addAction(new Pause(finger, Duration.ofSeconds(2)));
    sequence.addAction(finger.createPointerUp(PointerInput.MouseButton.LEFT.asArg()));

    driver.perform(Collections.singletonList(sequence));
}


### 3. Drag and Drop
This involves moving to an object, picking it up, moving it to a new location, and dropping it.

```java
public void dragAndDrop(WebElement source, WebElement target) {
    Point start = source.getLocation();
    Point end = target.getLocation();

    PointerInput finger = new PointerInput(PointerInput.Kind.TOUCH, "finger");
    Sequence sequence = new Sequence(finger, 1);

    sequence.addAction(finger.createPointerMove(Duration.ZERO, Origin.viewport(), start.x, start.y));
    sequence.addAction(finger.createPointerDown(PointerInput.MouseButton.LEFT.asArg()));
    sequence.addAction(new Pause(finger, Duration.ofMillis(500))); // Small pause to "grip" the object
    sequence.addAction(finger.createPointerMove(Duration.ofMillis(1000), Origin.viewport(), end.x, end.y));
    sequence.addAction(finger.createPointerUp(PointerInput.MouseButton.LEFT.asArg()));

    driver.perform(Collections.singletonList(sequence));
}


### 4. Double Tap
Simulates a quick double-click. It requires repeating the down/up sequence twice in rapid succession.

```java
public void doubleTap(int x, int y) {
    PointerInput finger = new PointerInput(PointerInput.Kind.TOUCH, "finger");
    Sequence sequence = new Sequence(finger, 1);

    // Tap 1
    sequence.addAction(finger.createPointerMove(Duration.ZERO, Origin.viewport(), x, y));
    sequence.addAction(finger.createPointerDown(PointerInput.MouseButton.LEFT.asArg()));
    sequence.addAction(finger.createPointerUp(PointerInput.MouseButton.LEFT.asArg()));
    // Short Gap
    sequence.addAction(new Pause(finger, Duration.ofMillis(100)));
    // Tap 2
    sequence.addAction(finger.createPointerDown(PointerInput.MouseButton.LEFT.asArg()));
    sequence.addAction(finger.createPointerUp(PointerInput.MouseButton.LEFT.asArg()));

    driver.perform(Collections.singletonList(sequence));
}


 Why use W3C Actions over `mobile:` commands?

* **Platform Uniformity:** You don't need separate code for `mobile: swipeGesture` (Android) and `mobile: swipe` (iOS). One W3C method handles both.
* **Precision:** You control the exact pixels and the exact milliseconds of the movement.
* **Multi-Finger Gestures:** You can create multiple `Sequence` objects (e.g., `finger1`, `finger2`) and run them simultaneously via `driver.perform(Arrays.asList(seq1, seq2))` to simulate **Pinch-to-Zoom**.

```

### Error Handling:

// Framework tries native first, falls back to W3C
try {
    if (isAndroid()) {
        androidSwipeGesture(elementId, direction, percent);
    } else {
        iosSwipeCommand(elementId, direction);
    }
} catch (Exception e) {
    // Fallback to W3C Actions
    w3cSwipe(startX, startY, endX, endY, duration);
}


### Wait Methods - WebDriverWait with ExpectedConditions
Universal Mechanism (Both Platforms):
// Selenium WebDriverWait - works on both platforms
WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(timeout));

// Different wait conditions:
wait.until(ExpectedConditions.visibilityOfElementLocated(locator));    // Visible
  ```
What it checks: It verifies that the element is present in the DOM AND that it is actually displayed on the screen (height and width $> 0$).
  ```
wait.until(ExpectedConditions.elementToBeClickable(locator));          // Clickable  
  ```
What it checks: This is the most "strict" condition. It verifies the element is present, visible, AND enabled (not grayed out/disabled).
```
wait.until(ExpectedConditions.presenceOfElementLocated(locator));  // Present
  ```
What it checks: It only verifies that the element exists in the DOM (Document Object Model) or the app's source tree.
```

### waitAndClick - Combined Wait + Action
// ANDROID: mobile:clickGesture
Map<String, Object> params = new HashMap<>();
params.put("x", x);    // Coordinate
params.put("y", y);    // Coordinate
js.executeScript("mobile: clickGesture", params);

// iOS: mobile:tap
Map<String, Object> params = new HashMap<>();
params.put("x", x);
params.put("y", y);
js.executeScript("mobile: tap", params);

### performHome - System Navigation
// ANDROID: mobile:pressKey with keycode
Map<String, Object> params = new HashMap<>();
params.put("keycode", 3);  // KEYCODE_HOME = 3
js.executeScript("mobile: pressKey", params);

// iOS: mobile:pressButton
Map<String, Object> params = new HashMap<>();
params.put("name", "home");
js.executeScript("mobile: pressButton", params);

5. backgroundApp - App State Management
// ANDROID: mobile:backgroundApp
Map<String, Object> params = new HashMap<>();
params.put("seconds", 3);  // Background duration
js.executeScript("mobile: backgroundApp", params);

// iOS: Native driver method
driver.runAppInBackground(Duration.ofSeconds(3));

6. foregroundApp - App Restoration

// ANDROID: mobile:startActivity
Map<String, Object> params = new HashMap<>();
params.put("appPackage", packageName);
params.put("appActivity", ".MainActivity");
js.executeScript("mobile: startActivity", params);

// iOS: Native driver method
driver.activateApp(appId);

 saveScreenshot - Evidence Capture
// Appium TakesScreenshot interface
TakesScreenshot screenshot = (TakesScreenshot) driver;
byte[] imageBytes = screenshot.getScreenshotAs(OutputType.BYTES);

// Save to file system
Files.write(Paths.get(fileName), imageBytes);

. hideKeyboard - Keyboard Dismissal
// PRIMARY: mobile:hideKeyboard
js.executeScript("mobile: hideKeyboard");

// FALLBACK: Press back key
params.put("keycode", 4);  // KEYCODE_BACK = 4
js.executeScript("mobile: pressKey", params);
// PRIMARY: Native driver method
driver.hideKeyboard();

// FALLBACK: Tap Done button
WebElement doneButton = driver.findElement(By.name("Done"));
doneButton.click();


forceQuitAndOpen - App Lifecycle

// ANDROID: mobile:terminateApp
Map<String, Object> params = new HashMap<>();
params.put("appId", packageName);

js.executeScript("mobile: terminateApp", params);
// iOS: Native driver method
driver.terminateApp(appId);


try {
    // Try platform-specific native command
    js.executeScript("mobile: command", params);
} catch (Exception e) {
    // Fallback to W3C Actions or alternative
    fallbackMethod();
}

---
## W3C Gestures 
(implemented via the **Actions API**) are the most powerful way to handle non-standard interactions in mobile automation. Because they use coordinates and "finger" sequences, they work identically for both **Android** and **iOS**.

Here are the most common gestures with their technical logic and code examples.


### 1. The Tap (Basic Interaction)
While `element.click()` is common, a W3C Tap is useful for clicking specific coordinates or handling elements that don't respond to standard clicks.

```java
public void tapAtCoordinates(int x, int y) {
    PointerInput finger = new PointerInput(PointerInput.Kind.TOUCH, "finger");
    Sequence tap = new Sequence(finger, 1);
    
    // Move to location -> Press -> Release
    tap.addAction(finger.createPointerMove(Duration.ZERO, Origin.viewport(), x, y));
    tap.addAction(finger.createPointerDown(PointerInput.MouseButton.LEFT.asArg()));
    tap.addAction(finger.createPointerUp(PointerInput.MouseButton.LEFT.asArg()));
    
    driver.perform(Collections.singletonList(tap));
}
```

---

### 2. The Swipe / Scroll
A swipe is a sequence of **Move -> Down -> Move (to new spot) -> Up**. 
* **Swipe Up:** Start at bottom, end at top.
* **Swipe Down:** Start at top, end at bottom.



```java
public void swipe(int startX, int startY, int endX, int endY) {
    PointerInput finger = new PointerInput(PointerInput.Kind.TOUCH, "finger");
    Sequence swipe = new Sequence(finger, 1);

    swipe.addAction(finger.createPointerMove(Duration.ZERO, Origin.viewport(), startX, startY));
    swipe.addAction(finger.createPointerDown(PointerInput.MouseButton.LEFT.asArg()));
    // Duration here controls the speed of the swipe
    swipe.addAction(finger.createPointerMove(Duration.ofMillis(600), Origin.viewport(), endX, endY));
    swipe.addAction(finger.createPointerUp(PointerInput.MouseButton.LEFT.asArg()));

    driver.perform(Collections.singletonList(swipe));
}
```

---

### 3. Zoom In (Pinch Open)
This requires **two fingers** moving simultaneously in opposite directions. You create two sequences and perform them together.



```java
public void zoomIn() {
    PointerInput finger1 = new PointerInput(PointerInput.Kind.TOUCH, "finger1");
    PointerInput finger2 = new PointerInput(PointerInput.Kind.TOUCH, "finger2");
    
    Sequence seq1 = new Sequence(finger1, 1);
    Sequence seq2 = new Sequence(finger2, 1);

    // Finger 1: Move from center to top
    seq1.addAction(finger1.createPointerMove(Duration.ZERO, Origin.viewport(), 500, 500));
    seq1.addAction(finger1.createPointerDown(PointerInput.MouseButton.LEFT.asArg()));
    seq1.addAction(finger1.createPointerMove(Duration.ofMillis(500), Origin.viewport(), 500, 200));
    seq1.addAction(finger1.createPointerUp(PointerInput.MouseButton.LEFT.asArg()));

    // Finger 2: Move from center to bottom
    seq2.addAction(finger2.createPointerMove(Duration.ZERO, Origin.viewport(), 500, 500));
    seq2.addAction(finger2.createPointerDown(PointerInput.MouseButton.LEFT.asArg()));
    seq2.addAction(finger2.createPointerMove(Duration.ofMillis(500), Origin.viewport(), 500, 800));
    seq2.addAction(finger2.createPointerUp(PointerInput.MouseButton.LEFT.asArg()));

    // Perform both sequences at the same time
    driver.perform(Arrays.asList(seq1, seq2));
}
```

---

### 4. Drag and Drop
Similar to a swipe, but often includes a **Pause** after the `PointerDown` to simulate the "grabbing" of an object before moving it.

```java
public void dragAndDrop(Point source, Point target) {
    PointerInput finger = new PointerInput(PointerInput.Kind.TOUCH, "finger");
    Sequence dragDrop = new Sequence(finger, 1);

    dragDrop.addAction(finger.createPointerMove(Duration.ZERO, Origin.viewport(), source.x, source.y));
    dragDrop.addAction(finger.createPointerDown(PointerInput.MouseButton.LEFT.asArg()));
    dragDrop.addAction(new Pause(finger, Duration.ofMillis(500))); // Wait to "pick up"
    dragDrop.addAction(finger.createPointerMove(Duration.ofMillis(700), Origin.viewport(), target.x, target.y));
    dragDrop.addAction(finger.createPointerUp(PointerInput.MouseButton.LEFT.asArg()));

    driver.perform(Collections.singletonList(dragDrop));
}
```


---
config
suite
locators
surefire
maven
pom..xml
failed scenario handling
permission, backgrounnd,network, gestures, bluetooth handling
retry

