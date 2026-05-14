

## 1. Automation Framework Design (Very Likely)

### Questions:

* How would you design an API automation framework in Java?
* How do you structure layered architecture?
* How do you manage test data?
* How do you run tests in CI/CD?

### Be ready to explain:

```
Test Layer
Service Layer
API Client Layer
Utils
DB Layer
Reports
```

My API automation frameowrk is using a layered modular architecture 

1- API client layer 
It encapsulates all REST calls(get post put delete)
central place for endpoints, headers, queryparams, authentication etc.

The RequestSpecBuilder sets the baseURI, content type,header and other parameter and the request specification build it so that it can be used as given().spec() for each HTTP METHOD

method with Response as return type and different parameter are made such as 
```
     protected Response get(String endpoint) {
        return ApiRequestExecutor.execute(() -> RestAssured.given()
                .spec(requestSpecification)
                .when()
                .get(endpoint)
                .then()
                .extract()
                .response());
```
Centralized HTTP methods and request specification
Shared headers/content-type/base URL handling

2- Object model layer
it is a abstraction layer - making object of Client layer and writing 
Business-friendly wrappers around endpoint groups
```
    public Response fetchAskStories() {
        return client.getAskStories();
    }

```

3- Data Model layer
testdataloader - It contains Jackson Object Mapper class with derializes the test data in json file to Java objects.
Models - contain the POJO for request,testdata and response mapping - typed asserrions 

4 - Utility layer 
retry utility - 
config/env mgmt - 
token manager- 
logging - 
schema validation 

5 - Test layer 
test classes that use testng annotation to define and organize tests
it will be having test such as 
POST - create new resource - 

201 STATUS CODE , Validate that the returned id is not null
Header Validation: Ensure Content-Type is application/json.

Sending a POST request with an existing email or an empty required field.
Status Code: 400 Bad Request (for missing fields) or 409 Conflict (for duplicate data).
Error Message: Verify the response body contains a readable error string like "Email already exists".
Schema Consistency: Even on failure, the error response should follow a standard format (e.g., { "error": "code", "message": "details" }).

GET - fetch data - check response code as 200
Scenario: Any GET request (e.g., GET /v1/products/123).
What to Verify:
Data Types: Ensure price is a float/double, id is an integer/string, and isAvailable is a boolean.
Required Fields: Use a JSON Schema Validator (like the one in REST Assured) to ensure no mandatory fields are missing.
Enum Values: If a field like status can only be PENDING, SHIPPED, or DELIVERED, verify it doesn't return UNKNOWN.

AUTH
Authentication & Authorization (Security)
Scenario: Accessing a protected endpoint like DELETE /v1/orders/1 without a token.
What to Verify:
No Token: Verify 401 Unauthorized.
Invalid/Expired Token: Verify 401 Unauthorized with an "expired session" message.
Wrong Permissions (RBAC): Attempt to delete an admin resource using a "Guest" user token and verify 403 Forbidden.

PUT - replace entire resource 
patch - partially uodate resource 
PATCH (Partial Update): Send a request with only one field changed. Verify that other fields remain untouched and the updatedAt timestamp is refreshed.

DELETE - delete response 
204 No Content

rate limiting 
Use a loop to fire requests rapidly. Verify that once the limit is hit, the server returns 429 Too Many Requests.

E2E scenario - 
create consent when partner is active and consent creation is failed - should return 502 bad gateway
create consent when partner not found - create consent is failed - should return 400 bad request 
 
Test across multiple services 
json schema validation

6- few more things that I have in my framework 
database testing 
so for that I have test containers which have test SQL database 
where i put in some test data start of each test case and then verify the flow 

7- Wiremock server - to make 3rd party stubs 
wiremock server only simulate the response 
I do wiremock verify as well and confirm that application actually sent the correct request to wiremock.

Create partner in db -> stub wiremock(onetrust) to return fake accept reciept ->
call Post -> assert 201 created and response body ->
verify onetrust was called with valid partner id , transaction type etc

Validating both the call was made and request body matched expected contract

8- listeners / hooks

9- Resources - having test data, schema , 


---
# few more things to revise 

## given when then structure
Given(): The Prerequisites. This is where you set up the request. You define headers, query parameters, authentication, and the request body (payload).

When(): The Action. This is where you define the HTTP method (GET, POST, PUT, DELETE) and the specific endpoint URL.

Then(): The Validation. This is where you assert the results. You check the status code, response time, headers, and the response body content.

## tricky bug you resolved - awaitality 
1 - use awaitality 

The Scenario: The "Phantom" Consent Bug
The Context: While testing a bio-wearable app (like Lingo), you were validating the Consent API. A new consent was created for a partner, but when immediately followed by a GET request, the data was intermittently missing or returning stale values.

1. The Symptoms (The "Tricky" Part)
The test passed 90% of the time in the local environment.

The test failed 50% of the time in the CI/CD pipeline (GitHub Actions).

Standard REST Assured assertions like .statusCode(200) were passing, but the body() validation was failing because the consentStatus was still PENDING instead of ACTIVE.

2. The Investigation
You investigated the backend architecture and discovered that the system used Eventual Consistency.

The POST request saved the data to a primary SQL database but then triggered an asynchronous message (via Kafka/RabbitMQ) to update a secondary search index (Elasticsearch) used by the GET endpoint.

The CI/CD pipeline was faster than the local machine, creating a Race Condition where the test was asking for the data before the background sync was finished.

3. The "Smart" Resolution (Not just a Thread.sleep)
Instead of adding a brittle Thread.sleep(5000), which would slow down the entire suite, you implemented an Intelligent Polling Utility using Awaitility.


2- The Scenario: The "Broken Encoding" Bug
The Context: You were testing a User Profile API for a medical app. The app allows users to enter their legal names and medical history notes.

1. The Symptoms (The "Tricky" Part)
The API worked perfectly for standard names like "Naincy Gupta."

However, the test failed when you used a name with an accent, like "Renée", or a name with a hyphen.

The Twist: The POST (Create) request returned a 201 Created and looked successful. But when you ran the GET (Read) request, the name came back as "Ren??e" or caused a 500 Internal Server Error during JSON parsing in your Java framework.

2. The Investigation
You used REST Assured to inspect the raw response headers.

You discovered that the API was saving the data correctly in the database, but the Response Header for the GET call was missing the character encoding definition.

It was sending Content-Type: application/json instead of Content-Type: application/json; charset=utf-8.

Because the encoding wasn't specified, your Java framework (and the Mobile App) defaulted to ISO-8859-1, which couldn't read the special "é" character, leading to the "???" or a crash.

3. The Resolution
Short Term: You updated your REST Assured configuration to force UTF-8 decoding to prove that the data in the backend was actually correct.

The Fix: You logged a bug for the backend developers to explicitly set the charset=utf-8 in the API response headers.

The Framework Improvement: You added a "Data-Driven" test case to your TestNG suite that specifically loops through a list of "Naughty Strings" (names with emojis, accents, and different languages) to ensure this never regressed.

Why this is a great answer:
Shows Attention to Detail: Most testers only check the "Body," but you checked the Headers.

Global Mindset: It shows you understand that software needs to work for everyone, regardless of their language or name.

Technical Breadth: You demonstrated knowledge of Character Encoding (UTF-8 vs ISO), which is a common pain point in API development.


observaibility

## challenges in api automation - webhook, ingestion
Challenge: Cascading Test Failures Due to Missing API Data
The Problem:
In PartnerApiFetchData.java, tests are chained using dependsOnMethods. If an API returns empty or null data, all downstream tests would fail with NullPointerException or PathNotFoundException, producing misleading test reports.

How It Was Resolved:
1. Guard Tests with SkipException

From the actual code — SkipException is imported and used:
@Test(groups = {"smoke"}, dependsOnMethods = {"validate_retrieval_of_sensor_reading"})
private void validate_sensor_reading_data_exists() {
    List<Object> sensorReadings = JsonPath.read(responseBody, "$.data.sensorReadings");
    if (sensorReadings == null || sensorReadings.isEmpty()) {
        throw new SkipException("No sensor reading data available. Skipping dependent tests.");
    }
    assertThat(sensorReadings).isNotEmpty();
}

2. Global Retry for Transient Failures

From AnnotationTransformer.java:

@Override
public void transform(ITestAnnotation annotation, Class testClass, Constructor testConstructor, Method testMethod) {
    annotation.setRetryAnalyzer(RetryAnalyzer.class);
}
Every test method is automatically assigned the RetryAnalyzer, so if a test fails due to a transient issue (network blip, slow API response), it is retried before being marked as failed.

✅ Clear root cause identification — skips vs failures are distinct
✅ Reduced false negatives — retries handle transient issues
✅ Reliable CI/CD pipelines — fewer flaky test runs
✅ No code changes needed per test — AnnotationTransformer applies retry globally


——
Tricky bug
Can you tell me bug yound in api testing and how did you debug it?

POST /user → create user PUT /user → update user name GET /user → fetch user

GET returns updated name
GET returns old name for a few seconds

Step 1: Reproduce consistently
Ran test multiple times → issue was intermittent
Happened ~30–40% of the time
✅ → Indicates timing / eventual consistency issue
Step 2: Verify test is not wrong
Checked:

Request payload ✅ correct
Response parsing ✅ correct
Assertion ✅ correct
✅ Eliminated test script issue

Step 3 - print before and after operation


Step 4: Try delay (hypothesis testing)
Added small delay:

✅ After delay → GET returned correct value

👉 Strong signal:

Backend is eventually consistent / async

Step 5: Check headers / caching
Checked:

Cache headers (Cache-Control, ETag)
Response metadata
Found:

API was hitting a cached layer / read replica(used for get operation)
Step 6: Cross-check with backend/devs
Shared:

Logs
Timestamps
Request IDs
Backend confirmed:

GET API was reading from a read replica with lag

Fix / Resolution
Backend:

Fixed by:
Reading from primary for critical flows OR
Reducing replica lag / cache invalidation
QA side:

Added retry logic:
await().atMost(5, SECONDS).until(() -> {
    return getUser().getName().equals(updatedName);
});

“In one case, I found that after updating a resource via API, the GET endpoint was returning stale data. I debugged it by adding request/response logs and noticed the update response was correct but the GET response lagged. After experimenting with delays and checking headers, I identified it as an eventual consistency issue due to a read replica. I confirmed it with backend logs and fixed test instability using a polling mechanism while the backend addressed caching.”

Cache-Control and ETag are HTTP headers used to manage caching. Cache-Control defines how long and where responses can be cached, while ETag provides a version identifier to detect changes. In API testing, incorrect caching can cause stale data issues, so we inspect these headers and sometimes disable caching to debug inconsistencies.”
---------


authentication header 
microservices 
graphql


Load testing ensures your API can handle expected and unexpected traffic levels without degrading performance. Here are five essential scenarios to configure in JMeter, ranging from daily operations to extreme conditions.

---
## load and performance
### 1. The "Baseline" Scenario (Average Load)

This determines how the API performs under normal, expected conditions. It serves as your benchmark for future tests.

* **Goal:** Establish a performance baseline for response times and resource utilization.
* **JMeter Setup:**
* **Threads:** Use the average number of concurrent users your API sees daily.
* **Ramp-up:** Moderate (e.g., 5 minutes).
* **Duration:** 30–60 minutes.


* **What to watch:** Ensure response times stay within your defined **SLA (Service Level Agreement)**, such as 95% of requests under 200ms.

### 2. The "Peak Hour" Scenario (High Load)

This simulates the busiest time of the day or week (e.g., a Monday morning or a scheduled marketing blast).

* **Goal:** Verify the system can handle its maximum expected traffic without failing.
* **JMeter Setup:**
* **Threads:** Use your "peak" historical data + 10% buffer.
* **Ramp-up:** Rapid but steady.
* **Duration:** 1 hour.


* **What to watch:** Monitor for **latency spikes** or a rise in error rates (4xx or 5xx status codes) as the thread count hits its peak.

### 3. The "Stress Test" (Breaking Point)

Instead of testing what you expect, you test until the API fails.

* **Goal:** Identify the upper limit of the system and see how it fails (Gracefully? Or does the database crash?).
* **JMeter Setup:**
* **Threads:** Start at peak load and use a **Stepwise Ramp-up** (adding 50 users every 5 minutes) until the API starts timing out.
* **Duration:** Continuous until failure.


* **What to watch:** Note the "Knee Point" where response times begin to increase exponentially. This is your true capacity limit.

### 4. The "Soak Test" (Endurance)

Sometimes an API works fine for an hour but fails after ten hours due to memory leaks or full disk space.

* **Goal:** Identify memory leaks, database connection pool exhaustion, or logging issues.
* **JMeter Setup:**
* **Threads:** Average load (Scenario 1).
* **Duration:** 8 to 24 hours.


* **What to watch:** Check for a slow, steady **increase in memory usage** or a gradual slowdown in response times over several hours.

### 5. The "Spike Test" (Sudden Surge)

This simulates a sudden, massive influx of users, like a "Flash Sale" or a viral social media post.

* **Goal:** Test if the auto-scaling and load balancers react fast enough to a 5x or 10x traffic jump.
* **JMeter Setup:**
* **Threads:** Jump from 0 to a very high number (e.g., 500) in a very short window (under 30 seconds).
* **Duration:** Short (10–15 minutes).


* **What to watch:** Look for **"Recovery Time"**—how long it takes the API to return to normal performance after the spike subsides.


# docker
to run app in isolated lightweight container 
spins up real postgres sql database inside docker container 
test connect to port to reach the db inside docker 

## contract testing - 
Contract Testing is a methodology used to ensure that two separate systems (typically a Consumer, like a mobile app, and a Provider, like a REST API) are compatible and can communicate accurately.
using pact consists of producer and consumer 

The Consumer: Defines the expected response (JSON structure, status codes).

The Provider: Promises to fulfill that structure. MOCK provider 

The Pact File: A JSON file that acts as the "Contract" between them.

Step 1: Consumer Defines Expectations (Java)
In your TestNG project, you write a "Pact Test." You define the request you will send and the exact response you expect.

Step 2: Consumer Generates the "Pact File"
When you run the TestNG test, Pact starts a Mock Provider. It verifies your Consumer code can handle the mock response and then generates a pact-json file.

Step 3: Publish to Pact Broker
You push this JSON file to a Pact Broker (a central server). This is the "Source of Truth" that both teams can see.

Step 4: Provider Verification
The API team (Provider) pulls the Pact file from the Broker. Pact then automatically fires the requests defined in the contract against the real API and verifies the responses match the contract.

Step 5: Can-I-Deploy?
Before deploying to production, both teams run a tool called can-i-deploy. It checks the Broker to see if the version of the Consumer being deployed is compatible with the version of the Provider currently in production.

1. Why use Stubs instead of Pact?
Stubs (using tools like WireMock, Mockito, or simple JSON files) are often chosen for their simplicity and low barrier to entry.

Ease of Implementation: Setting up a stub takes minutes. Setting up Pact requires a Pact Broker, a shared infrastructure, and a change in how both the Consumer and Provider teams work.

Low Complexity: If you only have two services that rarely change, the overhead of managing a contract testing framework might seem like "over-engineering."

## steps on how do you api testing everyday 

## important HTTP status codes 
In the context of a **Java + REST Assured + TestNG** framework, mastering HTTP status codes is about more than just knowing the numbers; it’s about knowing the **assertions** and **business logic** they represent.

Here are the essential HTTP status codes, categorized by how you’ll use them in your framework.

---

### 1. The Success Family (2xx)

These codes confirm that the action requested by the client was received, understood, and accepted.

| Code | Name | When do we get it? | How to use in Test / Assertion |
| --- | --- | --- | --- |
| **200** | **OK** | Standard successful `GET`, `PUT`, or `POST`. | `.then().statusCode(200)` |
| **201** | **Created** | Successful `POST` that results in a new resource. | `.then().statusCode(201).body("id", notNullValue())` |
| **202** | **Accepted** | Request accepted for processing (Async). | Use when testing long-running background tasks. |
| **204** | **No Content** | Successful `DELETE` or `PUT` with no response body. | `.then().statusCode(204).body(is(emptyString()))` |

---

### 2. The Client Error Family (4xx)

These are critical for **Negative Testing**. They indicate the client (your test script) sent something wrong.

| Code | Name | When do we get it? | Difference / Interview Insight |
| --- | --- | --- | --- |
| **400** | **Bad Request** | Malformed JSON, missing required fields. | The "catch-all" for validation errors. |
| **401** | **Unauthorized** | Missing or invalid Authentication (Token). | **Diff:** You are unauthenticated (Who are you?). |
| **403** | **Forbidden** | Valid token, but no permission for this action. | **Diff:** You are authenticated, but not allowed (Access denied). |
| **404** | **Not Found** | Resource ID doesn't exist or URL is wrong. | Common follow-up: "Does DELETE return 404 on the 2nd try?" (Yes). |
| **409** | **Conflict** | Duplicate data (e.g., email already exists). | Used for testing unique constraint violations. |
| **415** | **Unsupported Media** | Wrong `Content-Type` (e.g., sent Text instead of JSON). | Test by changing `.contentType(ContentType.TEXT)`. |
| **429** | **Too Many Requests** | Rate limit exceeded. | Vital for testing system resilience and headers. |

---

### 3. The Server Error Family (5xx)

These indicate a crash or bug in the backend code. Your automation should usually fail if it sees these.

| Code | Name | When do we get it? | Use in Test |
| --- | --- | --- | --- |
| **500** | **Internal Server Error** | Code crash (NullPointerException in backend). | Marks a definite bug in the application. |
| **502** | **Bad Gateway** | Server/Proxy is down. | Often indicates environment or infrastructure issues. |
| **503** | **Service Unavailable** | Server is overloaded or down for maintenance. | Use this to trigger your **Intelligent Retry** logic. |
| **504** | **Gateway Timeout** | Backend took too long to respond. | Helps identify performance bottlenecks. |

---

### 4. Key Differences Interviewers Love to Ask

#### **A. 401 Unauthorized vs. 403 Forbidden**

* **401:** The server doesn't know who you are. You forgot the `Authorization` header.
* **403:** The server knows who you are, but you (e.g., a "Guest" user) don't have "Admin" rights to delete a resource.

#### **B. 200 OK vs. 201 Created**

* **200:** The request worked. Used for fetching data (`GET`) or updating data (`PUT`).
* **201:** Specifically for when a **new** record is added to the database.

#### **C. 204 No Content vs. 404 Not Found (on DELETE)**

* **204:** Successful deletion.
* **404:** Attempting to delete a resource that has already been removed or never existed.

---



### Top 3 "Interviewer" Favorites:

1. **"What status code is returned when a resource is successfully deleted?"** (Ans: 204 or 200).
2. **"What is the difference between a 401 and 403?"** (The most common security question).
3. **"If your API returns a 500, is that a script issue or an app issue?"** (Ans: App issue—server-side crash).

Does this help clarify which codes to focus on for your framework's assertions?







---

# 🔥 2. API Testing Deep Dive (Extremely Likely)

### Questions:

* How do you test POST/PUT/DELETE endpoints?
* Difference between 200 / 201 / 204 / 400 / 401 / 409 / 500?
* What is idempotency?
* How do you validate response schema?
* How do you test retries/timeouts?

### Example they may give:

> “How would you test an endpoint that stores health records?”

Talk about:

* positive/negative
* auth
* invalid payload
* duplicate requests
* DB validation
* privacy/security

