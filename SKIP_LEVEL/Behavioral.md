## BACKGROUND 

I am currently working as automation lead at Lingo which is one of the division of Abbott focussed on making glucose monitoring app 

* **Framework Architecture:** Scaled mobile automation from the ground up by building a unified, cross-platform Appium/Java framework for iOS and Android, eliminating manual bottleneck dependencies for core user journeys.
* **Infrastructure Scaling:** Set up an integrated CI/CD execution pipeline utilizing an in-house mobile device farm, GitHub Actions, and BrowserStack, enabling automated, overnight multi-device regression sweeps for instant developer feedback loops.
* **Production Observability:** Collaborated cross-functionally with DevOps and SREs to bake New Relic (network/performance metrics) and Sentry (crash tracking) directly into the test infrastructure, shifting quality data left and catching regressions before production release.
* **End-to-End Coverage:** Automated API test automation framework  using REST-Assured to validate high-value transactional flows, including customer data and checkout flows.
* **Domain Ownership:** Functioning as a  Domain Lead driving cross-functional defect triage, deep root-cause analysis (RCA), and Agile governance alongside Dev and Product teams. 

* **Amazon (Alexa Ecosystem):** Owned end-to-end API and UI automation for internal ad-targeting systems. Architected nightly regression gates in AWS CodePipeline and built load test environments to test backend microservice resilience under peak traffic stress.
* **Adobe & DXC Technology:** Developed specialized regression test suites for complex Adobe PDF print engine at Adobe, and engineered automated frameworks for telecom management systems at DXC technology.

---
## What is Lingo ?

The Lingo biosensor, which can be worn for 14 days, tracks blood glucose levels, while its accompanying app provides personalized, real-time insights and customized coaching to help people create healthy eating habits, retrain their metabolism and improve their overall well-being.

----

## About Apple 

Apple is organized by functional specialties rather than business units — rare for a company our size. We’re experts leading experts: hardware experts lead hardware, software experts lead software, and design experts lead design. This differs from most other large companies, where general managers oversee managers. Apple is Apple because those with the most expertise in an area of work have decision rights for that area.

Leaders at Apple combine their expertise with two other important characteristics: immersion in the details and a willingness to collaboratively debate during collective decision-making. For people at every level here, it can be liberating — even exhilarating — to work with experts who offer relevant guidance and mentoring. This approach to leadership is a commitment to collaboration that leads to innovation.
--------

## why do you want to join apple?

I'm excited about Apple because of its strong engineering culture and relentless focus on quality and user experience. Throughout my career, I've worked on mobile applications, automation platforms, API testing, and healthcare-related products where I understood that reliability and accuracy are critical. What stands out to me about Apple Health is that the software directly impacts how users understand and manage their health, so quality isn't just a feature—it's fundamental to user trust.

In my current role at Abbott, I've been focused on building scalable automation frameworks, improving CI/CD quality gates, and validating complex mobile and backend systems. I'm particularly interested in applying those skills to products that operate at Apple's scale, where millions of users rely on the accuracy, performance, and reliability of the software every day.
---------

## why are you leaving Abbott?

"I've had a great experience at Abbott and have had opportunities to lead significant quality engineering initiatives, including automation modernization, device infrastructure, and CI/CD improvements. At this stage of my career, I'm looking for an environment where engineering innovation, large-scale distributed systems, and cutting-edge technologies are core parts of the product development process. Apple offers an opportunity to work on highly complex systems at massive scale while continuing to grow technically and contribute to products used by millions of customers."

--------

Why should we hire you? / Why are you good fit for us?

"I am a strong fit because I don't just write test scripts; I build scalable quality infrastructure. Whether it was building an Appium framework from scratch at my current company or managing high-load Alexa service testing at Amazon, I focus on 'Observability'—ensuring that every test run provides actionable data to developers through CI/CD integration and real-time monitoring. I’m ready to bring that same culture of automated, proactive quality to your team."

--------

Tell me about the most impactful project you've worked on. /
Biggest impact you have done: When You Solved a Problem

"I resolved the issue of fragile automation by architecting a resilient locator system using a centralized JSON repository with a progressive fallback mechanism. By decoupling the test logic from the volatile React Native hierarchy, I ensured that if a primary ID fails, the framework automatically attempts alternative strategies like iOS Class Chains or Android UiAutomator. This 'self-healing' approach reduced maintenance time by 80% and prevented false failures during framework upgrades."

Internal device farm 

Playwright Migration

--------

## When You Overcame a Challenge

### Introduced android - test become flaky
When introduced android - the test became flaky - where developers began to lose trust in our automation results."

My goal was to stabilize the framework and restore the team's confidence in our CI/CD pipeline. 

First, I implemented a centralized locator repository using a JSON-based structure that handled platform-specific identifiers for iOS and Android more intelligently. 
Second, instead of using generic 'sleep' statements, I developed a custom 'smart-wait' utility and an intelligent retry logic.I also collaborated closely with the dev team to ensure that unique accessibility IDs were added to the source code to make the UI more testable."
Third, I engineered a custom Android Gesture Utility - like vertical scrolling, swiping, and multi-touch interactions

"As a result, we reduced our flakiness from 30% to under 2%. This improved our release velocity significantly, as stakeholders could now rely on the automation report 

### Changing leadership - affecting product timelines ?

### internal device farm
The Challenge: Architecting an In-House Mobile Device Lab for High-Security Testing
Situation:
did not got clearance to exeute test protocols on emulator and simulator
Task:
"I was tasked with architecting and building a private, in-house device farm from scratch. The goal was to provide the team with a 24/7 automated execution engine that offered the same parallelization benefits of the cloud but within our own secure network firewall."
Action:
"This was a complex project that required solving several 'physical-meets-digital' hurdles:
Infrastructure Setup: I configured a dedicated Mac Mini rack as the host controller, using high-quality powered USB hubs to maintain stable data connections and prevent battery swelling on the iPhones and Android devices.
Software Layer: I utilized Appium Grid and Selenium Grid to manage device distribution. I wrote custom shell scripts to automate the 'cleanup' phase—ensuring each device cleared its cache, reset permissions, and stayed awake between test runs.
Network Orchestration: I worked with our IT department to set up a dedicated VLAN for the lab, ensuring the devices could communicate with our internal staging servers while remaining isolated from the rest of the corporate network."
Result:
"i successfully stabilized a farm of 20+ diverse physical devices. This shifted our automation coverage from 0% to 90% for critical regression paths in our secure environment. We reduced the feedback loop for developers from two days to under two hours, and because it was in-house, we eliminated the recurring monthly costs of cloud subscriptions, saving the department significant budget while increasing security compliance."

A Wit-Check for the Interview:
If they ask, "What was the most surprising difficulty of an in-house farm?" 
health scripts - random failures if appium session not started, device is offline, updates
--------

## Conflict with team/developer 

The Conflict: Negotiating a "No-Go" on a Tight Deadline
Situation:
"During a high-priority release for our bio-wearable app, we discovered a late-stage regression in the data syncing module just 24 hours before the scheduled deployment. The lead developer felt the bug was an 'edge case' and argued that we should ship the build to stay on schedule and fix it in a patch later. However, based on my assessment, I knew this could lead to data loss for a specific subset of users."
Task:
"My task was to manage the disagreement professionally without damaging my relationship with the engineering team, while simultaneously acting as the 'gatekeeper' for the user experience and product integrity."
Action:
"Instead of just saying 'no,' I took a data-driven approach to de-escalate the tension.
Objectivity: I quickly pulled our analytics to show that the 'edge case' actually affected about 5% of our active user base—thousands of people.
Collaboration: I sat down with the developer and, instead of criticizing the code, I focused on the risk to the brand. I proposed a middle ground: we would delay the release by just six hours, and I would personally assist in a focused 'war room' session to help reproduce and verify the fix immediately.
Communication: I kept the Product Manager informed with clear, non-emotional updates on the risk vs. reward of waiting for the fix."
Result:
"The developer agreed once he saw the impact data. We fixed the bug, ran a targeted automation sweep, and shipped the build with only a minor delay. Afterward, I initiated a 'Post-Mortem' where we agreed to implement a new 'Critical Path' automated check earlier in the sprint to prevent this kind of late-stage friction. It actually strengthened our partnership because the developer realized I was there to protect his work, not just block it."


A Wit-Check for the Interview:
If they ask, "What if the manager had ordered you to ship it anyway?"
Answer: "I would clearly document the known risks and provide a 'Mitigation Plan'—such as an immediate hotfix schedule or a targeted notification for affected users. At the end of the day, I advocate for the user, but I also understand that business decisions require balancing risks. My job is to make sure that risk is fully understood before the button is pressed."

------------

## Conflict with manager:



The Conflict: Defending UI Consistency Against "Feature Dropping"
Situation:
"Recently, during a high-pressure release planning meeting, stakeholders decided to move up a shipment date. To meet the new deadline, they proposed dropping several planned projects. However, the projects they chose to cut would have left the application in a 'split' state: a major new UI overhaul would be applied to the primary user flows, while secondary—but still critical—flows would remain on the legacy UI."
Task:
"As the Quality Lead, I recognized that this 'disjointed' experience would confuse users and diminish the perceived quality of the brand. My task was to challenge the shipment plan and advocate for a cohesive user experience, even though the pressure to ship early was high."
Action:
"I spoke up during the planning meeting, but I made sure to frame my objection through the lens of User Experience (UX) debt rather than just technical difficulty:
Visual Mapping: I created a quick 'User Journey Map' showing how a typical user would bounce between the new and old UIs, highlighting the friction points where buttons and navigation would change mid-session.
The 'Brand Integrity' Argument: I argued that shipping a 'half-finished' UI would lead to higher support tickets and a drop in user trust, which would outweigh the benefits of an early release.
The Solution: I proposed a 'Unified Toggle' strategy. If we couldn't finish the UI everywhere, I suggested we keep the new UI behind a feature flag for a smaller beta group, or revert the ship date by one week to ensure a 'Big Bang' release that was consistent across the entire app."
Result:
"My manager and the stakeholders took the feedback seriously. After seeing the journey map, they realized the 'disjointed' UI felt like a bug rather than a feature. We reached a compromise: we delayed the release by ten days, but we re-scoped the sprint to focus only on the visual consistency of the core flows. We shipped a product that felt finished and intentional, and it resulted in one of our highest-rated updates in terms of user satisfaction."
A Wit-Check for the Interview:
If they ask, "Why didn't you just let them ship it and fix it later?"
Answer: "Because first impressions are permanent. If a user feels the app is 'broken' or 'inconsistent' on day one, it’s much harder to win their trust back on day thirty. Quality isn't a patch you apply later; it has to be baked into the launch."

----------

## What does quality mean to you?

Quality means delivering a reliable experience that users can trust. It's not just finding defects; it's building confidence through prevention, automation, observability, and collaboration throughout the development lifecycle."

----------

When You Made a Mistake: 

Situation:
"Early in a major release cycle for our bio-wearable app, I was responsible for overseeing the final automation sign-off. We had a tight deadline, and while our automated suite passed 100%, a critical bug related to data syncing on older iOS versions reached production, affecting a small subset of users."
Task:
"I had to identify why our 'perfect' automation missed this and, more importantly, ensure it couldn't happen again. In the medical domain, data integrity is paramount, so any miss is a serious learning opportunity."
Action:
"I took immediate ownership. After performing a root cause analysis, I discovered that our automation was primarily running on the latest OS versions, and we had a gap in our cross-version compatibility matrix.
To fix this, I didn't just add a test case. I revamped our CI/CD infrastructure to use a cloud-based device farm that triggered parallel runs across a broader spectrum of legacy devices and OS versions. I also implemented a 'Technical Debt' review into our sprint planning to ensure we were updating our test environment profiles as frequently as our production user data suggested."
Result:
"We caught three similar edge-case bugs in the very next sprint before they reached the user. The experience taught me that quality isn't just about the code you write today, but about the diversity of the environment you test in. It made me a much more proactive Lead, as I now prioritize 'edge-case discovery' sessions at the start of every feature design."


## How do you ensure quality in a fast-moving team?
Shift-left testing
API-first validation
Automation
CI/CD quality gates
Risk-based testing

## BUG 
Cloud Sync Duplicate Write

How do you decide when software is ready to ship?
Escaped defects
Flaky test rate
Automation coverage
Pass rate
MTTR
Build stability
Regression duration

How do you handle disagreement with engineering leadership?
Strong Answer

"I present risks with data, explain customer impact, and propose mitigation options rather than simply saying yes or no."


What would your manager say is your biggest strength?
Ownership
Reliability
Problem solving
Collaboration

weakness
I sometimes dive too deeply into technical details and have learned to communicate at the right level for different audiences."

How do you prioritize testing?
Framework:
User impact
Business impact
Risk
Complexity
Recent changes
🍎 17. How do you use AI in quality engineering?
Use your real experience:
test generation
log analysis
test prioritization
🍎 18. Tell me about a failure.

Use:

DST Timezone Bug

Key:

accountability
learning
prevention

QA
"What qualities distinguish the most successful quality engineers on your team?"
