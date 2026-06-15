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
