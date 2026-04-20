

# 🧠 Core Categories to Prepare

```text id="2a9og3"
1. Framework Design
2. Page Object / Patterns
3. Stability / Flakiness
4. Parallel Execution
5. Mobile Automation
6. CI/CD
7. Test Data
8. Debugging
9. Coding Utilities
10. Tradeoffs / Decisions
```

---

# 🔥 1. Framework Design Questions (Highest Probability)

### Prepare for:

---
1. How would you design a scalable UI automation framework? What layers would you include?

Feature Layer (BDD): Plain English .feature files (Gherkin) that define the "What."  
Step Definition Layer: The "Glue" code that maps Gherkin steps to Java methods.  
Page Object Model (POM) Layer: Encapsulated page classes that represent the "How" (UI interactions).  
Utility/Core Layer: The "Engine" (DriverFactory, ConfigManager, Wait Utils).  

Core Components for Scalability  
A. The Driver Factory (Creational Pattern)    
To scale across multiple devices or platforms (Android/iOS), use a Factory Pattern combined with ThreadLocal. This allows you to run tests in parallel without "thread collision" (where one test's driver interferes with another).    
B. Externalized Locators (JSON/Properties)    
Storing locators inside Java classes makes maintenance a nightmare as the app grows.
Strategy: Move locators to a locators.json file.  
Benefit: You can update an ID for both Android and iOS in one file without recompiling the entire project.  
C. Environment Configuration  
Never hardcode URLs, device names, or app paths. Use a ConfigManager to load properties based on the environment on the basis of runtime value.  
D. Parallel Execution (TestNG)  
Parallelism is the only way to scale execution time. Use the TestNG XML to manage suites and set parallel="methods" or parallel="tests". This works seamlessly with the ThreadLocal driver we designed.  
e. Advanced Reporting (ExtentReports)  
A scalable framework needs clear feedback. ExtentReports provides a dashboard that includes:  
Visual pass/fail pie charts.  
Screenshot attachments for failed steps automatically handled in Hooks.  
Step-by-step logs for debugging  
  

---
3. How do you separate test logic from locators?

Externalizing Locators (JSON or Properties)
The "Gold Standard" for scalability is moving locators completely out of Java code and into external configuration files.

A. The Locator File (locators.json)
By using a JSON file, you can store Android and iOS locators side-by-side. This allows you to update a button's ID without recompiling your Java project.

```JSON
{
  "LoginPage": {
    "android": {
      "loginBtn": "id:com.lingo.app:id/btn_login"
    },
    "ios": {
      "loginBtn": "accessibility:login_button"
    }
  }
}
```
B. The Locator Factory (Utility)
You create a utility class that reads the JSON based on the current platform and returns a By object.

```Java
public class LocatorUtils {
    public static By getBy(String pageName, String elementKey) {
        String locatorRaw = JsonReader.getValue(pageName, elementKey); // e.g., "id:btn_login"
        String type = locatorRaw.split(":")[0];
        String value = locatorRaw.split(":")[1];

        return switch (type.toLowerCase()) {
            case "id" -> By.id(value);
            case "accessibility" -> AppiumBy.accessibilityId(value);
            case "xpath" -> By.xpath(value);
            default -> throw new IllegalArgumentException("Invalid locator type");
        };
    }
}
```
---
4. How do you structure reusable components?

Structuring reusable components in a scalable UI framework requires moving beyond simple "helper" methods to a **modular, inheritance-based design**. The goal is to ensure that code for common actions—like swiping, waiting, or handling alerts—is written **once** and inherited by every Page Object.


### 1. The BasePage (The "Parent" Component)
Every Page Object class should inherit from a `BasePage`. This is where you house the "low-level" reusable methods that use the **Actions API (W3C)** and **Fluent Waits**.

```java
public abstract class BasePage {
    protected AppiumDriver driver;
    protected WebDriverWait wait;

    public BasePage(AppiumDriver driver) {
        this.driver = driver;
        this.wait = new WebDriverWait(driver, Duration.ofSeconds(10));
        PageFactory.initElements(new AppiumFieldDecorator(driver), this);
    }

    // Reusable Component: Explicit Wait + Click
    protected void click(WebElement element) {
        wait.until(ExpectedConditions.elementToBeClickable(element)).click();
    }

    // Reusable Component: W3C Swipe Gesture
    protected void swipeUp() {
        // Implementation of W3C Actions logic...
    }
}
```


### 2. Composition via "Component" Classes
For UI elements that appear on multiple screens (like a **Navigation Bar**, **Side Menu**, or **Alert Pop-up**), don't duplicate the code in every Page Object. Instead, create a separate component class and "compose" it.



**NavigationComponent.java**
```java
public class NavigationComponent extends BasePage {
    @AndroidFindBy(id = "nav_home")
    private WebElement homeTab;

    public NavigationComponent(AppiumDriver driver) { super(driver); }

    public void goToHome() { click(homeTab); }
}
```

**Using it in a Page Class:**
```java
public class DashboardPage extends BasePage {
    // Instead of re-writing Nav logic, we include the component
    public NavigationComponent nav = new NavigationComponent(driver);

    public void checkStats() {
        // Dashboard specific logic...
    }
}
```



### 3. Utility Wrappers (Static Components)
If a component doesn't need a driver instance to function (like **Date Formatters**, **String Parsers**, or **JSON Readers**), structure them as **Static Utility Classes**. 

* **Example:** `DateUtils.getNextMonthDate()` or `StringUtils.extractOrderNumber(text)`.



### 4. Summary of Structure

| Component Level | Logic Contained | Implementation |
| :--- | :--- | :--- |
| **Base Level** | Common UI actions (Wait, Click, Swipe). | **Inheritance** (`BasePage`) |
| **UI Fragments** | Shared UI elements (Nav Bar, Search Bar). | **Composition** (Include class in Page) |
| **Logic Helpers** | Data manipulation, Math, File I/O. | **Static Utilities** (`FileUtils`, `LogUtil`) |
| **Driver Level** | Session setup, Capability management. | **Factory Pattern** (`DriverFactory`) |


---
5. How do you support multiple applications/modules in one framework?

Multi-Module Project Structure (Maven)
Use a parent-child relationship where the core logic resides in a "Core" module, and each app has its own dedicated module.

automation-core: Contains DriverFactory, BasePage, JSON Utils, and Reporting.

app-lingo-user: Contains Lingo-specific Page Objects and Features.

app-lingo-partner: Contains Partner-specific Page Objects and Features.

---
6. How do you onboard new tests quickly?
Quick onboarding is about reducing boilerplate code so that a tester only has to write the "What" (Gherkin) and the "How" (Page Actions).

A. Use a Custom Project Template  
Maven Archetype or AI 

B. Library of Reusable "Keywords"  
In your Step Definition layer, create highly generic steps that can be reused across different features:  
When I click on the element "loginButton" (where the string maps to a JSON locator).  

c. Automated Locator Generation  
Integrate a utility or use a naming convention that allows testers to find locators easily.  

D. Detailed "Definition of Done" for Tests
Provide a README.md or a "Cheat Sheet" in your SDET repository that explains:  
How to add a locator to the JSON.  
How to create a new Page Object (inheriting from BasePage).  

---

# 🔥 2. Page Object / Design Pattern Questions

### 1. What is Page Object Model (POM)?
**Page Object Model** is a design pattern that creates an object repository for storage of all UI elements. It encourages the separation of **Test Logic** from **UI Locators**.
* **Structure:** Each web page (or mobile screen) is represented by a corresponding Java class.
* **Key Benefit:** If the UI changes (e.g., a button ID changes), you only update the locator in one place (the Page Class), and all tests using that button are automatically fixed.



---

### 2. Drawbacks of POM?
While POM is the industry standard, it does have limitations:
* **High Initial Investment:** It takes more time to set up the folder structure and classes initially compared to a linear script.
* **Maintenance Overhead:** In very large projects, managing hundreds of page classes can become complex.
* **Redundancy:** If not designed carefully (e.g., without a `BasePage`), you may end up duplicating common methods across many classes.

---

### 3. Page Factory vs. POM?
These are often confused. **POM is the pattern**, while **Page Factory is a specific implementation** provided by Selenium/Appium.

| Feature | Page Object Model (POM) | Page Factory |
| :--- | :--- | :--- |
| **Type** | Architectural Pattern. | Built-in Library/Extension. |
| **Locators** | Uses `By` objects (e.g., `By.id("...")`). | Uses Annotations (e.g., `@FindBy`). |
| **Initialization** | Manual (e.g., `new LoginPage(driver)`). | Uses `PageFactory.initElements()`. |
| **Lazy Loading** | Not inherent; found when called. | Elements are "Lazy Initialized" when accessed. |

---

### 4. What is Screen Object Model (SOM) for mobile?
**Screen Object Model** is essentially the mobile version of POM. 
* **The Concept:** Since mobile apps have "Screens" rather than "Pages," the terminology changes.
* **Platform Specifics:** It often involves handling **Android** and **iOS** versions of the same screen within one class using Appium’s `@AndroidFindBy` and `@iOSXCUITFindBy` annotations.

---

### 5. When would you use Component Object Model (COM)?
You use the **Component Object Model** (or "Fragment Pattern") when you have **reusable UI pieces** that appear on multiple screens.
* **Scenario:** A "Side Navigation Menu" or a "Bottom Toolbar" that exists on every screen of your app.
* **Implementation:** Instead of copying the Nav Menu code into every Page/Screen Class, you create one `NavComponent` class and **compose** it into your other page classes.



---

### 6. How do you handle common widgets (header, date picker)?
Common widgets are best handled using **Composition** or **Static Utility Wrappers**:

* **Header/Footer:** Treat these as **Components** (see COM above). Since they are present on almost every page, you can initialize them in your `BasePage` so they are inherited by all other pages.
* **Complex Widgets (Date Picker/Select):** Since these involve multiple clicks (open, select month, click date), wrap the entire sequence into a single method within a **Widget Utility Class**.
  * *Example:* `DatePickerUtil.selectDate(driver, "2025-12-25")`.
* **Custom Elements:** For specialized widgets (like a custom slider), create a dedicated class that accepts a `WebElement` and provides methods like `.getValue()` or `.setValue()`.


---

# 🔥 3. Flaky Test / Reliability Questions (Very Likely)


### 1. Why do UI tests become flaky?
Flakiness occurs when a test fails intermittently without any change in the code. Common causes include:
* **Race Conditions:** The script tries to interact with an element before the app has finished loading it.
* **Environment Instability:** Fluctuations in network speed, low device memory, or background system pop-ups (e.g., low battery, system updates).
* **Dynamic Content:** Elements with dynamic IDs or data that changes every time the app loads.
* **Resource Leaks:** Failing to quit the driver correctly, leading to "Session Not Created" errors in subsequent runs.
developer did some change or frequently evolving app


### 2. How do you reduce flakiness?
* **Implement Smart Waits:** Avoid `Thread.sleep()`; use **ExpectedConditions** (Explicit/Fluent waits).
* **Atomic Tests:** Keep tests short and focused on one specific goal.
* **Independent Tests:** Ensure every test starts with a fresh app state (Reset/Clear Cache) so a failure in Test A doesn't break Test B.
* **Stable Locators:** Use **Accessibility IDs** instead of fragile XPaths.

locator startegy , using device pool, health scripts , thread safe objects



### 3. Hard Wait vs. Explicit Wait?
| Type | Mechanism | Impact |
| :--- | :--- | :--- |
| **Hard Wait (`Thread.sleep`)** | Stops the entire thread for a fixed time. | Slows down execution; doesn't care if the element is ready. |
| **Explicit Wait** | Polls the DOM at set intervals (e.g., every 500ms). | Continues as soon as the condition is met; much faster. |

```
wait.until(ExpectedConditions.visibilityOfElementLocated(locator)).getText();
elementToBeClickable
alertIsPresent
```
Fluent wait
```
Wait<AppiumDriver> fluentWait = new FluentWait<>(driver)
    .withTimeout(Duration.ofSeconds(30))
    .pollingEvery(Duration.ofMillis(500)) // Check every half-second
    .ignoring(NoSuchElementException.class)
    .ignoring(StaleElementReferenceException.class);

WebElement element = fluentWait.until(d -> d.findElement(By.id("dynamic_id")));
```


### 4. Why do tests pass local but fail CI?
This is a classic SDET challenge. The differences usually boil down to:
* **Resources:** CI servers (like GitHub Actions or Jenkins) often have less CPU/RAM than your local MacBook, leading to slower app rendering.
* **Headless vs. Headed:** CI often runs in "Headless" mode or on remote device farms (BrowserStack/SauceLabs) where latency is higher.
* **State:** Local runs often have "leftover" data or logged-in sessions that make tests pass, whereas CI starts in a clean, empty environment.
* **Network:** CI servers may have firewall restrictions or different DNS settings than your local machine.



### 5. How do you stabilize Appium tests?
Stabilizing Appium requires platform-specific tuning:
* **Auto-Grant Permissions:** Use capabilities like `autoGrantPermissions: true` to avoid unexpected system alerts.
* **No Reset:** Use `noReset: true` if you want to keep the app state, or `fullReset: true` for a clean slate.
* **Appium Settings:** Use `driver.setSetting(Setting.WAIT_FOR_IDLE_TIMEOUT, 0)` for apps with constant background animations that prevent Appium from interacting.
* **New Session Strategy:** Ensure the `DriverFactory` creates a completely fresh session for each test class.



### 6. How do you retry safely?
Retrying is a "band-aid," but if used, it must be done carefully:
* **TestNG IRetryAnalyzer:** Implement this to retry only **failed** tests, not the whole suite.
* **Limit Retries:** Set a maximum of 1 or 2 retries. If it fails 3 times, it is a genuine bug, not flakiness.
* **Cleanup Before Retry:** Ensure the `@AfterMethod` or `Hooks` still run to clean the environment/reset the app before the retry starts; otherwise, the second attempt will fail for the same reason as the first.


---

# 🔥 4. Parallel Execution / Scale Questions

### 1\. How do you run tests in parallel?

Test NG - parallel = methods (parallel = tests)
Ci cd - matrix startegy for platform and devices

1. parallel="tests" 
This setting targets the <test> tags within your testng.xml file. 
Scope: Each <test> tag runs in its own separate thread.
Internal Execution: All classes and methods inside a single <test> tag will execute sequentially in the same thread.
Best Use Case: Ideal for running different functional modules or cross-browser tests (e.g., one <test> for Chrome and another for Firefox) simultaneously.
2. parallel="methods"
This setting targets individual @Test annotated methods. 
Scope: Every test method is executed in its own separate thread, regardless of which class or <test> tag it belongs to.
Concurrency Level: Provides the highest degree of parallelism and can significantly reduce execution time for large suites.
Best Use Case: Best for independent, atomic test cases that do not share any state or data.
-----

### 2\. ThreadLocal Driver: Why?

The `ThreadLocal` class provides thread-local variables. Each thread that accesses a `ThreadLocal` variable (via `.get()` or `.set()`) has its own, independently initialized copy of the variable.

**Why it's mandatory for Appium:**

  * **Isolation:** Without `ThreadLocal`, if Test A and Test B run at the same time, they might both try to use the same `driver` instance. Test A might click a button while Test B is trying to enter text, leading to a crash.
  * **Safety:** It ensures that the `driver.quit()` command in Test A doesn't accidentally kill the session for Test B.

-----

### 3\. How do you prevent shared state issues?

Shared state (where one test depends on data from another) is the "death of parallelism." To prevent this:

  * **Atomic Tests:** Design tests to be small and independent.
  * **Clean Start:** Use `@BeforeMethod` to reset the app or clear the cache so every test starts from the same splash screen.
  * **Unique Data:** Never use the same user account for parallel tests. Use a **Data Provider** or a pool of accounts (e.g., `user_01`, `user_02`) to ensure no two threads are trying to modify the same profile simultaneously.

-----

### 4\. How many parallel tests should run?

There is no "magic number," but it is limited by two main factors:

  * **Hardware Resources:** Each Android Emulator or iOS Simulator consumes significant RAM and CPU. Typically, a standard developer laptop can handle **2 to 4** parallel local emulators.
  * **Appium Server:** A single Appium server can technically handle multiple sessions, but it is more stable to run one server per device or use a **Device Farm** (like BrowserStack) which scales to hundreds.

-----

### 5\. How do you scale device execution?

To scale beyond a few local devices, you move to the cloud:

  * **Cloud Device Farms:** Use services like **BrowserStack, SauceLabs, or AWS Device Farm**. You simply point your `DriverFactory` to their remote URL.
  * **Docker-Android:** Use Docker containers to spin up Android emulators on a Linux server.
  * **Selenium Grid 4:** Appium 2.0 integrates well with Selenium Grid, allowing you to distribute mobile tests across a network of different machines (Nodes).
device farm
-----

| Concept | Professional Implementation |
| :--- | :--- |
| **Orchestration** | **TestNG XML** with `parallel="tests"`. |
| **Concurrency** | **ThreadLocal\<AppiumDriver\>** in the DriverFactory. |
| **Data Safety** | **Account Pooling** (one unique user per thread). |
| **Scale** | **Cloud Providers** to remove local hardware bottlenecks. |



```java id="ozg3pt"
ThreadLocal<AppiumDriver>
```

---

# 🔥 5. Mobile Automation Questions 

### 1. Appium Architecture?
Appium follows a **Client-Server Architecture** based on the W3C WebDriver protocol.
* **Appium Client:** The automation code you write in Java. It sends HTTP requests (commands) to the server.
* **Appium Server:** A Node.js server that receives commands and routes them to the correct device.
* **Drivers:** The server uses platform-specific drivers to communicate with the device's native automation framework:
    * **Android:** Uses the `UiAutomator2` driver.
    * **iOS:** Uses the `XCUITest` driver.
* **Bootstrap/Appium Settings:** A small app installed on the device that executes the commands and returns the result to the server.



### 2. Appium vs. Espresso vs. XCUITest?
| Feature | Appium | Espresso (Android) | XCUITest (iOS) |
| :--- | :--- | :--- | :--- |
| **Language** | Multi-language (Java, Python, etc.) | Java / Kotlin | Swift / Objective-C |
| **Platform** | Cross-platform (iOS & Android) | Android Only | iOS Only |
| **Access** | Black-box (External) | White-box (Internal) | White-box (Internal) |
| **Speed** | Slower (Server overhead) | Extremely Fast | Extremely Fast |
| **Usage** | Functional / E2E Testing | Unit / Component Testing | Unit / Component Testing |


### 3. Best Locator Strategy on iOS?
The hierarchy of stability for iOS is:
1.  **Accessibility ID:** The gold standard. Fast and platform-independent.
2.  **iOS Class Chain:** A faster alternative to XPath that uses hierarchical queries.
3.  **iOS Predicate String:** Very powerful; allows complex filtering (e.g., `name BEGINSWITH 'Login'`).
4.  **XPath:** **Avoid on iOS.** It is notoriously slow because XCUITest must crawl the entire XML tree.


### 4. How do you automate gestures?
You should use the **W3C Actions API** (PointerInput and Sequence) for complex gestures like swiping, pinching, or long-pressing.
* **Process:** Define a `PointerInput` (finger), create a `Sequence`, add actions (`pointerMove`, `pointerDown`, `pointerUp`), and execute via `driver.perform()`.
* **Alternative:** For simpler gestures, you can use `mobile: swipe` or `mobile: scroll` (native commands), which are occasionally more stable but platform-specific.


### 5. How do you test Push Notifications?
Testing push notifications is tricky because they are system-level events outside the app.
* **Android:** Open the notification shade using `driver.openNotifications()`, find the notification element, and click it.
* **iOS:** Real push notifications require a physical device. You can simulate them by:
    1.  Pushing a payload via the `xcrun simctl push` command in the terminal.
    2.  Using the `mobile: terminateApp` and `mobile: launchApp` flow to trigger "Background" notifications.
    3.  Automating the interaction with the notification banner using the `SpringBoard` (iOS System) bundle ID.



### 6. How do you handle permissions popups?
There are two professional ways to handle "Allow Location/Camera" popups:
* **Capability Level (The Clean Way):** Set `autoGrantPermissions: true` in your `UiAutomator2Options` (Android). This grants all permissions when the app installs.
* **Logic Level (The Interactive Way):** Write a global handler in your `Hooks` or `BasePage` that looks for the "Allow" button and clicks it if it appears.
    * *Tip:* On iOS, use the `mobile: alert` command to accept or dismiss alerts without finding elements.



### 7. How do you run multiple devices?
To run on multiple devices simultaneously, you must ensure **Port Isolation**:
1.  **Unique System Port:** Each Android session needs a unique `systemPort`.
2.  **Unique WDA Port:** Each iOS session needs a unique `wdaLocalPort`.
3.  **Driver Instances:** Use **ThreadLocal** in your `DriverFactory` to ensure each thread manages its own device session.
4.  **Configuration:** Use a `testng.xml` file with different `<test>` blocks, passing the device name and port as parameters to the code.


---

# 🔥 6. CI/CD Questions

1. How do you integrate framework with Jenkins/GitHub Actions?
2. Smoke vs regression strategy?
3. What runs on PR merge?
4. How do you publish reports?
5. How do you reduce execution time?

---

# 🔥 7. Test Data Questions


1. How do you manage test accounts?
2. Dynamic data vs static data?
3. How do parallel tests avoid collisions?
4. Where do you store data (JSON/XML/DB)?
5. How do you clean test data?

---

# 🔥 8. Debugging Questions

### Prepare:

1. Test failing intermittently—how investigate?
2. Element not found though visible?
3. Works local, fails CI?
4. App crashes mid-test?
5. Network issue impacts test?

### Strong framework answer:

logs + screenshot + video + device logs + backend logs.

---

# 🔥 9. Coding Questions Related to Framework

You may be asked to code small utilities.

### Examples:

1. Write reusable wait method
2. Retry click method
3. Read config from properties
4. Sort list validation
5. Duplicate detection in UI table
6. Generic dropdown selector

---

# 🔥 10. Tradeoff / Senior-Level Questions

### Prepare:

1. What should NOT be automated in UI?
2. UI vs API automation balance?
3. How many E2E tests should exist?
4. Would you use BDD? Why / why not?
5. When rewrite framework vs refactor?

These questions separate senior candidates.

---

# 🎯 Questions Most Likely for YOU (Based on Resume)

## Since you migrated Selenium → Playwright:

1. Why move to Playwright?
2. Selenium vs Playwright?
3. What improved?

## Since you built device farm:

4. How did you manage devices?
5. How parallelized tests?

## Since you reduced flakiness:

6. What exact changes reduced 35% flakiness?

---

# 💡 Strong Answers Apple Likes

## UI Pyramid Thinking

> “I keep UI automation focused on critical journeys and push most validation to API/service layers.”

## Ownership Thinking

> “I don’t just fix tests—I fix root causes.”

## Scalability Thinking

> “Framework should make writing new tests faster.”

---

# 🚀 20 Most Important Questions to Practice Verbatim

1. Design scalable UI framework
2. Why POM?
3. POM drawbacks?
4. Reduce flaky tests?
5. Wait strategies?
6. Selenium vs Playwright
7. Appium vs XCUITest
8. Best locators mobile
9. Parallel execution design
10. ThreadLocal driver
11. CI/CD integration
12. Smoke vs regression
13. Test data strategy
14. Debug failing CI tests
15. API + UI hybrid testing
16. What belongs in UI tests?
17. How test sync interruption?
18. How run on multiple devices?
19. How report failures?
20. Example framework you built

---
Difficult things you automated and how ?

Strategy for test automation ?

challenges in test automation?
