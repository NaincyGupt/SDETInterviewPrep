## Background

I am currently working as automation lead at Lingo which is one of the division of Abbott focussed on making glucose monitoring app

* Scaled mobile automation from the ground up by building a unified, cross-platform Appium/Java framework for iOS and Android, eliminating manual dependencies for core user journeys.
* Set up and integrated CI/CD execution pipeline utilizing an in-house mobile device farm, GitHub Actions, and BrowserStack, enabling automated, overnight regression runs for instant developer feedback.
* Collaborated with DevOps and SREs to create New Relic (network/performance metrics) and Sentry (crash tracking) directly into the test infrastructure.
* Automated API test automation framework using REST-Assured to validate high-value transactional flows, including customer data and checkout flows.
* Functioning as a Domain Lead driving cross-functional defect triage and defect management, root-cause analysis (RCA), and followed Agile practices.
* **Amazon:** Owned end-to-end API and UI automation for internal ad-targeting systems at Alexa. Architected nightly regression suites in AWS CodePipeline and built load test environments to test backend microservice.
* **Adobe & DXC Technology:** At Adobe, I developed regression test suites for complex Adobe PDF print engine, and At DXC I worked on automating test frameworks for telecom management systems.

------------

## Why do you want to join Apple?

* "I've been a loyal Apple user for many years.
* As an engineer, I’ve always deeply admired Apple’s strong engineering culture and their focus to quality, user experience and privacy.
* Apple's attention to detail and craftsmanship are also up to the mark.
* Throughout my career, I've  focused on building scalable automation frameworks, improving CI/CD quality gates, and validating complex mobile and backend systems.
* I'm particularly interested in applying those skills to products that operate at Apple's scale, where millions of users rely on the accuracy and reliability of the software every day.

------------

## Challenging Project - Device Farm Concurrency

* At Abbott, after building an in-house mobile device farm to replace BrowserStack, we hit a critical bottleneck when attempting to scale our automated test suites. As multiple feature teams integrated into the CI/CD pipeline and simultaneous code commits grew, the device farm began failing catastrophically. Tests were constantly crashing, throwing session overrides and widespread stale element exceptions. The pipeline became incredibly noisy with false negatives, causing development teams to completely lose trust in our automated quality gates and ignore test results, which severely threatened our release velocity."

* "I was tasked with identifying the root cause of this systemic flakiness, replacing the failed scaling strategy, and architecting an infrastructure layer that could handle concurrent executions reliably to restore the development team's confidence in the pipeline."	""

* "I began by conducting a deep failure analysis across months of execution logs. I discovered that our initial strategy—which relied on standard TestRunner multi-threading with dynamic Appium port allocation—was causing severe race conditions. Parallel threads were hitting the exact same physical devices at the same millisecond. 
To resolve this, I re-engineered our infrastructure's orchestration layer. I built a centralized device pool management system from scratch using a Linked Blocking Queue in Java. I decoupled the test runner from hardcoded targets, implementing a strict thread-safe lock-and-release mechanism: a thread requests an available device from the queue, locks it exclusively for that suite, and safely releases it back to the pool upon completion. I also authored automated cleanup shell scripts that executed between sessions to clear app caches, reset permissions, and handle dirty device states so every test started in a pristine environment."""

* "This architectural shift completely eliminated device collisions and resource exhaustion, driving our infrastructure-related false failure rate down to zero. We successfully scaled parallel test execution capacity across our entire device matrix, which slashed our overall pipeline execution time. Most importantly, it completely restored engineering trust: the development teams went from ignoring the framework to actively blocking code merges on pipeline failures, establishing a bulletproof quality gate for our mobile applications."


---------------

## AI 

1. Writing Test Cases Faster
I use Copilot or ChatGPT to quickly draft test cases from user stories or acceptance criteria. Example: “Login with valid/invalid credentials” → AI generates positive, negative, and edge cases.

2. Generating Boilerplate Automation Code
Page Object class templates
Driver setup
Basic test structure This saves time on repetitive coding.

3. Debugging Test Failures
I paste logs or stack traces into AI tools to quickly identify:
Common Appium/Selenium errors
Locator issues
Timing/synchronization problems

4. Creating Test Data
I use AI to generate realistic test data like:

5. AI-Based Flaky Locator Healing (Web + Mobile)

Step 1: Locator failure detection
When test fails:
NoSuchElementException
ElementNotInteractableException
Framework captures:

Screenshot
DOM snapshot
Previous working locator

Step 2: AI suggests alternative locator
AI model (or rule-based + ML hybrid) suggests alternatives like:

Step 3: Smart locator ranking engine
Rank based on:
Stability score
Uniqueness in DOM
Historical success rate

Step 4: Auto-healing execution (optional safe mode)
Framework tries:
Primary locator
AI-suggested fallback locators
Logs best match
If successful → update locator repository (with review flag)

🎯 Result
Reduced flaky test failures
Less manual maintenance
Faster debugging cycle
More resilient UI automation


---

## Challenging Bug 
dedup 

-----
## Conflict with dev/team/manager (how did you resolve the conflict ? What were learning from that episode ? Were you at good terms after that ? )

### The Conflict: Negotiating a "No-Go" on a Tight Deadline
Situation: "During a high-priority release for our bio-wearable app, we discovered a late-stage regression in the data syncing module just 24 hours before the scheduled deployment. The lead developer felt the bug was an 'edge case' and argued that we should ship the build to stay on schedule and fix it in a patch later. However, based on my assessment, I knew this could lead to data loss for a specific subset of users."

Task: "My task was to manage the disagreement professionally without damaging my relationship with the engineering team, while simultaneously acting as the 'gatekeeper' for the user experience and product integrity."

Action: "Instead of just saying 'no,' I took a data-driven approach to de-escalate the tension.

Objectivity: I quickly pulled our analytics to show that the 'edge case' actually affected about 5% of our active user base—thousands of people.
Collaboration: I sat down with the developer and, instead of criticizing the code, I focused on the risk to the brand. I proposed a middle ground: we would delay the release by just six hours, and I would personally assist in a focused 'war room' session to help reproduce and verify the fix immediately.
Communication: I kept the Product Manager informed with clear, non-emotional updates on the risk vs. reward of waiting for the fix."
Result: "The developer agreed once he saw the impact data. We fixed the bug, ran a targeted automation sweep, and shipped the build with only a minor delay. Afterward, I initiated a 'Post-Mortem' where we agreed to implement a new 'Critical Path' automated check earlier in the sprint to prevent this kind of late-stage friction. It actually strengthened our partnership because the developer realized I was there to protect his work, not just block it." I TRIED TO BOND with colleagues to create cohesive environment, we can have disagreements , but still come to a decison for team or project ,

A Wit-Check for the Interview: If they ask, "What if the manager had ordered you to ship it anyway?" Answer: "I would clearly document the known risks and provide a 'Mitigation Plan'—such as an immediate hotfix schedule or a targeted notification for affected users. At the end of the day, I advocate for the user, but I also understand that business decisions require balancing risks. My job is to make sure that risk is fully understood before the button is pressed."


-----

### The Conflict: Defending UI Consistency Against "Feature Dropping"

Situation: "Recently, during a high-pressure release planning meeting, stakeholders decided to move up a shipment date. To meet the new deadline, they proposed dropping several planned projects. However, the projects they chose to cut would have left the application in a 'split' state: a major new UI overhaul would be applied to the primary user flows, while secondary—but still critical—flows would remain on the legacy UI."

Task: "As the Quality Lead, I recognized that this 'disjointed' experience would confuse users and diminish the perceived quality of the brand. My task was to challenge the shipment plan and advocate for a cohesive user experience, even though the pressure to ship early was high."

Action: "I spoke up during the planning meeting, but I made sure to frame my objection through the lens of User Experience (UX) debt rather than just technical difficulty:

Visual Mapping: I created a quick 'User Journey Map' showing how a typical user would bounce between the new and old UIs, highlighting the friction points where buttons and navigation would change mid-session.
The 'Brand Integrity' Argument: I argued that shipping a 'half-finished' UI would lead to higher support tickets and a drop in user trust, which would outweigh the benefits of an early release.
The Solution: I proposed a 'Unified Toggle' strategy. If we couldn't finish the UI everywhere, I suggested we keep the new UI behind a feature flag for a smaller beta group, or revert the ship date by one week to ensure a 'Big Bang' release that was consistent across the entire app."
Result: "My manager and the stakeholders took the feedback seriously. After seeing the journey map, they realized the 'disjointed' UI felt like a bug rather than a feature. We reached a compromise: we delayed the release by ten days, but we re-scoped the sprint to focus only on the visual consistency of the core flows. We shipped a product that felt finished and intentional, and it resulted in one of our highest-rated updates in terms of user satisfaction."

A Wit-Check for the Interview: If they ask, "Why didn't you just let them ship it and fix it later?" Answer: "Because first impressions are permanent. If a user feels the app is 'broken' or 'inconsistent' on day one, it’s much harder to win their trust back on day thirty. Quality isn't a patch you apply later; it has to be baked into the launch."


----

## Why do want to leave abbott? ()


I've had a great experience at Abbott and have had opportunities to lead significant quality engineering initiatives, including automation modernization, device infrastructure, and CI/CD improvements."	"WHAT ARE SOME ISSUES ?

However, as a medical company it somehow lacks in beng very adoptive to new technologies, it take sometime for multiple rounds of approvals. "
