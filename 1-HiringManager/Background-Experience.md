# Professional Experience & Background

When I joined my current role, mobile automation was almost non-existent.
I built a mobile automation framework from scratch using Appium with Java for both iOS and Android, enabling full end-to-end user journey testing without manual intervention.


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

I set up nightly test jobs in AWS CodePipeline to make sure these backend tests ran automatically and caught issues before any code got merged. I also worked with AWS infrastructure to create test environments that could handle high API loads, so we could be confident about performance under stress.

Besides writing tests, I took charge of analyzing defects and digging into root causes to prevent the same issues from happening again. Throughout this, I worked closely with engineers, product managers, and support teams to improve our overall test strategy and quality standards.

**Example: Testing a "Buy product" Alexa skill with pre-roll ads**
* EC2 launches AVS SDK session in US region.
* It triggers the skill and verifies ad audio playback before product intent.
* Logs ad metadata and validates ad provider API calls.
* Stores logs in CloudWatch.
* Another EC2 in EU region repeats the test with different ads.

