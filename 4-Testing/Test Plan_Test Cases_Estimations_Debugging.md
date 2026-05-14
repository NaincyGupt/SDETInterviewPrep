# Generic Test plan

A test plan defines your testing team’s strategy, goals, scope, and execution approach to help ensure software is tested thoroughly and consistently before release. It gives stakeholders a shared view of what will be tested, how testing will be done, and what needs to happen before a release can move forward.

## How to create a test plan
Follow these six steps to create an efficient test plan:

    Define the release scope
    Schedule timelines 
    Define test objectives
    Determine test deliverables
    Design the test strategy
    Plan test environment and test data

    1. Define the release scope
    Examples of questions to ask when defining the release scope include: 

    Are there new features being released in this version? 
    What are the risk areas?
    Are there any particularly sticky areas where you’ve seen regressions in the past?
    What type of release is it? Is this a maintenance release that includes bug fixes? Is this a minor feature release? Is this a major feature release?

    2. Schedule timelines 

    Set a clear timeline for testing based on your release deadlines. 
    Confirm the release timeline with your project manager: Make sure you understand key dates, dependencies, and any non-negotiable deadlines.
    Review past releases: Look at previous testing timelines to estimate how long similar work took and where delays happened.
    Account for external deadlines: If the release needs to align with events such as conferences, campaigns, or customer commitments, include those constraints in your planning.
    Align with development schedules: Understand when development work is expected to be complete so you can plan test execution and bug validation accordingly.
    Build in buffer time: Unexpected delays are common. Add extra time for late code changes, environment issues, or retesting.

    3. Define test objectives
    Define will there be functional, performance, security or usuability testing


    4. Determine test deliverables
        Before testing: test plan , test case
        After testing: test completion report

    5. Design test strategy

        Identify testing types : manual testing
        automated testing
        smoke testing
        exploratory testing
        usability testing
        unit testing
        regression testing
        integration testing
        performance testing
        security testing
        accessibility testing

        Document risks and issues, 
        Document test logistics
        Test logistics define the practical details of how testing will be carried out, including the who, what, where, when, and how of execution.

        Establish test criteria - entry and exit criteria
        Example exit criteria may include:

        A target pass rate for critical test cases
        No open critical or high-severity defects
        Required regression coverage completed
        Key test deliverables submitted and reviewed
        For example, a team might set an exit criterion that 92% of critical test cases must pass before a feature is approved for release.

    6. Plan the test environment and test data
    
    Determine hardware and software requirements: Identify the devices, operating systems, browsers, databases, and testing tools needed to support the planned test scope.
    
    Install required software and tools
    
    Configure the network: Set up network conditions and configurations, 
    
    Prepare test data: Create or source the data required to run test cases. This may include mock data, anonymized production data, or data generated with automated tools.
    
    Ensure build access: Make sure testers can access the correct application builds, such as through a shared repository, CI/CD pipeline, or version control system.
    
    Verify the environment setup: 

## Key elements of a test plan

1. Test plan ID and title: A unique identifier and name for easy reference and version tracking.
2. Introduction and objective: A brief overview of the test effort, including its purpose and high-level goals.
3. Scope of testing: Defines what is in scope and out of scope for the test cycle.
4. Test objectives and approach: Describes testing goals and the overall approach, such as manual, automated, or risk-based testing.
5. Test schedule and milestones: Outlines the timeline for key phases, including planning, execution, defect triage, and test closure.
6. Test environment setup: Lists the hardware, software, tools, and configurations required to execute testing.
7. Resources and responsibilities: Identifies who is involved, their roles, and ownership across the test cycle.
8. Test deliverables: Defines the testing artifacts to be created or maintained, such as test cases, logs, reports, and defect records.
9. Entry and exit criteria: Specifies the conditions required to begin testing and the conditions that must be met to complete it.
10. Risks and mitigation strategies: Identifies potential blockers, constraints, or failure points and how the team plans to address them.

## When and how to update your test plan
A good test plan is not static. It should evolve as your project changes so the team stays aligned on scope, priorities, timelines, and release readiness.

Consider updating your test plan at these points:

after scope or requirement changes: If features are added, removed, or reprioritized, update your in-scope and out-of-scope items, test objectives, and coverage priorities.
when defects change testing priorities: If major defects affect timelines, environments, or focus areas, document those changes so the plan reflects current execution needs.
during sprint retrospectives or test cycle reviews: In Agile teams, use retrospectives to refine coverage, timelines, tools, and responsibilities for the next cycle.
when team ownership or capacity changes: If team members join, leave, or shift responsibilities, update the resources and responsibilities section to reflect current ownership and bandwidth.
at the start of a new phase or release: Carry forward lessons learned and revise entry criteria, exit criteria, risks, and deliverables for the next milestone.

# Calculator Test Plan

## Ask questions
1. Functional & Feature Scope
Which modes are in scope? "Are we testing only the Basic and Scientific modes, 
iPhone only or iPad as well?
Portrait + landscape modes?
History & Persistence: "Should calculation history synced via iCloud across other Apple devices?"
2. Compatibility & Environmental Scope
Hardware & OS Matrix: "Are we targeting only the latest iOS (e.g., iOS 18), or do we need to support legacy versions? Does the scope include iPadOS (which has different resizing logic) or watchOS?"
External Inputs: "Does the scope include external hardware, such as Bluetooth keyboards or Apple Pencil 
Orientation Handling: "How should the app behave during a mid-calculation orientation flip?
3. Edge Cases & Constraints
Input Limits: "Is there a hard character limit for the input string, or should the font size dynamically shrink to accommodate infinite input (the iOS 18 behavior)?"
Error Handling: "How should the app handle mathematical 'undefined' states (e.g., $0/0$ 
Performance Benchmarks: "Are there specific performance 'budgets' for cold-start time
4. Integration & Accessibility
Accessibility (A11y): "What are the requirements for VoiceOver? 
System Integration: "Are we testing the Siri integration for calculations, or just the standalone app?"

-----------

## Test plan
1. Intro and Objective
This test plan defines the strategy for validating the iOS Calculator app. The primary objective is to ensure that all basic and scientific mathematical operations are accurate, the interface is accessible, and the app remains stable under various stress conditions.

2. Scope of Testing
In-Scope
Basic arithmetic (Addition, Subtraction, Multiplication, Division).

Scientific calculator mode (Landscape orientation).

UI/UX transitions (Portrait to Landscape).

Accessibility features (VoiceOver, Dynamic Type).

Performance (Launch time, input latency).

iOS 18 Features: Math Notes integration, handwritten expression evaluation, and dynamic window resizing (for iPad/iPhone).

Edge Cases: Boundary Value Analysis, division by zero, and maximum character input.

Out-of-Scope
Hardware-level button failures (physical volume/power buttons).
button press from external hardware

3. Test Strategy

A Hybrid Approach combining:

Testing Levels

Unit Testing: Verify individual mathematical algorithms (handled by developers using XCTest).

Integration Testing: Ensure the Calculator app correctly interacts with iOS system services (e.g., Clipboard for copy/paste, Notes for Math Notes).

System Testing: End-to-end validation of the app as a whole.

Acceptance Testing: Acceptance testing verifies that the app meets business requirements and is "fit for use" by the end user. For an iOS calculator, it focuses on mathematical accuracy across all standard operations and ensuring the UI/UX aligns with Apple's design standards and user expectations.



Identify Testing Types

A. Functional Testing 

Smoke Testing: Verifying the most critical "happy path" functions - Basic Arithmetic, C functionality, operational chain (=)

Sanity Testing: Brief testing after a bug fix to ensure the specific fix works.(decimal related bug fix)

Regression Testing: Ensuring new code hasn't broken existing functionality.(like BODMAS, large number handling, zero handling) Boundary value analysis, interupt testing - call - calculation is not lost. Negative testing - see cases such as Multiple decimal points, operators without numbers, and division by zero.

Globalization/Localization: Checking if the app works in different languages or regions. (decimal seperation , and . for diff country, Comma placement differ for india and US)

B. Non-Functional Testing 
Verifies the internal attributes and performance of the software.

Performance Testing: Tapping the buttons in app very fast to see if there is a lag, rapidly rotate device

Security Testing: attempt to paste long string and see it does not crash the app

Usability Testing: button responsiveness, haptic feedback

Accessibility Testing: Voiceover accuracy


Identify what part will be 

Automated Testing: Regerssion and sanity checks 
Manual Testing: For exploratory, usability, and accessibility checks.



4. Test Environment
Hardware: iPhone 13, iPhone 15 Pro, and iPad Pro (for Math Notes).
Software: iOS 17.x (Base) and iOS 18.x (Beta/Release).
Tools:
Automation: XCUITest (native Apple framework) or Appium.
Proxy: Proxyman/Charles Proxy (to monitor any currency conversion API calls).
Test Management: TestRail or Jira.

5. Test Data
Standard Sets: Integers, decimals, and negative numbers.
Scientific Sets: Extremely large exponents, irrational numbers ($\pi$, $e$), and trigonometric inputs.
Invalid Inputs: Multiple decimal points, operators without numbers, and division by zero.
Handwritten Data: Apple Pencil inputs for Math Notes validation.

6. Resources and responsiibility and test schedule 
which team will be doing what 
Test schedule 
test planning - 1, env setup - 1, exe phase - 5, defect triage -1, test closure - 1

7. test deliverable 
test plan , automation exe report or traceability matrix generation

8. Entry and Exit Criteria
Entry Criteria: Stable build available in TestFlight; Test environment configured; Requirements signed off.

Exit Criteria: 100% of P0 and P1 test cases executed; No open "Blocker" or "Critical" defects; 


9. Risks and Mitigations
Risk: Floating-point errors causing slight inaccuracies in complex scientific calculations.
Mitigation: Use "Gold Standard" mathematical libraries to verify results against the app's output.
Risk: iOS 18 Math Notes might fail to recognize poor handwriting.
Mitigation: Test with various handwriting styles and provide a "Clear" or "Undo" path for users.
Risk: High-frequency input leading to UI "App Quiescence" or hangs.
Mitigation: Perform stress testing on the UI thread using automation.

Risk : 3rd party does not delievr on time till so and so date, will remove it from scope

## Test cases
Test cases for calculator

1. Functional Testing (Positive)
Ensures the app works as intended under normal conditions.
Basic Arithmetic: Perform 12 + 25 and verify the result is 37.
Scientific Switch: Switch to landscape mode and verify scientific functions (sin, cos, tan) appear.
Order of Operations: Input 2 + 3 * 4 and verify the result is 14 (PEMDAS/BODMAS).
Percentage Logic: Input 100 + 10% and verify the result is 110.
Memory Functions: Use m+ to store a value, perform another calculation, and use mr to recall the stored value correctly.
2. Functional Testing (Negative)
Ensures the app handles invalid inputs or illegal operations gracefully.
Division by Zero: Input 50 / 0 and verify the app displays "Error" or "Not a number."
Multiple Decimals: Attempt to enter 5.5.5 and verify the app ignores the second decimal point.
Leading Zeros: Input 00005 and verify the app displays it as 5.
Operator Overload: Press +, -, * consecutively and verify the app only honors the last operator pressed.
Invalid Math: Attempt to calculate the square root of a negative number (√-1) in basic mode and verify the error handling.
3. Functional Testing (Boundary)
Tests the extreme limits of the input and output ranges.
Maximum Digits: Enter the maximum allowable digits (usually 9 in portrait) and verify no further input is accepted.
Large Number Overflow: Multiply two very large numbers (e.g., $999,999,999 \times 999,999,999$) and verify it switches to scientific notation (e.g., 1e18).
Smallest Decimal: Perform calculations resulting in very small numbers (e.g., $1 \times 10^{-9}$) and verify precision.
Maximum Memory: Repeatedly add large numbers to memory (m+) to check for buffer overflow.
Negative Boundary: Subtract a large number from zero to reach the maximum negative display limit.

4. Non-Functional: Performance (Stress)
Pushes the app beyond normal operating capacity to find the breaking point.
Rapid Input: Tap digits and operators at 10+ taps per second to check for lag or crashes.
Long-Running Calculations: Perform a calculation with 50+ operators and nested parentheses.
Low Memory Stress: Launch the calculator when the iPhone has 99% RAM usage and verify it still functions.
Battery Drain: Run the app continuously for 2 hours and monitor for excessive heat or power consumption.
Rapid Rotation: Rotate the device between portrait and landscape 20 times quickly while a calculation is in progress.
5. Functional: Performance (Load)
Validates the app's behavior under expected "heavy" usage.
High-Volume History: Populate the "History" feature with 500+ past calculations and check for scrolling lag.
Simultaneous Features: Use "Math Notes" while recording a voice memo in the background to check for processing delays.
Backgrounding: Perform a complex scientific calculation, swipe to background, and return to verify the state is preserved immediately.
Cold Start Time: Measure the time from icon tap to "Ready for Input" state (should be < 1 second).
Data Sync Load: If using iCloud for "Math Notes," sync a large volume of handwritten equations and verify sync speed.
6. Usability Testing
Focuses on the user experience and ease of use.
Haptic Feedback: Verify that each button press provides a subtle vibration response.
Button Spacing: Ensure buttons are spaced appropriately to avoid "fat-finger" errors.
Dark/Light Mode: Verify the UI is legible and aesthetically pleasing in both Dark and Light appearance.
Swipe to Delete: Verify that swiping across the numbers deletes the last digit entered.
VoiceOver: Ensure that every button is correctly labeled for visually impaired users using Accessibility features.

7. Compatibility Testing
Ensures the app works across different hardware and software versions.
Screen Sizes: Test on iPhone SE (4-inch) vs. iPhone 15 Pro Max (6.7-inch) for UI clipping.
iOS Versions: Test on the current stable iOS and the latest Developer Beta.
Dynamic Type: Increase system font size in Settings and verify the calculator text doesn't overlap.
iPad Support: If testing a universal build, verify the "Split View" and "Slide Over" functionality on iPadOS.
External Keyboards: Connect a Bluetooth keyboard and verify that the number pad works for input.
8. Security Testing
Protects user data and privacy.
Clipboard Privacy: Verify that "Copy" only puts the numeric result in the clipboard and no hidden metadata.
Permission Control: Ensure the app doesn't ask for unnecessary permissions (e.g., Location or Contacts).
Math Notes Privacy: If syncing notes to iCloud, verify they are end-to-end encrypted.
Overlay Attacks: Ensure system-level popups (like a phone call) hide the calculator's sensitive data if requested.
Memory Scrubbing: Verify that clearing the memory (mc) actually wipes the data from the RAM.
9. Scalability Testing
Checks if the app can handle growth in features or data.
Unit Conversion: Verify the app can handle new unit types (e.g., adding a new currency) without UI breakage.
Scientific Notation: Ensure the app can scale from standard digits to E-notation without losing precision.
Expansion of History: Verify that as the history grows, the app doesn't slow down linearly.
Localization: Test how the UI scales when labels are translated into longer languages (e.g., German).
Math Notes Complexity: Verify the app handles increasingly complex handwritten formulas without lag.

10. Stability Testing
Ensures the app remains reliable over a long period.
Idle State: Leave the app open for 24 hours and verify it doesn't crash or leak memory.
Interruption Recovery: Receive a 10-minute phone call mid-calculation and verify the state is 100% intact upon return.
Low Battery: Test app behavior when the device hits 1% battery or enters "Low Power Mode."
Storage Full: Open the app when the iPhone has 0KB of storage left and verify it doesn't crash on launch.
Network Toggle: Switch from WiFi to Airplane Mode while using "Math Notes" and verify no crashes occur.
11. Smoke Testing
A quick "sanity check" of the most critical paths.
App Launch: Does the app open to the main screen?
Digit Input: Can I type the number 1?
Addition: Does 1 + 1 equal 2?
Clear All: Does the AC button reset the display to 0?
Result Display: Does the equals button (=) actually trigger a calculation?
12. Sanity Testing
Detailed testing of a specific new fix or feature.
Precision Fix: If a bug was fixed for 0.1 + 0.2, verify it now equals 0.3 exactly.
Orientation Layout: Verify the specific scientific buttons are correctly aligned after a landscape UI fix.
History Delete: Verify that swiping to delete a single history item works without deleting the whole list.
RPN Mode: If RPN was just added, verify the stack pushes and pops correctly.
Haptic Toggle: If a fix was made for haptics, verify toggling them off in Settings actually stops the vibration.
13. Regression Testing
Ensures new updates haven't broken existing features.
Legacy Math: Verify that standard multiplication still works after an update to the Scientific mode.
Settings Persistence: Verify that the "Scientific" preference stays saved even after an app update.
UI Assets: Verify that button icons haven't reverted to old designs after a code merge.
Accessibility Labels: Ensure that VoiceOver still reads "Plus" correctly after a localization update.
Copy/Paste: Re-verify that copying a result still works across other apps like Notes or Messages.

----
# Test Strategy
A high-level document that defines the general approach and principles guiding the testing process across multiple projects.

## Steps to Write a Test Strategy Document

Step 1. Understand the project requirements.

Gather information: Review the project requirements, objectives, and scope to determine what needs to be tested.
Stakeholder Consultations: Engage with stakeholders (such as product managers, developers, and business analysts) to ensure that expectations are aligned.

Step 2. Define the scope of testing.

Identify the Testing Levels: Specify which levels of testing will be performed (unit, integration, system, acceptance, etc.).
In-Scope Versus Out-of-Scope: Clearly state which features, modules, or areas will and will not be evaluated.

Step 3. Outline the Test Objectives

Determine goals: Define the major testing objectives, such as validating functionality, assuring performance, and detecting hazards.
Set criteria for when testing is complete and successful.

Step 4. Select Testing Approaches and Methodologies.

Testing Types: Choose which types of testing to use (functional, non-functional, regression, exploratory).

Step 5. Choose Testing and Environment Tools: 

Identify and document the necessary tools for test management, automation, performance testing, and defect tracking.
Configure the test environment(s), including hardware, software, network configurations, and data needs.

Step 6. Risk Management and Mitigation.

Identify risks: List potential hazards to testing (e.g., tight timelines, limited resources, technological issues).
Mitigation Strategies: Create plans to mitigate these risks and delegate responsibility for monitoring them.

Step 7. Define roles and responsibilities.

Team Structure: Outline the roles and duties of the testing team (Test Manager, QA Engineers, and Automation Engineers).
Communication Plan: Determine how and when team members will communicate progress, issues, and updates.

Step 8. Set up test metrics and reporting.

Key metrics: Determine the test metrics that will be measured (such as test coverage metrics, defect density, and test execution rate).
Reporting Process: Determine the frequency and type of test reports, as well as how they will be distributed to stakeholders.

Step 9. Create a test schedule and milestones.

Timeline: Create a high-level testing schedule that coincides with project deadlines and critical milestones.
Resource Allocation: Assign resources to different stages of testing based on availability and competence.


## Sample Test Strategy Document

1. Introduction

This Test Strategy outlines the testing approach, goals, tools, and resources to ensure the quality and reliability of the iOS Calculator product line.


2. Scope

In Scope:
Functional testing of login, product search, cart, checkout, payment, and order history modules.
Integration testing with in-house APIs (inventory, user accounts, payment processing).
Performance testing of the checkout flow.
UI and cross-browser testing for the responsive web application.
Out of Scope:
Testing of third-party delivery partner integrations.
Compatibility testing on legacy mobile devices (below Android 10/iOS 13).
Testing deprecated promotional modules being phased out.
3. Test Objectives

Ensure $100\%$ mathematical precision for all arithmetic and scientific operations.
Maintain a "Zero-Crash" policy for UI transitions (Portrait to Landscape).
Achieve seamless integration with iOS system features (Dark Mode, Haptics, Accessibility).

4. Testing Approach

B. Testing Levels & Types
Levels:
Testing Levels

Unit Testing: Verify individual mathematical algorithms (handled by developers using XCTest).

Integration Testing: Ensure the Calculator app correctly interacts with iOS system services (e.g., Clipboard for copy/paste, Notes for Math Notes).

System Testing: End-to-end validation of the app as a whole.

Acceptance Testing: Acceptance testing verifies that the app meets business requirements and is "fit for use" by the end user. For an iOS calculator, it focuses on mathematical accuracy across all standard operations and ensuring the UI/UX aligns with Apple's design standards and user expectations.

Types:

Functional: Core math, memory functions (M+, MR), and scientific modes.
Non-Functional: Performance (launch speed), Security (clipboard), and Accessibility (VoiceOver)

Automation Strategy (The "Senior SDET" Layer)
Toolstack: Appium with Java/TestNG for cross-version compatibility.

Framework Pattern: Page Object Model (POM) with a Centralized JSON Locator Repository for React Native/iOS hierarchy resilience.

Synchronization: Use Quiescent State Handling to manage looping animations on the "Today" or "History" tabs.

Parallelization: Execute tests across a Device Pool using a LinkedBlockingQueue to ensure zero driver collisions.

5. Test Environment and Tools

env - qa qa2 stage and prod 
tools for automation and exploratory 
Device Matrix: Must cover all screen sizes (iPhone SE to 15 Pro Max) and the last two major iOS versions.


6. Risk Management

3rd party dependency | mitigation plan

7. Roles and Responsibilities


8. Test Metrics and Reporting

Key Metrics:
Test Case Coverage (% of requirements tested)
Defect Density (defects per module)
Test Execution Rate (tests/day)
Pass/Fail Ratio
Reporting:
Daily smoke and functional test reports
Weekly defect summaries
Final Test Summary Report post-execution
9. Test Schedule

Phase	Start Date	End Date
Test Planning	June 21, 2025	June 24, 2025
Test Case Design	June 25, 2025	June 28, 2025
Test Execution (Cycle 1)	July 1, 2025	July 5, 2025
Regression Testing	July 6, 2025	July 9, 2025
UAT Support	July 10, 2025	July 12, 2025
Final Reporting	July 13, 2025	July 14, 2025

10. entry and exit criteria
$100\%$ of automated Smoke and Regression suites must pass.
All S1 and S2 defects must be closed.


# Test plan and test cases of other scenarios
  - login 
  - apple health - api
  - camera

# Positive, negative , boundary, stress, load, usability, compatibility, security . scalability, stability

  ## LOGIN
Positive - Basic Valid Login, forgot pass, case sensitivity, password masking, enter key for login button
Negative - invalid login
forgot pass
Attempt login with a correct username but incorrect password.
Attempt login with an unregistered email address.
Blank field


BOUNDARY - 
Password Length: If the limit is 8–16 characters, test exactly 7, 8, 15, 16, and 17 characters.
Special Characters: Test a password with 100% special characters vs. 0% special characters.
Leading/Trailing Spaces: Enter a valid email with a space at the end (user@test.com ) to see if the app trims it.
character limit in username

security or functional: 
Account Lockout: 
Expired/Inactive Accounts: 
Unverified Accounts: 
.MFA Failures: 
Session Timeout:

error handling

----

Load Testing:
Simulate 500 concurrent users logging in simultaneously via the API to check server response time.
Login response time UNDER SLA

Stress: 
simulate 5000 concurrent users logging in simultaneously via the API to check server response time.

Negative/Volume Testing (Data Flow): Flooding a database with massive amounts of data or concurrently executing thousands of transactions to detect when lockups, data corruption, or slowdowns occur

Resource Depletion (System Stress): Intentionally restricting a system's resources, such as limiting CPU to 10% or capping RAM, to evaluate if the software crashes or continues to run gracefully.

Recovery Testing (Disaster Simulation): Abruptly shutting down a database or server during peak load to analyze how quickly the system recovers and if it saves data securely


stability: 

scalability: 
----
Security : 
SQL Injection: 
Cross-Site Scripting (XSS): 
Unauthorized Navigation: Attempt to access the dashboard directly via its URL without being logged in.
-----
usability: 
UI  -  Responsive design

compatibility: devices, browser

# apple health - api 
https://medium.com/@zubairkhansh/10-critical-api-testing-scenarios-every-qa-engineer-should-master-2954e5e60e38

API testing scenarios are structured conditions designed to validate the functionality, security, and performance of an application's interface. Effective testing strategies typically cover the following categories: [1, 2]  
1. Functional Scenarios 
These tests ensure the API performs its intended operations correctly according to the requirements. 

• Status Code Validation: Verify that the API returns the expected HTTP status codes (e.g.,  for success,  for new resources, or  for missing resources). 
• Payload Accuracy: Check that the response body contains the correct data and matches the requested information. 
• CRUD Operations: Validate the complete "Create, Read, Update, and Delete" flow to ensure data integrity across the system. 
• Chain Requests (Scenario Testing): Mimic real-world user journeys by chaining multiple requests together, such as registering a user, then logging in with those credentials. [1, 4, 5, 6, 7, 8, 9, 10]  

2. Negative & Edge Case Scenarios 
These scenarios test how the API handles invalid or extreme inputs to ensure robustness. 

• Invalid Inputs: Send malformed data, incorrect data types (e.g., text in a numeric field), or empty fields to verify the API returns helpful error messages rather than crashing. 
• Boundary Value Testing: Test the minimum and maximum limits for input parameters to see how the system reacts to extreme values. 
• Fuzz Testing: Input random or "fuzzed" data to identify unexpected vulnerabilities or edge cases. [4, 11, 12, 13, 14]  

3. Security & Authentication Scenarios 
Security tests focus on protecting sensitive data and preventing unauthorized access. 

• Unauthorized Access: Attempt to access protected endpoints without a token or with an invalid API key to ensure they are properly blocked (typically a  response). 
• Insufficient Permissions: Verify that a user with restricted roles cannot perform administrative actions (Authorization testing). 
• Rate Limiting: Confirm the API correctly throttles users who make too many requests in a short period to prevent DDoS attacks or system overload. [1, 4, 6, 8, 15]  

4. Performance & Reliability Scenarios 
These assess the API's speed and stability under various traffic conditions. 

• Load Testing: Monitor how the API behaves under expected high traffic to ensure response times remain acceptable. 
• Stress Testing: Push the API beyond its capacity to find its breaking point and see how it recovers from failure. 
• Response Time Benchmarks: Measure the time it takes for various calls to ensure they meet performance service level agreements (e.g., responding in under 500ms). [4, 6, 12, 16, 17]  

5. Integration & Contract Scenarios 

• Contract Testing: Ensure the API follows the agreed-upon structure (schema) between the provider and the consumer to prevent breaking changes. 
• Database Integrity: Verify that API actions result in the correct changes within the underlying database (e.g., a "Delete" request actually removes the record). [1, 4, 5]  

# How would you decide what to manual test and what to automate ?


1. First principle (the most important)
Automate checks.
Manually test thinking.

Automation is great at:

Repeating
Verifying
Regressing
Humans are great at:

Exploring
Judging
Noticing the unexpected
2. What SHOULD be automated ✅
✅ 1. High‑risk, high‑impact paths
Anything that:

Affects revenue
Affects compliance
Affects core user flows
Examples:

User signup / onboarding
Payments
SMS/email consent flows
Order creation
Critical APIs
👉 If it breaks, you want to know immediately.

✅ 2. Tests that are run repeatedly
If a test is run:

Every build
Every release
Every sprint
✅ Automate it.

Manual repetition is expensive and error‑prone.

✅ 3. Deterministic, predictable behavior
Automation works best when:

Input → output is consistent
Assertions are clear
Examples:

API response codes
Validation rules
DB state changes
Contract checks
✅ 4. Regression tests
Anything that:

Has broken before
Is likely to break again
👉 This is automation gold.

✅ 5. API & backend logic
APIs are:

Stable
Fast
Less flaky than UI
✅ Perfect for automation:

REST‑Assured
Contract tests
Integration tests
✅ 6. Data‑driven scenarios
Same logic, different data:

Valid vs invalid inputs
Boundary values
Role‑based access
✅ Automate once, run many times.

3. What should stay MANUAL ❌ (or mostly manual)
❌ 1. New features (initially)
When a feature is:

Brand new
Changing rapidly
Poorly understood
✅ Manual first
✅ Automate later when stable

❌ 2. Exploratory testing
Things like:

“What if I try this?”
“Does this feel right?”
“Is this confusing?”
Humans > automation here.

❌ 3. UX / usability / visual feedback
Automation can check:

Element exists
But it cannot judge:

Clarity
Confusion
Tone
Trust
✅ Manual testing needed.

❌ 4. One‑time or rare scenarios
If a test is:

Run once
Rarely repeated
Very environment‑specific
❌ Automation ROI is low.

❌ 5. Highly unstable areas
If:

Requirements change daily
UI locators change constantly
APIs are not finalized
Manual until stable.

4. A simple decision framework (very practical)
Ask these 5 questions:

Will this test be run often?
Is the behavior stable?
Is the expected result clear?
Is failure costly?
Is it technically feasible?
✅ If most answers are YES → automate
❌ If most answers are NO → manual

5. Automation ROI rule (important)
If automation costs more to maintain than it saves, don’t automate it.

This is why:

Not everything should be automated
100% automation is a myth
6. How mature teams usually split work
Manual testing focuses on:
New features
Exploratory testing
Edge‑case discovery
UX validation
Automation focuses on:
Regression
API integration
Critical workflows
Data validation
Compliance rules
7. Test pyramid alignment
        Manual / Exploratory

     UI automation (few)

  API automation (many)  ✅

Unit tests (most)

``

SDETs usually own: ✅ API automation
✅ Integration tests
✅ Regression safety nets

8. Common mistakes to avoid 🚫
❌ Automating unstable UI
❌ Automating one‑off scenarios
❌ Automating without clear assertions
❌ Skipping manual exploration because “we have automation”

Automation supports testing — it doesn’t replace thinking.

9. Example from your context (SMS consent)
✅ Automate:

API returns correct consent state
Webhook processing
DB updates
Idempotency
Failure handling
🧠 Manual:

Message wording clarity
User understanding
Edge UX flows
10. One‑sentence answer (interview‑ready)
“I automate stable, repeatable, high‑risk checks and keep exploratory, UX, and fast‑changing scenarios manual to maximize confidence and ROI.”


 

## how much time do you spend automating test and doing manual test ? how is your day in work looks like as a sdet?



1. High‑level split: automation vs manual
In most mature teams, an SDET’s time looks roughly like this over a sprint:

60–70% automation & framework work
20–30% manual / exploratory testing
10–20% collaboration & analysis (design reviews, debugging, meetings)
This is not fixed — it shifts depending on the sprint.

2. How the split changes over time
Early in a feature / sprint
Manual testing is higher.

Understanding requirements
Exploring edge cases
Finding unknown risks
Verifying UX and flows
👉 Automation comes after understanding stabilizes.

Mid to late sprint
Automation dominates.

Automating regression scenarios
Adding API tests
Covering edge cases discovered manually
Stabilizing flaky tests
Mature product / steady state
Automation-heavy.

Most core flows already automated
Manual testing focuses on: 
New features
Exploratory testing
Release validation
3. What “manual testing” means for an SDET (important)
Manual testing is not repetitive clicking.

It usually means:

Exploratory testing
Edge‑case discovery
Failure scenario testing
Reviewing logs, payloads, DB states
Verifying integrations (webhooks, async flows)
This feeds directly into what gets automated next.

4. A typical SDET workday (example)
Morning
Check CI failures (API/UI)
Triage flaky tests
Review overnight test runs
Investigate failures (real bug vs test issue)
Mid‑day
Write or update automation: 
API integration tests
Testcontainers setup
Resilience / async tests
Improve framework utilities
Refactor existing tests
Afternoon
Manual testing of new features
Exploratory testing
Pair with developers on testability
Review PRs for test coverage gaps
End of day
Push automation changes
Update test results / notes
Communicate risks and findings
5. How SDETs decide in the moment what to do
A common decision rule:

If the risk is unknown → manual first
If the risk is known and repeatable → automate


6. Automation work is more than “writing tests”
Automation time includes:

Framework design
Utilities & helpers
Test data management
CI integration
Debugging flaky tests
Improving reporting
Removing technical debt


## 1. How I decide test scope
I decide test scope by balancing risk, business value, and change impact.

Key inputs I look at
What changed?
New features, refactors, config changes, dependency/version upgrades
Risk level
User impact, revenue impact, security/compliance, data integrity
Business priority
Critical user journeys, SLAs, regulatory features
History
Previously flaky or bug‑prone areas
Environment & platform
Browsers, devices, OS, integrations affected
Type of release
Hotfix vs minor vs major vs production incident fix
A simple rule I use:
Test what can break + what would hurt most if it breaks.

## 2. What I include vs exclude
✅ I include
Critical user flows
Login, checkout, payments, core workflows
Changed code paths
Direct changes and dependent areas
High‑risk integrations
APIs, third‑party services, data pipelines
Regression on historically unstable areas
Boundary and negative cases
Validation, error states, permissions
Smoke + sanity tests
Especially before pushing to higher environments
Automation candidates
Stable, repeatable, high‑value tests
❌ I exclude (or deprioritize)
Low‑risk, unchanged functionality
Purely cosmetic UI changes
Unless user‑facing or brand‑critical
Rare edge cases
When confidence is high and time is limited
Redundant tests
Covered elsewhere (unit, API, automation)
Unsupported environments
Browsers/devices outside the defined support matrix
Excluding tests is a conscious decision, not an oversight—and it’s always documented.

## 3. How I handle time constraints
When time is tight, I shift from “complete coverage” to “risk‑based confidence.”

Step 1: Re‑prioritize aggressively
I classify tests as:

P0 – Must test
Release blockers, critical paths
P1 – Should test
Important but not release‑blocking
P2 – Nice to have
Exploratory, edge cases
Only P0 is mandatory under extreme constraints.

Step 2: Reduce depth, not breadth
Prefer happy paths + key failures over full combinatorial testing
One solid scenario > five shallow ones
Sample environments instead of full matrix (e.g., Chrome + one mobile browser)
Step 3: Lean on automation & existing signals
Trust stable automation results
Use unit/API test coverage to justify reducing UI testing
Review logs, monitoring, feature flags for additional confidence
Step 4: Communicate and document risk
If something isn’t tested, I:

Explicitly call it out
Explain why it was skipped
Describe potential impact
Suggest post‑release validation
Example:

“Cross‑browser testing on Safari was skipped due to time constraints. Risk: layout regression. Mitigation: monitor user reports and schedule follow‑up test.”

4. Decision frameworks I commonly use
Risk‑based testing
Impact analysis
Test pyramid
More unit/API tests → fewer UI tests
80/20 principle
20% of tests usually cover 80% of production risk
 

 

# Structured Step‑by‑Step Debugging Approach (Generic)

STEP 1: Clearly define the problem (avoid assumptions)
Before debugging, write the problem in one precise sentence.

✅ Answer:

What is failing?
What was expected vs what happened?
STEP 2: Gather evidence
Evidence checklist
Error messages or logs
Screenshots / output
Timestamps
Inputs used
Environment details
Repro steps (if known)
 

STEP 3: Check for reproducibility
This step determines debugging depth.

✅ Ask:

Is it 100% reproducible?
Outcomes:
Consistent → logic/config/data issue
Intermittent → timing, state, concurrency, environment
 

STEP 4: Reduce the problem scope (isolation)
Debugging is about removing variables.

✅ Techniques:

Minimum set of steps
STEP 5: Where does behavior diverge from expectation?
 

STEP 6: Form a hypothesis (one at a time)
 

STEP 7: Test the hypothesis

 

STEP 8: Identify the root cause (not just the symptom)

 

STEP 9: Fix and verify
✅ Verification checklist:

STEP 10: Prevent recurrence


# ESTIMATIONS
Estimating QA work for a release involves breaking down the scope, analyzing complexity, and factoring in risks to predict the time and resources needed. A structured approach ensures realistic timelines and prevents last-minute, buggy releases. [1, 2]

Here is a step-by-step framework to estimate QA work for a release, based on industry best practices: [3, 4, 5]

1. Define the Release Scope and Requirements
Analyze User Stories: Read all stories to understand acceptance criteria, UX flows, and business intent. If requirements are vague, stop and clarify with the PO/BA.
Identify Testing Types: Determine which tests are required: Functional, Regression, Performance, Security, or UAT.
Define Coverage Matrix: List devices, operating systems, and browsers needed for testing, using market share or user analytics to guide the strategy. [1, 4, 6, 7]
2. Decompose the Work (Work Breakdown Structure - WBS) [4, 5, 8]

Break testing down into smaller, manageable tasks. A typical breakdown includes:
Test Design: Time to create test cases or update BDD scenarios.
Test Data/Environment Setup: Provisioning environments and preparing necessary data.
Manual Execution: Running happy path + edge cases.
Automation: Developing new scripts or updating existing ones.
Bug Triage/Retest: Time for retesting bugs after fixes.
Regression Suite Run: Ensuring old functionality remains stable. [4, 10, 11, 12]
3. Choose an Estimation Method
Three-Point Estimation (PERT): Use this for uncertainty. Calculate: (Optimistic + $4 \times$ Most Likely + Pessimistic) $\div$ 6.
Percentage of Development Time: A common shortcut is to take a percentage of the total development estimation (e.g., 25%–50% of dev time), though this should be verified with a custom estimate.
Historical Data: Use data from similar past projects. [1, 3, 13, 14]
4. Factor in Risks and Buffers
Environment Stability: If the staging environment is often broken, add a buffer.
Third-Party Dependencies: If testing depends on third-party APIs that may be slow, add extra time.
Rework/Bugs: Allocate 20–30% of total testing time for exploratory testing and unforeseen bugs. [3, 4]
5. Validate and Refine Estimates
Peer Review: Walk through the estimate with other QA team members.
Team Consensus: Discuss estimates with developers to ensure the technical complexity matches your assumptions.
Re-estimate if Needed: If the scope changes during the sprint, recalculate and communicate the new timeline immediately. [4, 5, 15]
Summary Checklist for Release Estimation

Task
Description
Requirements Clarity
Are acceptance criteria clearly defined?
Test Case Design
Time for writing/updating scripts?
Test Environment
Is it stable and ready?
Test Data
Time to create test accounts/data?
Automation
Time for new scripts + maintenance?
Regression
Full or partial suite?
Bug Verification
Time to retest fixes?
Buffer
20-25% for unknowns?


Here are 20 common interview questions and answers covering general software testing, methodology, and technical troubleshooting.


 
