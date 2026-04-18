# Professional Experience & Background

When I joined my current role, mobile automation was almost non-existent. I built a mobile automation framework from scratch using Appium with WebdriverIO for both iOS and Android, enabling full end-to-end user journey testing without manual intervention.

When I joined my current role, mobile automation was almost non-existent. I built a mobile automation framework from scratch using Appium with Java for both iOS and Android, enabling full end-to-end user journey testing without manual intervention.

### Key Features
* Platform specific locators
* Hooks to set device and platform context
* Mobile utility to support various gestures
* Advance reporting

### Stability & Observability
To enhance stability and observability, I collaborated closely with DevOps and SRE teams, integrating New Relic for performance metrics and network call monitoring, and Sentry for crash tracking. This improved device debugging and allowed proactive issue detection before releases.

### Scaling the Solution
I set up an in-house mobile device farm, integrated it with CI/CD pipelines using GitHub Actions, Docker, and BrowserStack. Docker was used to containerize the Appium server and test dependencies for consistent execution across environments, while BrowserStack was leveraged for running tests on a wider range of real devices and OS/browser combinations not available in-house. This allowed overnight test runs on multiple devices, giving developers actionable feedback before the next day’s work started.

### API Testing
On the API side, I automated REST API tests covering customer data, subscriptions, and checkout flows. I used Postman for exploratory validation and added Java-based REST Assured tests for regression suites.

### Web Automation
For web testing, I wrote JavaScript-based Selenium tests using the Page Object Model. I also integrated Axios for making backend API calls and AJV for validating JSON schemas. For example, after a key UI action like placing an order, the automation framework would immediately query backend APIs to validate transaction data structure and status.

### Leadership & Quality Governance
Functioned as the Domain Lead, coordinating with developers, QA, and product teams to oversee defect triage and facilitate root cause analysis, implementing quality standards across teams.

---

# Experience at Amazon

At Amazon, I worked on automating UI tests using Selenium WebDriver with Java. On the API side, I automated end-to-end flows for internal ad-targeting services in the Alexa ecosystem, using REST Assured and Postman to cover everything from authentication to data validations and error handling.

**Example: Testing a "Buy product" Alexa skill with pre-roll ads**
* EC2 launches AVS SDK session in US region.
* It triggers the skill and verifies ad audio playback before product intent.
* Logs ad metadata and validates ad provider API calls.
* Stores logs in CloudWatch.
* Another EC2 in EU region repeats the test with different ads.

I set up nightly test jobs in AWS CodePipeline to make sure these backend tests ran automatically and caught issues before any code got merged. I also worked with AWS infrastructure to create test environments that could handle high API loads, so we could be confident about performance under stress.

Besides writing tests, I took charge of analyzing defects and digging into root causes to prevent the same issues from happening again. Throughout this, I worked closely with engineers, product managers, and support teams to improve our overall test strategy and quality standards.

---

# In-House Device Farm with Biosensor Testing (Abbott)

At Abbott, I designed and implemented an in-house mobile device farm to support automation testing for both iOS and Android applications.

### Setup & Implementation
I started by procuring a diverse set of physical devices (different models and OS versions) and mounted them in racks connected via high-capacity USB hubs to dedicated Mac Minis (for iOS) and Linux servers (for Android). For Android, I configured ADB over TCP/IP, and for iOS, I set up WebDriverAgent on each device to enable remote Appium sessions.

The device farm was integrated with Appium Grid so automation tests — written in WebdriverIO + Appium — could be triggered in parallel from our CI/CD pipeline. This cut our mobile regression suite time by over 70%.

### Biosensor Integration
Since our application connected to Bluetooth-enabled biosensors (e.g., glucose monitors, heart rate trackers), I ensured the farm supported real hardware pairing during tests. I implemented automation hooks to:
* Enable Bluetooth automatically before tests.
* Simulate pairing/unpairing sequences with biosensors.
* Validate real-time data sync from the sensor to the mobile app.

This setup allowed QA engineers across teams to run end-to-end sensor-to-app scenarios on real devices without physically being in the lab. The farm also captured device logs, biosensor event logs, and network traces, which made debugging faster and more reliable.

---

# Technical Deep Dive: Observability Tools

## 1. New Relic
**Purpose:** Application performance monitoring (APM) and observability tool.

**What it does:**
* Monitors application performance in real time.
* Captures transaction traces, error rates, API response times, and crash logs.
* Collects frontend (mobile/web) and backend metrics in one place.

**SDET’s role:**
* Validate that APIs, microservices, and mobile apps are not showing increased errors or latency after deployments.
* Monitor crash trends in production/staging to catch regressions early.
* Use New Relic Insights/Queries to pull detailed logs for failed transactions and debug root causes.

## 2. Grafana
**Purpose:** Visualization and dashboard tool that works with various data sources.

**What it does:**
* Displays metrics, logs, and traces in custom dashboards.
* Helps track system health, error spikes, API latency, and resource usage.

**SDET’s role:**
* Create dashboards that show test environment health (CPU, memory, API failure rates).
* Set alerts for error spikes, slow endpoints, or unusual traffic patterns.
* Correlate test failures with infrastructure or performance issues.

## 3. SDET Practical Application

### A. Monitoring Crashes
* **New Relic Mobile APM:** Shows crash rates by device model, OS version, or app version and provides stack traces.
* **Action:** After deployment, monitor for spikes. Pull stack traces to link to code changes.

### B. Detailed Log Analysis
* **New Relic Logs:** Filter for specific error codes or user flows.
* **Grafana Loki:** Aggregate logs from multiple sources for pattern recognition.
* **Action:** Match CI automation failures with logs in New Relic/Grafana to find root causes faster.

### C. Device Debugging
* **Workflow:** Automation detects a failure (e.g., Bluetooth pairing) -> Pull logs from device farm -> Match with New Relic crash event -> Check Grafana for CPU/Network spikes -> Isolate/Reproduce.

---

# Interview Preparation: Q&A

## Mobile Automation Framework

**1️⃣ Q: Can you walk me through how you designed and built the mobile automation framework from scratch?**
A: I started by assessing the app requirements on both iOS and Android. I chose Appium with WebdriverIO for JavaScript/TypeScript support and parallel execution. I used a Page Object Model, reusable utility functions, and integrated device-specific capabilities (XCUITest for iOS, UIAutomator2 for Android). I also incorporated custom commands for gestures and asynchronous waits.

**2️⃣ Q: How did you manage parallel execution and device management?**
A: I integrated the framework with Appium Grid and a local device farm. Device capabilities were managed via configuration files and environment variables. We leveraged WebdriverIO’s native parallel runner to trigger tests across multiple Appium nodes.

**3️⃣ Q: How did you handle differences between iOS and Android?**
A: I abstracted platform-specific locators and gestures into separate helper modules while keeping test logic platform-agnostic. I used conditional logic inside reusable commands to handle platform nuances.

**4️⃣ Q: What challenges did you face, and how did you overcome them?**
A: Challenges included flaky tests and asynchronous behavior. I implemented explicit waits and retry logic. For device management, I scripted health checks and automatic recovery for disconnected devices.

**5️⃣ Q: How did you integrate this framework into your CI/CD pipeline?**
A: Used GitHub Actions for PR and nightly triggers. The pipeline spins up Appium servers and connects to the device farm or BrowserStack. Results/logs are uploaded to JIRA and Slack. Docker containers ensure consistency.

**6️⃣ Q: How did you ensure tests were reliable and maintainable?**
A: Enforced Page Object Model and centralized locators. Used Cucumber with BDD for collaboration. Conducted regular code reviews and monitored flakiness reports.

## Device Farm & Infrastructure

**1️⃣ Q: Can you explain what your in-house device farm setup looked like?**
A: Real iOS/Android devices connected via USB hubs to Mac/Linux servers. Configured with WebDriverAgent (iOS) and ADB (Android). Managed health through scripts and OpenSTF.

**2️⃣ Q: How did you integrate the device farm with CI/CD?**
A: Integrated into GitHub Actions workflows where jobs spin up Docker containers containing the test environment. Results and screenshots were automatically uploaded to GitHub and JIRA.

**3️⃣ Q: Why did you use Docker?**
A: Docker ensured consistent environments across local, staging, and CI. It packaged all dependencies (Node.js, Appium, etc.) so tests ran identically regardless of the host OS.

**4️⃣ Q: How did BrowserStack fit into your strategy?**
A: BrowserStack provided access to a wider variety of device models/OS versions. Our CI pipeline routed critical suites to the in-house farm for speed and cross-device compatibility tests to BrowserStack.

## API Automation

**1️⃣ Q: Describe your approach to automating API testing for microservices?**
A: Built reusable suites using REST Assured and Postman covering customer data, subscriptions, and checkout. Integrated into CI for rapid feedback on backend stability.

**2️⃣ Q: How did you extend your Selenium + TypeScript framework for API testing?**
A: Built a reusable API client module using Axios and AJV for schema validation. This allowed us to chain UI interactions with backend validations.

**3️⃣ Q: Why validate backend API state immediately after UI actions?**
A: It ensures data consistency and uncovers issues UI tests miss, like partial data commits or masked errors.

**4️⃣ Q: How did you handle flaky or transient API failures?**
A: Implemented retry logic in the API client and ensured logs were detailed enough to distinguish between true failures and transient glitches.


**4️⃣ Q: How did your API automation contribute to the overall release cycle?
A:
 By automating API tests that cover core microservices, we caught critical backend regressions early—often before UI tests ran—helping reduce bugs in production. The automation suite was part of our CI/CD pipeline, giving developers fast, reliable feedback and accelerating the release cadence without sacrificing quality.

**4️⃣ Q:  Can you walk me through how you automated the API tests for the Alexa ad-targeting services?
A:
 I started by understanding the critical API workflows, such as authentication, data retrieval, and error scenarios. Using REST Assured and Postman, I created modular, reusable tests that validated not only response status codes but also the schema and business logic correctness. These tests were integrated into our CI/CD pipeline with AWS CodePipeline to run nightly, providing quick feedback on backend stability.

**4️⃣ Q: How did you ensure the reliability of your automated tests, especially for APIs under heavy load?
A:
 I collaborated with the infrastructure team to provision AWS environments that could simulate high API load scenarios. I incorporated retries and timeout handling in the test scripts to handle transient failures gracefully. Additionally, I monitored test flakiness and regularly reviewed logs to quickly identify and fix intermittent issues.

**4️⃣ Q:  What kind of defect analysis and root cause investigation did you perform?
A:
 Whenever a test failed, I dug into logs from both the API and UI layers to understand if the failure was due to flaky tests, data issues, or genuine bugs. I worked closely with developers to trace defects back to their source, often identifying upstream problems like race conditions or data inconsistencies, which helped us improve both the test suite and the product.

**4️⃣ Q: How did you collaborate with cross-functional teams in your testing efforts?
A:
 I regularly synced with engineers and product managers to clarify requirements and ensure test coverage aligned with business priorities. I also coordinated with customer support to understand real-world issues and prioritize test scenarios accordingly. This collaboration helped build a more effective, well-rounded automation strategy.
