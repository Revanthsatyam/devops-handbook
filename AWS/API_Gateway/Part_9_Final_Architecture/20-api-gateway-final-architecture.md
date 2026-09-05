# Amazon API Gateway — Final Architecture

## 1. Complete API Gateway Architecture

The overall architecture we learned is:

```text
                    ┌───────────────┐
                    │    Client     │
                    │ Browser/Postman│
                    └───────┬───────┘
                            │
                            ▼
                    ┌───────────────┐
                    │   Route 53    │
                    │     DNS       │
                    └───────┬───────┘
                            │
                            ▼
              ┌──────────────────────────┐
              │ API Gateway Custom Domain│
              │        + HTTPS           │
              └────────────┬─────────────┘
                           │
                           ▼
              ┌──────────────────────────┐
              │      API Gateway         │
              │  Routes / Methods / API  │
              └────────────┬─────────────┘
                           │
                    WAF Protection
                           │
                           ▼
              ┌──────────────────────────┐
              │   JWT Authorizer         │
              │      Cognito             │
              └────────────┬─────────────┘
                           │
                           ▼
              ┌──────────────────────────┐
              │         Lambda           │
              │        Backend           │
              └──────────────────────────┘

                     │
                     ▼
              ┌──────────────────────────┐
              │      CloudWatch          │
              │ API Gateway Metrics/Logs │
              └──────────────────────────┘
```

The goal is to understand the responsibility of each layer rather than simply memorize the diagram.

---

## 2. Complete Request Flow

A typical request looks like:

```text
Client
  ↓
Route 53
  ↓
Custom Domain
  ↓
HTTPS
  ↓
API Gateway
  ↓
WAF protection
  ↓
JWT Authorizer
  ↓
Lambda
  ↓
Response
  ↓
Client
```

Each layer has a specific responsibility:

- **Route 53** — DNS resolution
- **Custom Domain** — friendly API hostname
- **HTTPS** — encrypted client connection
- **API Gateway** — API entry point and request routing
- **WAF** — web request protection
- **JWT Authorizer / Cognito** — authentication and authorization checks
- **Lambda** — backend logic
- **CloudWatch** — monitoring and metrics

---

## 3. Authentication Flow

For a protected API:

```text
User
 ↓
Cognito
 ↓
Login
 ↓
JWT Token
 ↓
Client
 ↓
Authorization: Bearer <token>
 ↓
API Gateway
 ↓
JWT Authorizer validates token
 ↓
Lambda
```

The JWT authorizer can validate information such as:

- Token issuer
- Audience / client ID
- Token validity
- Token expiration
- Required authorization information

If authorization fails, the request does not reach the backend.

---

## 4. WAF Flow

AWS WAF provides an additional security layer for the API.

```text
Client Request
      ↓
API Gateway protected by WAF
      ↓
WAF evaluates rules
      ↓
Allowed ───────→ API processing
Blocked ───────→ Request rejected
```

We also tested a **rate-based rule** to understand how WAF can help protect an API from excessive request rates.

---

## 5. DNS Flow

When using a custom domain:

```text
api.example.com
       ↓
Route 53
       ↓
API Gateway Custom Domain
       ↓
API Mapping
       ↓
API + Stage
```

The important relationship is:

**Route 53 → Custom Domain → API Mapping → API Stage**

---

## 6. HTTPS Flow

AWS Certificate Manager (ACM) provides the certificate used by the API Gateway custom domain.

```text
ACM Certificate
      ↓
API Gateway Custom Domain
      ↓
HTTPS
      ↓
https://api.example.com
```

This allows clients to connect to the API securely over HTTPS.

---

## 7. Monitoring Flow

API Gateway publishes metrics to CloudWatch.

Important API Gateway metrics we learned:

- **Request Count**
- **4XX Errors**
- **5XX Errors**
- **Latency**
- **Integration Latency**
- **Throttled Requests**

These metrics help identify request failures, backend problems, latency, and throttling.

CloudWatch itself will be learned separately as its own AWS service.

---

## 8. Troubleshooting Flow

When an API request fails, use a layered troubleshooting approach:

```text
Request failing?
      │
      ├── DNS / Custom Domain?
      │
      ├── HTTPS / Certificate?
      │
      ├── API Mapping / Stage?
      │
      ├── Route / Method?
      │
      ├── WAF blocking?
      │
      ├── JWT / Cognito authorization?
      │
      ├── API Gateway integration?
      │
      ├── Lambda?
      │
      └── CloudWatch metrics/logs?
```

The idea is to start from the request entry point and move through each layer until the failing component is identified.

---

## Key Takeaways

- API Gateway acts as the API entry point for clients.
- Route 53 handles DNS for the custom API hostname.
- ACM provides the certificate used for the API Gateway custom domain.
- HTTPS secures communication between the client and the API.
- API Gateway handles routes, methods, integrations, and stages.
- WAF adds protection against unwanted or excessive web requests.
- Cognito and JWT authorizers protect APIs that require authentication.
- Lambda can provide the backend logic.
- CloudWatch provides API Gateway monitoring metrics.
- The complete mental model is:

```text
Client
  ↓
DNS
  ↓
Custom Domain + HTTPS
  ↓
API Gateway
  ↓
Security (WAF + Authorization)
  ↓
Backend
  ↓
Monitoring
```
