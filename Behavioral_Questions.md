# Behavioral & Cross-Functional Collaboration

## 1. Conflict Resolution (The "Data over Opinion" Rule)

**Question:** *“Tell me about a time you had a conflict with a developer regarding a bug priority or a test result.”*
Tell Background services bug - that led the user enter app violating 

* **Situation:** During a high-stakes release for the Lingo app, I found a race condition causing intermittent crashes on iOS.
* **Task:** The lead developer wanted to "monitor" it post-release because it only reproduced in ~2% of cases. I knew this would compromise user trust and Abbott’s quality standards.
* **Action:** Instead of just arguing, I provided a "Debug Package." I used **Appium logs**, a screen recording, and the specific thread dump to show the exact state inconsistency. I then collaborated with the dev to find a "middle ground" fix—a quick null-check that prevented the crash without a full architectural refactor.
* **Result:** We patched the bug within 4 hours. The release went out on time with zero reported crashes for that module.

**Question:** *Question: "How do you work with Product Managers to define 'Quality'?"*
Answer: "I view quality as a shared responsibility. I meet with PMs during the Sprint Grooming phase to define the 'Definition of Done.' I advocate for 'Testability' early on—asking if we have the right hooks or Accessibility IDs for automation. This prevents the 'QA as a bottleneck' syndrome."
Make sure requirements are well documented by pre reading all the stories and making notes to point out during grooming.
define defination of done and ask questions for testability.

## 2. Handling Pressure & Technical Ambiguity

**Question:** *“Describe a time when a major release was failing and you had very little time to debug.”*

* **Situation:** A React Native upgrade broke 40% of our automated suite two days before a major demo.
* **Task:** I had to quickly identify if these were real regressions or just locator flakiness from the hierarchy shift.
* **Action:** I prioritized. I manually verified the "Happy Path" (P0) first. Simultaneously, I implemented my **Intelligent Retry/Fallback** mechanism (using W3C coordinate taps) to bypass the broken locators and get a clean signal on the actual app functionality.
* **Result:** We identified that the app was stable; only the automation was "noisy." We cleared the release for the demo and fixed the locators the following day using our centralized JSON repo.

## 3. Leadership & Mentorship

**Question:** *“How do you handle a situation where a junior tester is consistently writing flaky scripts?”*

* **Answer:** I don't just fix the code for them. I initiate a **Pair Programming** session. I show them how to use **Fluent Waits** instead of `Thread.sleep()` and explain the "Why" behind it.
* **Process Change:** I implemented a "Sandbox Check" in our CI/CD where new scripts must pass 10 times consecutively in a separate environment before being merged into the master regression suite.

## 4. Conflict Resolution (The "Amigo" Conflict)
Question: "Give an example of a time you disagreed with your manager’s technical direction."

Situation: My manager wanted to achieve 100% automation coverage for all legacy features.

Task: Based on my experience, I knew the ROI on 100% coverage is negative due to high maintenance.

Action: I created a Risk-Based Testing Matrix. I showed that 20% of the features accounted for 80% of user traffic. I proposed focusing automation on these high-impact areas while using exploratory testing for the low-risk legacy features.

Result: We saved 30% of the team's bandwidth, which we then used to build the Device Pool Reservation System to solve our flakiness issues.

## 4. Cross-Functional Alignment (Shift-Left)


---

### Potential Follow-Up Questions from the Interviewer

1. **"How do you handle 'Pushback' from developers when you find a bug right before a holiday?"**
* *Strategy:* Focus on risk assessment. Ask: "What is the cost of a hotfix vs. the cost of delaying 2 days?"


2. **"Tell me about a time you failed. What did you learn?"**
* *Strategy:* Choose a technical oversight (like a missed edge case) and describe the **process change** (e.g., adding a new check to your Test Plan template) you implemented to prevent it.


3. **"If you could change one thing about your current team's culture, what would it be?"**
* *Strategy:* Focus on moving **Shift Left**—getting QA involved in the architectural discussions, not just the testing phase.


4. **"How do you stay updated with the SDET landscape?"**
* *Strategy:* Mention your GitHub repos (like [SDETInterviewPrep](https://github.com/NaincyGupt/SDETInterviewPrep)), following Appium's open-source updates, or experimenting with AI-based healing tools.



Would you like me to help you draft a "Quality Philosophy" statement to include as an introduction to this file?
