# CI
practise of merging the code in shared repoand automatically validating these changes dev pushes code.
ci pipeline runs - install dependency , build the app, run unot and integration testsm pipeline reports success/failurem 

on every PR i run test without blocking the PR , create test report and notify via slack - regression and smoke test, nightly runs

# CD 
continuous delivery or deployment 
ensures code is alwyas in deployable state
if ci pipeline pass -> code is deployed to env -> production deploy happens after manual approval

focusses on delivery and deliver feature faster

# tell me about the release management process an sdet should know - all the steps required for both iOS relaease and android release for mobile app.

As a Senior SDET and Automation Lead, your role in release management goes far beyond just running tests. Especially in environments where software health and data integrity are critical, you act as the gatekeeper between development and production. Mobile releases (iOS and Android) are uniquely challenging because, unlike web deployments, you cannot instantly roll back a broken app once a user installs it from the store.

Here is the end-to-end mobile release management process, broken down by phases, with specific nuances for iOS and Android.

---

### Phase 1: Code Freeze and Branching

The release process begins when development for the upcoming version is structurally complete.

* **Release Branch Cut:** A specific release branch (e.g., `release/v2.4.0`) is cut from the main branch. Only critical bug fixes are merged here; no new features.
* **Version Bumping:** The version numbers are updated.
* **Android:** `versionCode` (an integer that must always increment) and `versionName` (e.g., "2.4.0").
* **iOS:** `CFBundleVersion` (Build Number) and `CFBundleShortVersionString` (Release Version).



### Phase 2: Quality Gates & Regression (The SDET Engine)

This is where your automated framework proves its ROI. Because mobile updates are permanent until the next release, the regression net must be airtight.

* **Automated Sanity & Regression:** Trigger your Appium and TestNG suites via your CI/CD pipeline (like GitHub Actions). This should run across a device farm to ensure compatibility with different OS versions and screen sizes.
* **API Compatibility Testing:** Mobile apps often rely on older API versions. Ensure that any backend API changes are backward-compatible so users who *don't* update the app immediately aren't broken.
* **Exploratory & Edge Case Testing:** Manual testing focuses on things automation might miss: network interruptions (airplane mode), biometric interruptions (FaceID/TouchID), and deep linking.
* **Compliance & Documentation:** In regulated domains, this step includes generating test execution reports, updating the Traceability Matrix, and ensuring all Jira tickets linked to the release have passing QA status.

### Phase 3: Beta Distribution (UAT & Internal Testing)

Before hitting the public stores, the app must be distributed to internal stakeholders, beta testers, or a UAT team.

#### **iOS: TestFlight**

Apple’s ecosystem is highly restricted by certificates and provisioning profiles.

* The CI/CD pipeline builds an `.ipa` file signed with an **Apple Distribution Certificate**.
* The build is uploaded to **App Store Connect**.
* It is distributed via **TestFlight** to internal testers (who need to be added to your App Store Connect team) or external testers (which requires a brief Apple Beta Review).

#### **Android: Google Play Console Tracks**

Android is slightly more flexible but requires strict cryptographic signing.

* The CI/CD pipeline builds an Android App Bundle (`.aab`) or `.apk`, signed with the company's **Keystore**.
* The build is uploaded to the **Google Play Console**.
* It is promoted to the **Internal Testing** track (for QA/Devs) or the **Closed Testing** track (for a wider beta audience).

### Phase 4: App Store Submission & Review

Once QA gives the final sign-off (the "Go/No-Go" meeting), the builds are submitted for formal review by Apple and Google.

| Platform | Artifact | Review Nuances | Typical Review Time |
| --- | --- | --- | --- |
| **Apple (iOS)** | `.ipa` to App Store Connect | Extremely strict. They will reject apps for requesting permissions (like Camera/Location) without a clear business justification in the `Info.plist`, or for broken UI on iPads. | 24 to 48 hours. |
| **Google (Android)** | `.aab` to Google Play Console | Mostly automated scanning for malware and policy violations (e.g., unauthorized use of background location or sensitive user data). | 2 hours to 7 days (varies wildly). |

*SDET Tip:* Always provide Apple and Google with a dedicated "Test Account" and detailed login instructions in the review notes so they can access the app behind the login screen.

### Phase 5: Phased Rollout & Production Monitoring

Never release to 100% of your user base on day one. A staged rollout mitigates risk if a critical crash slipped through.

* **Phased Rollout:** Set the release to go out to 1%, then 10%, 50%, and finally 100% over a span of 7 days.
* **Production Monitoring:** Watch crash analytics (like Firebase Crashlytics or Datadog) closely during the 1% and 10% phases. If a spike in crashes occurs, you can halt the rollout immediately.
* **Post-Release Sanity (Smoke Test):** Once live, run a lightweight automated smoke test against the production environment using the live App Store/Play Store builds to verify that production APIs and databases are responding correctly.

---

### The "Hotfix" Process

If a critical P0 bug is found in production (e.g., users cannot log in), the standard process is bypassed for a **Hotfix**.

1. A branch is cut directly from the live release tag.
2. The fix is applied and specifically targeted regression is run.
3. An expedited review is requested from Apple/Google (Apple has an explicit "Expedited Review" form for critical bugs).

What does the current "Go/No-Go" sign-off process look like for your team? Do you use a specific dashboard or just Jira reports to show stakeholders that your automated gates passed?
