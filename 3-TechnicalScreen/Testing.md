

# Test Automation Reference Guide

**Topics:** Appium, TestNG, Rest Assured, Cucumber, Maven, ExtentReports, Java, Jenkins/GitHub Actions, API, Microservices, XCUITest

---

## 📱 APPIUM: Cross-Platform Test Automation Framework

### Architecture & Core Concepts

Appium operates on a **Client-Server Architecture**.

* **Client-Server Architecture:** Your test script uses the Appium client library to send commands to the Appium server (HTTP server).
* **Desired Capabilities:** The Appium Java client defines these (like device name, platform version, and app path) to tell the server what kind of session to start.
* **Interpretation:** The server interprets these commands and uses platform-specific drivers (e.g., **XCUITest for iOS**, **UIAutomator2 for Android**) to execute actions on devices.

### Why Appium?

* **Bridge Utility:** It acts as a bridge so you don't have to write platform-specific code.
* **WebDriver Protocol:** It uses the standardized, platform-independent W3C WebDriver Protocol.
* **Common Language:** It is the "common language" that automation tools use to tell software to "click this," "type that," or "go to this URL."

### System Requirements

* **Node.js:** Appium is a Node.js application.
* **Android:** Android SDK, Android Studio/emulators, or real devices.
* **iOS:** Xcode, macOS, Simulators/real devices.
* **Appium Client:** A driver/client library specific to your chosen language.

### Ecosystem & Logistics

* **XCUITest Driver:** Apple maintains XCUITest. Appium converts WebDriver protocol commands into XCUITest library calls.
* **HTTP Server:** Appium must run as a process on a computer for the duration of the automation session.
* **Network Flexibility:** The server and client do not need to be on the same computer; they just need to communicate over a network via HTTP requests.

---

## ⚙️ Capabilities & Configuration

### Common Appium Capabilities

| Capability | Type | Description |
| --- | --- | --- |
| `appium:automationName` | String | The name of the Appium driver to use |
| `appium:udid` | String | Unique device identifier of a particular device |
| `appium:app` | String | The path to an installable application |

### Appium Options Object

For cleaner code, combine capabilities into a single `appium:options` block:

```json
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

```

### Executing JavaScript

```java
JavascriptExecutor jsDriver = (JavascriptExecutor) driver;
jsDriver.executeScript("return arguments[0] + arguments[1]", 3, 4);

```

---

## 🚀 How to Use Appium: Step-by-Step

1. **Add Maven Dependencies:** Include the Appium Java Client in your `pom.xml`.
2. **Set Up Desired Capabilities (Options):**
```java
XCUITestOptions options = new XCUITestOptions();
options.setDeviceName("iPhone 15");
options.setApp("/path/to/your/app.ipa");
options.setPlatformVersion("17.0");

```


3. **Initialize the Driver:** Connect to the Appium Server (default: `http://127.0.0.1:4723`).
4. **Write the Test Logic.**

---

## ❓ Frequently Asked Questions (Interview Prep)

* **What are Desired Capabilities?** Key-value pairs sent to the server to define the session (e.g., `platformName`, `deviceName`, `app`).
* **Limitations of Appium?** Complex setup (Node.js, JDK, SDKs) and slightly slower execution compared to purely native frameworks.
* **Android Specifics:** * `appPackage`: Unique identifier for the app (e.g., `com.example.app`).
* `appActivity`: The specific screen/activity to start (e.g., `.MainActivity`).



---

## 🍏 iOS Deep Dive (XCUITest)

### Key Appium Objects for iOS

1. **IOSDriver:** `IOSDriver driver = new IOSDriver(new URL("http://127.0.0.1:4723"), options);`
2. **XCUITestOptions:** Replaces the old `DesiredCapabilities`.
* `setDeviceName("iPhone 15 Pro")`
* `setBundleId("com.apple.example")`
* `setAutomationName("XCUITest")`


3. **AppiumBy:** Use for iOS-specific strategies like `AppiumBy.iOSClassChain("/XCUIElementTypeButton[label == 'Login']")`.
4. **AppiumFieldDecorator:** Used in POM to handle `@iOSXCUITFindBy` annotations.

### Handling System Permissions

> "I utilize `XCUITestOptions` to set `setAutoAcceptAlerts(true)` during initialization. This ensures WDA handles system-level pop-ups like Location or Camera permissions automatically."

---

## 👆 Gestures & Interactions

### W3C Actions API (Vertical Swipe Example)

Appium treats gestures as a series of "events" (Move, Down, Up) performed by a `PointerInput`.

```java
PointerInput finger = new PointerInput(PointerInput.Kind.TOUCH, "finger");
Sequence swipe = new Sequence(finger, 1);

swipe.add(finger.createPointerMove(Duration.ZERO, PointerInput.Origin.viewport(), 500, 800));
swipe.add(finger.createPointerDown(PointerInput.MouseButton.LEFT.asArg()));
swipe.add(finger.createPointerMove(Duration.ofMillis(600), PointerInput.Origin.viewport(), 500, 200));
swipe.add(finger.createPointerUp(PointerInput.MouseButton.LEFT.asArg()));

driver.perform(Collections.singletonList(swipe));

```

### Mobile Command Shortcut (iOS Specific)

Use `mobile: executeScript` for optimized OS gestures:

```java
Map<String, Object> params = new HashMap<>();
params.put("direction", "down");
params.put("elementId", ((RemoteWebElement) element).getId());
driver.executeScript("mobile: swipe", params);

```

*Note: Use W3C for complex patterns, but Mobile Commands for standard, stable OS gestures.*

---

## 🛠️ Troubleshooting Common Issues

1. **WebDriverAgent (WDA) Failures:** * *Issue:* WDA crashes or fails to install due to signing issues.
* *Fix:* Use `setUseNewWDA(true)` or rebuild the WDA project in Xcode.


2. **Element Visibility & Quiescence:**
* *Issue:* XCUITest waits for the app to be "idle." Looping animations can cause hangs.
* *Fix:* Set `setWaitForQuiescence(false)` in `XCUITestOptions`.


3. **Clean State Management:**
* *Fix:* Use `setShouldTerminateApp(true)` to kill zombie processes and `setFullReset(true)` for a 100% clean install.



---

## 🛡️ Stability & Recovery Framework

### 1. Pre-Test: The "Clean Slate"

```java
public void setup() {
    XCUITestOptions options = new XCUITestOptions();
    options.setDeviceName("iPhone 15");
    options.setBundleId("com.apple.example");
    options.setShouldTerminateApp(true); 
    options.setNoReset(false); 
    driver = new IOSDriver(new URL("http://127.0.0.1:4723"), options);
}

```

### 2. During Test: The "State Check"

```java
public void performSafeAction(WebElement element) {
    ApplicationState state = driver.queryAppState("com.apple.example");
    if (state != ApplicationState.RUNNING_IN_FOREGROUND) {
        throw new RuntimeException("App crashed or backgrounded! State: " + state);
    }
    element.click();
}

```

### 3. Post-Failure: "Recovery & Evidence"

* **Screenshot:** `driver.getScreenshotAs(OutputType.FILE);`
* **System Logs:** `driver.manage().logs().get("syslog");`
* **Recovery:** Use `mobile: launchApp` to restart for the next test.

---

## 🌐 Network & Security Handling

### Simulating Network Conditions

* **Airplane Mode:**
```java
Map<String, Object> params = new HashMap<>();
params.put("type", "airplane-mode");
driver.executeScript("mobile: setNetworkConnection", params);

```


* **Latency/Slow Internet:** Use `options.setCapability("appium:networkCondition", "3G");`.

### SSL & Security

* **Insecure Certs:** `options.setAcceptInsecureCerts(true);`
* **Biometrics (FaceID/TouchID):**
```java
options.setCapability("appium:allowTouchIdEnroll", true);
// Trigger success
Map<String, Object> params = new HashMap<>();
params.put("type", "faceId");
params.put("match", true);
driver.executeScript("mobile: sendBiometricMatch", params);

```



---

## 🔗 Resources

* **Appium Interview Questions:** [TestMu Learning Hub](https://www.testmuai.com/learning-hub/appium-interview-questions/)

---

**Next Section:** TESTNG




