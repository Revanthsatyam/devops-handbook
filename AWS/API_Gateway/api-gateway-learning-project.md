# Amazon API Gateway — Learning Project

## 1. Project Overview

This project brings together the API Gateway features we implemented while learning the service.

The goal is that, by reading this document, you should be able to **recreate the project from scratch** and understand why each component is configured.

### Architecture

```text
                         ┌──────────────┐
                         │    Client    │
                         │ Browser/Postman
                         └──────┬───────┘
                                │
                                ▼
                         ┌──────────────┐
                         │   Route 53   │
                         │     DNS      │
                         └──────┬───────┘
                                │
                                ▼
                  ┌──────────────────────────┐
                  │ API Gateway Custom Domain│
                  │       + HTTPS            │
                  └────────────┬─────────────┘
                               │
                               ▼
                  ┌──────────────────────────┐
                  │      API Gateway         │
                  │ Routes / Methods / Stage │
                  └────────────┬─────────────┘
                               │
                    ┌──────────┴──────────┐
                    │                     │
                    ▼                     ▼
                   WAF              JWT Authorizer
                                         │
                                      Cognito
                                         │
                                         ▼
                                      Lambda
                                         │
                                         ▼
                                   API Response

                         CloudWatch
                       API Monitoring
```

> Note: WAF is attached to the API Gateway resource and evaluates protected web requests. The diagram is a learning model of the request path, not a claim about the exact internal AWS network hop.

---

# 2. Components Used

| Component | Purpose |
|---|---|
| API Gateway | Exposes and manages the API |
| Lambda | Backend logic |
| Cognito User Pool | User authentication |
| JWT Authorizer | Validates Cognito JWTs |
| API Keys | Identifies API clients |
| Usage Plans | Controls API key usage |
| WAF | Protects the API from unwanted/high-rate requests |
| ACM | Provides the HTTPS certificate |
| API Gateway Custom Domain | Provides a friendly HTTPS hostname |
| Route 53 | DNS for the custom hostname |
| CloudWatch | API Gateway monitoring |

---

# 3. Step 1 — Create the Lambda Backend

Create a Lambda function that will act as the API backend.

Example Python function:

```python
def lambda_handler(event, context):
    return {
        "statusCode": 200,
        "body": "Hello from Lambda"
    }
```

### Important

The handler must be configured correctly.

For a file named `lambda_function.py` with a function named `lambda_handler`:

```text
lambda_function.lambda_handler
```

Deploy/save the function after changing the code.

---

# 4. Step 2 — Create the API Gateway API

Create an API Gateway API.

For this project, we worked with both **HTTP API** and **REST API** concepts.

When recreating a specific configuration, use the API type required by the feature you are implementing.

### Create a route

A route combines:

```text
HTTP Method + Path
```

Example:

```text
GET /users/{id}
```

Here:

- `GET` = HTTP method
- `/users/{id}` = resource/path
- `{id}` = path parameter

A request such as:

```text
GET /users/101
```

matches the route.

A different method such as:

```text
POST /users/101
```

does not match a `GET` route.

---

# 5. Step 3 — Connect API Gateway to Lambda

Configure the API Gateway integration to invoke the Lambda function.

The basic flow becomes:

```text
Client
   ↓
API Gateway Route
   ↓
Lambda Integration
   ↓
Lambda Function
   ↓
Response
```

Test the API before adding authentication or other security layers.

If the API returns an unexpected error, first check:

1. Route and HTTP method
2. Integration configuration
3. Lambda code
4. Lambda handler
5. Lambda deployment
6. CloudWatch logs

---

# 6. Step 4 — Configure CORS

If a browser-based frontend will call the API from another origin, configure CORS.

For local development, our frontend used:

```text
http://localhost:3000
```

Configure the required:

- Allowed origin
- Allowed methods
- Allowed headers

The `Authorization` header must be allowed when the browser sends a JWT.

The basic browser flow is:

```text
Browser
   ↓
Preflight / CORS check
   ↓
API Gateway
   ↓
Actual API request
```

---

# 7. Step 5 — Create a Cognito User Pool

For JWT-based API authentication, use a **Cognito User Pool**.

Do not confuse this with a Cognito Identity Pool.

Create:

```text
Cognito User Pool
        ↓
App Client
```

The app client is used by the application during authentication.

For a public client, the app client does not have a client secret.

---

# 8. Step 6 — Configure Cognito Authentication

Create a test user in the User Pool.

For OAuth-based testing, configure the Cognito hosted authentication flow and callback URL.

The authorization-code flow is:

```text
User
 ↓
Cognito Login
 ↓
Authorization Code
 ↓
Client
 ↓
Token Endpoint
 ↓
Access Token / ID Token
```

The Cognito token endpoint is on the Cognito **authentication domain**:

```text
https://<cognito-domain>/oauth2/token
```

It is not the Cognito User Pool issuer URL.

---

# 9. Step 7 — Create the JWT Authorizer

Create a JWT authorizer in API Gateway.

For Cognito, the issuer follows:

```text
https://cognito-idp.<region>.amazonaws.com/<user-pool-id>
```

Configure the audience as the Cognito **app client ID**.

The identity source is:

```text
$request.header.Authorization
```

The client sends:

```text
Authorization: Bearer <JWT>
```

API Gateway validates the JWT before allowing the request to reach Lambda.

---

# 10. Step 8 — Protect the Route

Attach the JWT authorizer to the route that requires authentication.

The protected flow becomes:

```text
Client
  ↓
Authorization: Bearer <JWT>
  ↓
API Gateway
  ↓
JWT Authorizer
  ↓
Valid?
 ├── No  → Reject request
 └── Yes → Lambda
```

Test both cases:

### Without token

```text
GET /protected-route
```

Expected result:

```text
Unauthorized
```

### With valid token

```text
Authorization: Bearer <valid-token>
```

Expected result:

```text
Request reaches Lambda
```

---

# 11. Step 9 — API Keys and Usage Plans

API keys are different from Cognito authentication.

Think of them as:

```text
Cognito/JWT
→ Who is the user?

API Key
→ Which client/application is making the request?
```

For a REST API, the basic setup is:

```text
API Key
   ↓
Usage Plan
   ↓
API Stage
   ↓
API Method
```

A usage plan can define:

- Throttling
- Quotas
- Associated API stages
- Associated API keys

### Testing

Send the API key using the configured header:

```text
x-api-key: <api-key>
```

Do not treat an API key as a replacement for user authentication.

---

# 12. Step 10 — Configure a Custom Domain

Instead of exposing the default API Gateway hostname, create a custom domain.

Example:

```text
api.example.com
```

The high-level setup is:

```text
ACM Certificate
      ↓
API Gateway Custom Domain
      ↓
API Mapping
      ↓
API + Stage
```

### Important

The API mapping determines which API and stage receive requests for the custom domain.

If the mapping points to the wrong API or stage, the custom domain can successfully resolve but still send traffic to the wrong API.

---

# 13. Step 11 — ACM Certificate

Request an ACM public certificate for the custom hostname.

Example:

```text
api.example.com
```

Complete certificate validation, typically using DNS validation.

Then select the certificate when configuring the API Gateway **custom domain**.

### Important distinction

The ACM certificate used for a custom domain is different from an API Gateway stage **client certificate**.

The custom-domain certificate provides normal server-side HTTPS for clients.

---

# 14. Step 12 — Configure Route 53

Create the DNS record for the API hostname.

Conceptually:

```text
api.example.com
       ↓
Route 53
       ↓
API Gateway Custom Domain
```

After DNS propagation, the API can be accessed through the custom hostname.

Example:

```text
https://api.example.com/...
```

---

# 15. Step 13 — Attach AWS WAF

Create a WAF Web ACL and associate it with the correct API Gateway resource/stage.

Example Web ACL:

```text
api-gateway-waf
```

A simple rate-based rule can be used for testing.

Example:

```text
Rate-based rule
Limit: 10 requests
```

The exact enforcement is not an exact request counter. WAF rate-based rules use an evaluation window and are designed for high-rate mitigation.

### WAF flow

```text
Client Request
      ↓
WAF evaluates rules
      ↓
Allowed → API Gateway processing
Blocked → Request rejected
```

### Important

Make sure WAF is associated with the **same API Gateway API/stage that receives the traffic**.

For example, if your custom-domain API mapping points to API A but WAF is associated with API B, requests through the custom domain will not be protected by the WAF association on API B.

---

# 16. Step 14 — Monitor the API

API Gateway metrics are available in CloudWatch.

Useful metrics include:

| Metric | Meaning |
|---|---|
| Count | Number of requests |
| 4XXError | Client-side/API request errors |
| 5XXError | Server-side/integration errors |
| Latency | Overall API latency |
| IntegrationLatency | Backend integration latency |
| ThrottledRequests | Requests affected by throttling |

Use these metrics when troubleshooting the API.

CloudWatch itself is covered separately as an AWS service.

---

# 17. Complete Testing Checklist

After building the project, test it layer by layer.

### Basic API

```text
□ API Gateway route works
□ Correct HTTP method works
□ Incorrect method/path is rejected
□ Lambda returns the expected response
```

### CORS

```text
□ Browser frontend can call the API
□ Correct origin is allowed
□ Authorization header is allowed
```

### Authentication

```text
□ Cognito user can authenticate
□ Authorization code is returned
□ Code can be exchanged for tokens
□ Valid JWT reaches the protected route
□ Missing/invalid JWT is rejected
```

### API Keys

```text
□ API key is accepted where required
□ Usage plan is associated correctly
□ Throttling/quota configuration is applied
```

### Custom Domain

```text
□ ACM certificate is validated
□ Custom domain is configured
□ API mapping points to the correct API/stage
□ Route 53 record points to the custom domain
□ HTTPS request works
```

### WAF

```text
□ Web ACL is associated with the correct API/stage
□ WAF rule is enabled
□ Requests can be evaluated by the rule
```

### Monitoring

```text
□ API Gateway metrics appear in CloudWatch
□ 4XX/5XX errors can be identified
□ Latency can be checked
□ Throttling can be investigated
```

---

# 18. Troubleshooting Guide

When something fails, troubleshoot from the outside toward the backend.

```text
Client
  ↓
DNS
  ↓
Custom Domain
  ↓
HTTPS
  ↓
API Mapping / Stage
  ↓
Route / Method
  ↓
WAF
  ↓
JWT / API Key
  ↓
Integration
  ↓
Lambda
  ↓
CloudWatch
```

### Route problem

Check:

```text
HTTP method
Path
Path parameters
Stage
```

For example:

```text
GET /users/{id}
```

does not match:

```text
POST /users/101
```

if only the GET route exists.

### Lambda returns 500

Check:

```text
Lambda code
Handler
Indentation
Deployment
Execution role
CloudWatch logs
```

### JWT authentication fails

Check:

```text
Cognito User Pool
App Client
Issuer
Audience / Client ID
Authorization header
Token expiration
```

Remember that Cognito authorization codes are short-lived and single-use.

### Custom domain reaches the wrong API

Check:

```text
Route 53 record
Custom domain
API mapping
API
Stage
```

### WAF appears not to work

Check:

```text
Correct Web ACL
Correct API
Correct stage
Correct Region
WAF rule
Recent sampled requests
```

Also remember that rate-based rules are not precise request counters and may take some time to detect/enforce.

---

# 19. Final Architecture

The complete project can be remembered as:

```text
                         Client
                           │
                           ▼
                       Route 53
                           │
                           ▼
                 Custom Domain + HTTPS
                           │
                           ▼
                     API Gateway
                    /                              /                              WAF          Authentication
                                │
                             Cognito
                                │
                           JWT Authorizer
                                │
                                ▼
                              Lambda
                                │
                                ▼
                            Response

                    CloudWatch Monitoring
```

---

# 20. What This Project Taught Us

By completing this project, we learned how to:

- Create and configure APIs in API Gateway
- Work with routes and HTTP methods
- Connect API Gateway to Lambda
- Handle request parameters and headers
- Configure CORS
- Understand REST API and HTTP API differences
- Deploy APIs using stages
- Authenticate users with Cognito
- Protect routes with JWT authorizers
- Use API keys and usage plans
- Configure throttling and quotas
- Configure custom domains
- Use ACM certificates for HTTPS
- Connect Route 53 DNS to the custom domain
- Protect API Gateway with AWS WAF
- Monitor API Gateway using CloudWatch metrics
- Troubleshoot the complete request path

## Final Mental Model

```text
API Gateway is the front door.

Route 53
    → finds the API

Custom Domain + ACM
    → provides the HTTPS hostname

API Gateway
    → receives and routes requests

CORS
    → controls browser cross-origin access

Cognito + JWT
    → authenticates/authorizes users

API Keys + Usage Plans
    → control client/API usage

WAF
    → protects the API from unwanted/high-rate traffic

Lambda
    → executes backend logic

CloudWatch
    → helps monitor the API
```

This is the **API Gateway project we actually implemented while learning**, rather than a hypothetical production architecture.
