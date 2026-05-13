

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

given when then structure
observaibility

challenges in api automation - webhook, ingestion
tricky bug you resolved - awaitality 
load and performance
authentication header 
microservices 
graphql

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

