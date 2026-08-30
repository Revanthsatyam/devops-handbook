# Amazon API Gateway — Learning Syllabus

This is the master syllabus for our **Amazon API Gateway** learning journey.

We will learn each concept individually, document it properly, review it together, and then mark it as complete.

The goal is not just to memorize API Gateway features. The final documentation should be detailed enough that someone learning a concept for the first time can understand it, implement it in AWS, test it, troubleshoot it, and use the documentation later for revision.

---

# 🟢 Part 1 — API Gateway Fundamentals

## 1. What is API Gateway?

- [ ] What is an API?
- [ ] What is Amazon API Gateway?
- [ ] Why do we need an API Gateway?
- [ ] Client → API Gateway → Backend
- [ ] API Gateway's responsibilities
- [ ] API Gateway vs Backend
- [ ] REST API vs HTTP API
- [ ] Regional endpoint

### Basic Request Flow

```text
Client
   ↓
API Gateway
   ↓
Backend
```

---

## 2. APIs, Resources, Methods and Routes

This is the foundation of API Gateway.

### API

- [ ] What is an API?
- [ ] What does an API represent in API Gateway?
- [ ] API endpoint
- [ ] API Gateway URL

### Resource

- [ ] What is a resource?
- [ ] Resource path
- [ ] Root resource
- [ ] Child resources
- [ ] Resource hierarchy

### Method

- [ ] What is an HTTP method?
- [ ] Method + resource relationship
- [ ] Method configuration

### Route

- [ ] What is a route?
- [ ] Route matching
- [ ] Resource + method

### Parameters

- [ ] Path parameters
- [ ] Query parameters
- [ ] Headers
- [ ] Request body

### Response

- [ ] Response
- [ ] Response headers
- [ ] Response body
- [ ] HTTP status codes

### Example

```text
/users
/users/{id}

GET    /users
POST   /users
GET    /users/{id}
PUT    /users/{id}
PATCH  /users/{id}
DELETE /users/{id}
```

---

# 🟢 Part 2 — HTTP Methods

Each HTTP method will have its own section.

For every method we will cover:

- [ ] What it means
- [ ] Why it exists
- [ ] When to use it
- [ ] How it works
- [ ] Request structure
- [ ] Response structure
- [ ] Real-world example
- [ ] API Gateway implementation
- [ ] AWS Console implementation steps
- [ ] Testing
- [ ] Expected result
- [ ] Common problems
- [ ] Troubleshooting
- [ ] Important points to remember

## 3. GET

- [ ] What GET means
- [ ] When to use GET
- [ ] How GET works
- [ ] GET request structure
- [ ] GET response
- [ ] GET with path parameters
- [ ] GET with query parameters
- [ ] Real-world example
- [ ] API Gateway implementation
- [ ] AWS Console steps
- [ ] Testing
- [ ] Expected result
- [ ] Common errors
- [ ] Troubleshooting

## 4. POST

- [ ] What POST means
- [ ] When to use POST
- [ ] How POST works
- [ ] Creating resources
- [ ] Request body
- [ ] Request headers
- [ ] Response
- [ ] Real-world example
- [ ] API Gateway implementation
- [ ] AWS Console steps
- [ ] Testing
- [ ] Expected result
- [ ] Common errors
- [ ] Troubleshooting

## 5. PUT

- [ ] What PUT means
- [ ] When to use PUT
- [ ] How PUT works
- [ ] Replacing a resource
- [ ] Request body
- [ ] Request headers
- [ ] Response
- [ ] PUT vs PATCH
- [ ] Real-world example
- [ ] API Gateway implementation
- [ ] AWS Console steps
- [ ] Testing
- [ ] Expected result
- [ ] Common errors
- [ ] Troubleshooting

## 6. PATCH

- [ ] What PATCH means
- [ ] When to use PATCH
- [ ] How PATCH works
- [ ] Partial updates
- [ ] Request body
- [ ] Request headers
- [ ] Response
- [ ] PATCH vs PUT
- [ ] Real-world example
- [ ] API Gateway implementation
- [ ] AWS Console steps
- [ ] Testing
- [ ] Expected result
- [ ] Common errors
- [ ] Troubleshooting

## 7. DELETE

- [ ] What DELETE means
- [ ] When to use DELETE
- [ ] How DELETE works
- [ ] Deleting resources
- [ ] Request structure
- [ ] Request headers
- [ ] Response
- [ ] Real-world example
- [ ] API Gateway implementation
- [ ] AWS Console steps
- [ ] Testing
- [ ] Expected result
- [ ] Common errors
- [ ] Troubleshooting

## 8. OPTIONS & CORS

- [ ] What OPTIONS means
- [ ] Why browsers use OPTIONS
- [ ] What CORS is
- [ ] CORS preflight
- [ ] Preflight request
- [ ] Preflight response
- [ ] Allowed origins
- [ ] Allowed methods
- [ ] Allowed headers
- [ ] Credentials
- [ ] Actual request
- [ ] API Gateway CORS configuration
- [ ] Testing CORS
- [ ] Common CORS problems
- [ ] Troubleshooting

### Browser Flow

```text
Browser
   ↓
OPTIONS
   ↓
API Gateway
   ↓
CORS response
   ↓
Browser
   ↓
POST / GET / PATCH / etc.
```

---

# 🟢 Part 3 — API Gateway Configuration

## 9. Integrations

### What is an Integration?

- [ ] What integration means
- [ ] Why API Gateway needs an integration
- [ ] API Gateway → Backend
- [ ] Integration request
- [ ] Integration response
- [ ] Backend response

### Lambda Integration

- [ ] API Gateway + Lambda
- [ ] Lambda integration
- [ ] Lambda proxy integration
- [ ] Request passed to Lambda
- [ ] Lambda response
- [ ] API Gateway response

### Integration Problems

- [ ] Backend errors
- [ ] Integration errors
- [ ] Timeout
- [ ] Incorrect integration configuration
- [ ] 5XX responses
- [ ] Troubleshooting integrations

### Basic Flow

```text
Client
   ↓
API Gateway
   ↓
Integration
   ↓
Backend
   ↓
Integration
   ↓
API Gateway
   ↓
Client
```

## 10. Stages

### What is a Stage?

- [ ] What a stage is
- [ ] Why stages exist
- [ ] Stage URL
- [ ] Stage configuration
- [ ] Relationship between API and stage

### Environments

- [ ] `dev`
- [ ] `test`
- [ ] `prod`

### Example

```text
API
├── dev
├── test
└── prod
```

### Important Concepts

- [ ] Why different environments use different stages
- [ ] Testing the correct stage
- [ ] Stage-specific configuration
- [ ] Stage + deployment relationship

## 11. Deployments

This is especially important because we encountered this during our lab.

- [ ] What a deployment is
- [ ] Configuration vs deployed configuration
- [ ] Why changing something doesn't necessarily change the live API
- [ ] Creating a deployment
- [ ] Deploying to a stage
- [ ] Testing the deployed API
- [ ] Testing the correct stage
- [ ] Troubleshooting old configuration
- [ ] Understanding why changes may not appear immediately

### Important Flow

```text
Configuration
      ↓
Deployment
      ↓
Stage
      ↓
Live API
```

---

# 🟢 Part 4 — Authentication & API Access

## 12. Authentication vs Authorization

This is the foundation before Cognito.

### Authentication

- [ ] What authentication means
- [ ] Identity
- [ ] "Who are you?"

### Authorization

- [ ] What authorization means
- [ ] Permissions
- [ ] "Are you allowed to do this?"

### Comparison

```text
Authentication → Who are you?

Authorization → Are you allowed to do this?
```

### Real-World Examples

- [ ] Login
- [ ] Accessing protected resources
- [ ] User permissions
- [ ] API access

## 13. Cognito + JWT

### Amazon Cognito

- [ ] What Cognito is
- [ ] User Pool
- [ ] User
- [ ] App client
- [ ] Authentication flow

### JWT

- [ ] What JWT is
- [ ] Why JWT is used
- [ ] JWT structure
- [ ] Header
- [ ] Payload
- [ ] Signature
- [ ] Claims
- [ ] Access token
- [ ] ID token
- [ ] Token expiration

### API Gateway Authorizer

- [ ] What an authorizer does
- [ ] Connecting Cognito to API Gateway
- [ ] Protecting methods
- [ ] Authorization header
- [ ] Bearer token
- [ ] Protected endpoints

### Testing

- [ ] No token
- [ ] Invalid token
- [ ] Expired token
- [ ] Valid token
- [ ] Troubleshooting authorization failures

### Basic Flow

```text
User
 ↓
Cognito
 ↓
JWT
 ↓
API Gateway
 ↓
Authorizer
 ↓
Backend
```

## 14. API Keys

- [ ] What an API key is
- [ ] Why API keys exist
- [ ] Creating an API key
- [ ] `x-api-key`
- [ ] Requiring an API key
- [ ] API key + API Gateway method
- [ ] API key validation
- [ ] API key vs JWT
- [ ] What API keys should NOT be used for
- [ ] Testing API keys
- [ ] Troubleshooting API key issues

### Example

```http
x-api-key: <API_KEY>
```

### Important Comparison

```text
API Key → Identifies/controls an API consumer

JWT → Identifies/authenticates a user and can carry authorization claims
```

## 15. Usage Plans

- [ ] What a usage plan is
- [ ] Why usage plans exist
- [ ] API keys + usage plans
- [ ] Associating APIs with usage plans
- [ ] Associating stages
- [ ] Consumer-level controls
- [ ] Throttling
- [ ] Burst
- [ ] Quotas
- [ ] Testing usage plans
- [ ] Troubleshooting usage plan issues

### Relationship

```text
API Key
   ↓
Usage Plan
   ↓
API + Stage
   ↓
Throttling / Quota
```

## 16. Throttling vs Quota

This will be a dedicated concept because it is easy to confuse them.

### Throttling

- [ ] What throttling means
- [ ] Request rate
- [ ] Burst
- [ ] Request limiting
- [ ] `429 Too Many Requests`
- [ ] Real-world example
- [ ] Testing throttling
- [ ] Troubleshooting throttling

### Quota

- [ ] What quota means
- [ ] Usage limit
- [ ] Time period
- [ ] Request limit
- [ ] Real-world example
- [ ] Testing quota

### Comparison

```text
Throttling → How fast can I send requests?

Quota → How much can I use during a period?
```

---

# 🟢 Part 5 — CORS

## 17. CORS

- [ ] What CORS means
- [ ] Why CORS exists
- [ ] Same-origin concept
- [ ] Why browsers enforce CORS
- [ ] Allowed origins
- [ ] Allowed methods
- [ ] Allowed headers
- [ ] Credentials
- [ ] Preflight
- [ ] OPTIONS
- [ ] Actual request
- [ ] CORS response headers
- [ ] API Gateway CORS configuration
- [ ] Testing CORS
- [ ] Common CORS problems
- [ ] Troubleshooting CORS

### Browser Flow

```text
Browser
   ↓
OPTIONS
   ↓
API Gateway
   ↓
CORS response
   ↓
Browser
   ↓
POST / GET / PATCH / etc.
```

---

# 🟢 Part 6 — Custom Domain & HTTPS

## 18. ACM

### AWS Certificate Manager

- [ ] What ACM is
- [ ] Why ACM is used
- [ ] TLS/SSL certificate
- [ ] HTTPS
- [ ] Public certificate
- [ ] Certificate request
- [ ] DNS validation
- [ ] Certificate validation
- [ ] Certificate lifecycle
- [ ] Certificate expiration
- [ ] Certificate renewal
- [ ] Certificate + API Gateway
- [ ] Troubleshooting certificate issues

## 19. API Gateway Custom Domain

- [ ] Why custom domains exist
- [ ] Default API Gateway URL
- [ ] Custom URL
- [ ] Regional custom domain
- [ ] Custom domain configuration
- [ ] Certificate association
- [ ] API Gateway custom-domain flow
- [ ] Testing the custom domain
- [ ] Troubleshooting custom domains

### Example

```text
Default:

https://abc123.execute-api.region.amazonaws.com/dev

Custom:

https://api.example.com
```

## 20. API Mapping

This deserves its own section because it was directly relevant to our troubleshooting.

- [ ] What API mapping is
- [ ] Why API mapping exists
- [ ] API mapping → API
- [ ] API mapping → Stage
- [ ] Mapping path
- [ ] Default mapping
- [ ] Custom domain routing
- [ ] How API Gateway determines which API receives the request
- [ ] Troubleshooting incorrect API mappings
- [ ] Understanding why a custom domain can reach the wrong API

### Basic Flow

```text
api.example.com
       ↓
   API Mapping
       ↓
   API + Stage
```

## 21. Route 53

### DNS Fundamentals

- [ ] What Route 53 is
- [ ] What DNS is
- [ ] Domain names
- [ ] DNS resolution
- [ ] Hosted zone

### DNS Records

- [ ] DNS records
- [ ] A record
- [ ] AAAA record
- [ ] Alias record
- [ ] API Gateway custom-domain target
- [ ] DNS resolution
- [ ] Route 53 → API Gateway
- [ ] Testing DNS
- [ ] Troubleshooting DNS issues

## 22. Complete Custom Domain Flow

Combine everything we learned:

```text
User
 ↓
Route 53
 ↓
api.example.com
 ↓
API Gateway Custom Domain
 ↓
ACM Certificate
 ↓
API Mapping
 ↓
API
 ↓
Stage
 ↓
Resource
 ↓
Method
 ↓
Integration
 ↓
Backend
```

We will explain **every step of this flow**.

---

# 🟢 Part 7 — WAF with API Gateway

> We are NOT learning all of AWS WAF here.
>
> We will only learn the WAF concepts required to understand and protect our API Gateway setup.
>
> WAF will be studied in depth later as a separate AWS service.

## 23. What is WAF?

- [ ] What AWS WAF is
- [ ] Why WAF exists
- [ ] Web ACL
- [ ] Rules
- [ ] Rule evaluation
- [ ] Allow
- [ ] Block
- [ ] WAF + API Gateway
- [ ] Associating WAF with API Gateway
- [ ] Basic WAF troubleshooting

### Basic Flow

```text
Client
  ↓
WAF
  ↓
ALLOW ─────→ API Gateway
  │
  └── BLOCK → 403
```

## 24. Rate-Based Rule

- [ ] What a rate-based rule is
- [ ] Why it exists
- [ ] Request counting
- [ ] Threshold
- [ ] Evaluation window
- [ ] Source IP aggregation
- [ ] Block action
- [ ] Testing rate-based rules
- [ ] Understanding false expectations during testing
- [ ] Troubleshooting rate-based rules

## 25. WAF + API Gateway

- [ ] Creating a Web ACL
- [ ] Creating a rule
- [ ] Configuring a rate-based rule
- [ ] Associating WAF with API Gateway
- [ ] Testing the rule
- [ ] Understanding allowed requests
- [ ] Understanding blocked requests
- [ ] WAF dashboard
- [ ] Sampled requests

### Architecture

```text
Client
  ↓
WAF
  ↓
API Gateway
  ↓
Backend
```

## 26. WAF Troubleshooting

This section will include the actual problem we encountered during our lab:

> "I tested one URL and got 200, but WAF wasn't showing anything."

We will learn how to determine whether:

- [ ] We are hitting the correct API
- [ ] The request is going to the correct URL
- [ ] Route 53 points to the correct domain
- [ ] The custom domain maps to the correct API
- [ ] The custom domain maps to the correct stage
- [ ] WAF is associated with that API
- [ ] Traffic is actually reaching the protected resource
- [ ] WAF dashboard is showing the expected traffic
- [ ] Sampled requests contain the expected requests

### Important Troubleshooting Question

> **"Am I actually sending the request to the API that has the WAF attached?"**

---

# 🟢 Part 8 — Monitoring

> CloudWatch will be learned as a separate AWS service.
>
> Here we only cover the API Gateway monitoring concepts required to understand API Gateway.

## 27. API Gateway Monitoring

- [ ] Why monitoring matters
- [ ] API Gateway metrics
- [ ] Request count
- [ ] 4XX errors
- [ ] 5XX errors
- [ ] Latency
- [ ] Integration latency
- [ ] Throttling
- [ ] Where to find API Gateway metrics
- [ ] Choosing the correct API
- [ ] Choosing the correct stage
- [ ] Choosing the correct time range
- [ ] Why metrics may take time to appear
- [ ] Basic troubleshooting using metrics

### Important Metrics

```text
Count
4XXError
5XXError
Latency
IntegrationLatency
ThrottledRequests
```

---

# 🟢 Part 9 — Architecture

## 28. API Gateway Final Architecture

This will be created **after completing all the concepts above**.

### Final Architecture

```text
                         ┌──────────────┐
                         │    Client    │
                         └──────┬───────┘
                                │
                                ▼
                         ┌──────────────┐
                         │   Route 53   │
                         └──────┬───────┘
                                │
                                ▼
                    ┌──────────────────────┐
                    │ API Gateway Domain   │
                    │ + ACM Certificate    │
                    └──────────┬───────────┘
                               │
                               ▼
                         ┌───────────┐
                         │    WAF    │
                         └─────┬─────┘
                               │
                               ▼
                    ┌──────────────────┐
                    │   API Gateway    │
                    │                  │
                    │ Cognito / JWT    │
                    │ API Key          │
                    │ Usage Plan       │
                    │ Throttling       │
                    │ CORS             │
                    └────────┬─────────┘
                             │
                             ▼
                       ┌────────────┐
                       │ Integration│
                       └─────┬──────┘
                             │
                             ▼
                         Backend
                             │
                             ▼
                       CloudWatch
```

We will explain:

- [ ] Every component
- [ ] Why each component exists
- [ ] How the components connect
- [ ] Complete request flow
- [ ] Authentication flow
- [ ] Authorization flow
- [ ] API key flow
- [ ] Usage plan flow
- [ ] WAF flow
- [ ] DNS flow
- [ ] HTTPS flow
- [ ] Monitoring flow
- [ ] Possible failure points
- [ ] Troubleshooting flow

---

# 🟢 Part 10 — Production-Style API Gateway Project

This is the final practical goal.

We will eventually take the application we deployed and evolve it toward a production-style architecture.

> **Important:**
>
> We will NOT consider an AWS service "learned" simply because it appears in this architecture.
>
> Each AWS service will be learned separately before being added to the final architecture.

### Target Architecture

```text
                    Route 53
                       │
                       ▼
                 Custom Domain
                       │
                    ACM HTTPS
                       │
                       ▼
                      WAF
                       │
                       ▼
                 API Gateway
                  /                      Cognito       API Key
                │              │
                └──────┬───────┘
                       │
                   Backend
                       │
                    Database
```

Eventually this architecture may expand as we learn additional AWS services.

For example:

```text
Route 53
    ↓
ACM
    ↓
WAF
    ↓
API Gateway
    ↓
Authentication / Authorization
    ↓
Backend
    ↓
Database
```

Additional AWS services such as:

- Lambda
- DynamoDB
- S3
- CloudFront
- etc.

will only be added after we properly learn and implement them.

---

# 📝 Learning Status

| # | Topic | Status |
|---|---|---|
| 1 | What is API Gateway? | ⬜ |
| 2 | APIs, Resources, Methods & Routes | ⬜ |
| 3 | GET | ⬜ |
| 4 | POST | ⬜ |
| 5 | PUT | ⬜ |
| 6 | PATCH | ⬜ |
| 7 | DELETE | ⬜ |
| 8 | OPTIONS & CORS | ⬜ |
| 9 | Integrations | ⬜ |
| 10 | Stages | ⬜ |
| 11 | Deployments | ⬜ |
| 12 | Authentication vs Authorization | ⬜ |
| 13 | Cognito + JWT | ⬜ |
| 14 | API Keys | ⬜ |
| 15 | Usage Plans | ⬜ |
| 16 | Throttling vs Quota | ⬜ |
| 17 | CORS | ⬜ |
| 18 | ACM | ⬜ |
| 19 | API Gateway Custom Domain | ⬜ |
| 20 | API Mapping | ⬜ |
| 21 | Route 53 | ⬜ |
| 22 | Complete Custom Domain Flow | ⬜ |
| 23 | What is WAF? | ⬜ |
| 24 | Rate-Based Rule | ⬜ |
| 25 | WAF + API Gateway | ⬜ |
| 26 | WAF Troubleshooting | ⬜ |
| 27 | API Gateway Monitoring | ⬜ |
| 28 | API Gateway Final Architecture | ⬜ |
| 29 | Production-Style API Gateway Project | ⬜ |

---

# 🎯 Final Goal

By the end of this learning path, I should be able to:

- [ ] Explain API Gateway from scratch
- [ ] Explain what an API is
- [ ] Design API resources and routes
- [ ] Understand resources and methods
- [ ] Understand GET
- [ ] Understand POST
- [ ] Understand PUT
- [ ] Understand PATCH
- [ ] Understand DELETE
- [ ] Understand OPTIONS
- [ ] Understand path parameters
- [ ] Understand query parameters
- [ ] Understand headers
- [ ] Understand request and response bodies
- [ ] Understand HTTP status codes
- [ ] Connect API Gateway to a backend
- [ ] Understand integrations
- [ ] Understand stages
- [ ] Understand deployments
- [ ] Understand authentication
- [ ] Understand authorization
- [ ] Secure APIs using Cognito/JWT
- [ ] Understand API keys
- [ ] Use API keys correctly
- [ ] Configure usage plans
- [ ] Understand throttling
- [ ] Understand quotas
- [ ] Configure CORS
- [ ] Understand OPTIONS/preflight
- [ ] Configure HTTPS using ACM
- [ ] Configure a custom API Gateway domain
- [ ] Understand API mappings
- [ ] Connect Route 53 to API Gateway
- [ ] Understand the complete DNS → API Gateway flow
- [ ] Protect an API using WAF
- [ ] Configure rate-based WAF rules
- [ ] Understand WAF sampled requests
- [ ] Troubleshoot WAF/API Gateway routing issues
- [ ] Understand API Gateway monitoring
- [ ] Understand important API Gateway CloudWatch metrics
- [ ] Draw the complete API Gateway architecture
- [ ] Explain every component in the architecture
- [ ] Implement a production-style API Gateway setup

---

# 🔄 Documentation Process

For each concept, we will follow this process:

```text
Learn Concept
      ↓
Understand Theory
      ↓
Understand Why It Exists
      ↓
Understand How It Works
      ↓
Real-World Example
      ↓
Implement in AWS
      ↓
Test
      ↓
Troubleshoot
      ↓
Write Documentation
      ↓
Review Together
      ↓
Approve
      ↓
Commit to GitHub
      ↓
Move to Next Concept
```

> **Rule:** A concept is not considered complete until it is understood, implemented where appropriate, tested, documented, and reviewed.
