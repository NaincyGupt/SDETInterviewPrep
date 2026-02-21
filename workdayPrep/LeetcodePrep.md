Ooo okay 👀 preparing for **Workday SDET + HackerRank round** — nice.

First thing: companies like Workday usually don’t ask wild competitive programming puzzles for SDET. It’s more:

* Solid coding fundamentals
* Data structures
* String / array manipulation
* OOP design
* Basic automation logic
* Sometimes API / test framework thinking

Let me break this down realistically.

---

# 🔥 Common HackerRank Question Types for SDET (Workday Level)

## 1️⃣ String Manipulation

Very common.

Example:

* Reverse words in a sentence
* Check if two strings are anagrams
* Longest substring without repeating characters
* Validate parentheses

Example problem:

```text
Given a string, return the first non-repeating character.
```

They’re checking:

* HashMap usage
* Time complexity (O(n))
* Clean coding

---

## 2️⃣ Array / List Problems

Example:

* Remove duplicates from sorted array
* Find second largest number
* Merge two sorted arrays
* Rotate array by k steps

Typical SDET version:

```text
Given logs of test results, return failed test names sorted by frequency.
```

They want:

* Map
* Sorting with comparator
* Clean logic

---

## 3️⃣ Basic Data Structures

* Stack implementation
* Queue implementation
* LRU Cache (sometimes)
* Binary search

Example:

```text
Implement a stack using two queues.
```

This tests core CS basics.

---

## 4️⃣ OOP Design Question

Very common for SDET.

Example:

```text
Design a simple test automation framework structure.
```

Or:

```text
Design a parking lot system.
```

They check:

* Classes
* Interfaces
* Abstraction
* Clean design

---

## 5️⃣ API / JSON Handling

Sometimes they give:

* JSON input
* Parse and extract values
* Validate response structure

Example:

```text
Given JSON test results, count total failures per suite.
```

This checks:

* JSON parsing
* Iteration
* Clean logic

---

## 6️⃣ Concurrency Basics

Since you’re SDET, they might test:

* What is thread-safe code?
* Synchronize two threads
* Print even/odd using two threads

Example:

```text
Two threads print numbers alternately from 1 to 10.
```

If you understand ThreadLocal + parallel testing, you’ll crush this.

---

# 🧠 What Workday Specifically Focuses On

From candidate experiences:

✔ Clean code
✔ Edge case handling
✔ Unit-test thinking
✔ Time complexity explanation
✔ Writing readable code (not hacky)

They may ask:

> “How would you test this function?”

That’s where you stand out.

---

# 🎯 Example HackerRank Question They Might Ask

### Example 1:

```text
Given a list of integers, return true if any value appears at least twice.
```

Solution: Use HashSet.

---

### Example 2:

```text
Given test execution logs, find the most frequently failing test.
```

Use:

* HashMap
* Sorting
* Comparator

---

### Example 3:

```text
Validate if a string containing (), {}, [] is valid.
```

Use stack.

---

# ⚡ How to Prepare Smartly (Not Randomly)

Focus on:

### Must Know:

* HashMap
* HashSet
* ArrayList
* StringBuilder
* Sorting
* Comparator
* Streams (bonus)

### Practice On:

* HackerRank Easy + Medium
* LeetCode Easy
* Focus on clarity, not tricks

---

# 🚀 How To Impress in Coding Round

When solving:

1. Clarify input assumptions
2. Talk about edge cases
3. Mention time complexity
4. Write clean methods
5. Add simple test cases verbally

Example:

> “Time complexity is O(n) because we iterate once and use HashSet for constant lookup.”

That makes you sound strong.

---

# 🧨 Bonus: They Might Also Ask Automation Coding

Like:

* Write a function to retry a method 3 times
* Parse log file and count errors
* Implement simple test runner logic

---

If you want, I can:

* Give you 5 realistic Workday-style practice questions
* Conduct a mock coding interview
* Or give you a 7-day prep plan

What stage are you at? Phone screen or onsite? 😌
