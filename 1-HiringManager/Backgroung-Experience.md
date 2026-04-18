I've converted your notes and interview preparation material into a clean, structured Markdown format. I’ve organized the content into logical sections for easier reading and professional use.

---

# Professional Experience & Background

## Current Role: Senior SDET (Abbott)
When I joined my current role, mobile automation was almost non-existent. I built a mobile automation framework from scratch using **Appium with WebdriverIO** (JavaScript/TypeScript) and **Appium with Java** for both iOS and Android, enabling full end-to-end user journey testing without manual intervention.

### Key Framework Features:
* **Platform-Specific Locators:** Modularized elements for iOS and Android.
* **Context Hooks:** Automated setup for device and platform context.
* **Mobile Utilities:** Custom support for complex mobile gestures.
* **Advanced Reporting:** Integrated detailed execution logs and visual reports.

### Stability & Observability:
Collaborated with DevOps/SRE teams to integrate:
* **New Relic:** For real-time performance metrics and network monitoring.
* **Sentry:** For mobile crash tracking and proactive issue detection.

### Scaling & Infrastructure:
* **In-House Device Farm:** Built a physical lab with Mac Minis (iOS) and Linux servers (Android).
* **Hybrid Cloud:** Leveraged **BrowserStack** for wider device/OS coverage.
* **CI/CD Integration:** Integrated with **GitHub Actions** and **Docker** to containerize Appium servers, allowing overnight parallel test runs.

---

## Experience at Amazon
Focused on automating UI and API layers for the **Alexa ecosystem** (Internal ad-targeting services).

### Technical Highlights:
* **UI Automation:** Selenium WebDriver with Java.
* **API Automation:** REST Assured and Postman for end-to-end flows (Auth, Data Validation).
* **Cloud Infrastructure:** Used **AWS EC2** instances across regions (US/EU) to verify regional ad-playback and metadata.
* **CI/CD:** Established nightly jobs in **AWS CodePipeline**.
* **Monitoring:** Logged ad metadata and validated provider API calls via **CloudWatch**.

---

# Technical Deep Dive: Observability Tools

| Tool | Purpose | SDET Role |
| :--- | :--- | :--- |
| **New Relic** | APM & Observability | Monitor crash trends, validate API latency post-deployment, and query logs for failed transactions. |
| **Grafana** | Visualization Dashboards | Create health dashboards (CPU/Memory) and set alerts for error spikes or slow endpoints. |
| **Sentry** | Crash Reporting | Capture stack traces for mobile app crashes to link them to specific code changes. |

### Practical Workflow Example:
1.  **Automation Failure:** A regression test fails for a Bluetooth biosensor pairing.
2.  **Log Analysis:** Pull logs from the device farm and match them with a **New Relic** crash event.
3.  **Correlation:** Use **Grafana** to check if a CPU spike or network latency occurred at the same timestamp.
4.  **Resolution:** Isolate and reproduce the bug in the lab.

---

# Interview Preparation: Q&A

### Mobile Automation Strategy
* **Handling Platform Differences:** Abstract platform-specific locators into helper modules while keeping test logic platform-agnostic. Use conditional logic for gestures (swiping/scrolling) that differ between iOS and Android.
* **Ensuring Reliability:** Implement explicit waits, retry logic for flaky elements, and use **Idempotent** test design to ensure tests are independent.

### Infrastructure & CI/CD
* **Why Docker?** It ensures a consistent "clean room" environment. Packaging Node.js, Appium, and dependencies ensures tests run identically on a developer's machine and the CI server.
* **In-House vs. Cloud (BrowserStack):** Use the in-house farm for speed and cost-control on core devices; use BrowserStack for edge-case OS versions and hardware models not available locally.

### API & Microservices
* **UI + API Chaining:** Extend Selenium frameworks with **Axios** (for JS) or **REST Assured** (for Java).
* **State Validation:** After a UI action (e.g., "Place Order"), immediately trigger an API call to validate the backend database state and transaction status.

---

# Test Data & Governance
* **Leadership:** Functioned as **Domain Lead**, overseeing defect triage, root cause analysis (RCA), and implementing quality standards across cross-functional teams.
* **BDD:** Incorporated **Cucumber** to create human-readable test scenarios, reducing ambiguity between product owners and engineering.

---

### Additional Notes:
* **Biosensor Testing:** Specifically handled Bluetooth pairing automation, simulating real hardware sequences for medical sensors.
* **Performance:** Achieved >70% reduction in regression suite time through parallelization and the local device farm.
