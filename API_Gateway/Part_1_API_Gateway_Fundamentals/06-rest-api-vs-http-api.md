# Concept 6: REST API vs HTTP API

API Gateway provides two main API types: **REST API** and **HTTP API**.

---

## 1. REST API

REST API is the **feature-rich** API Gateway option.

It supports features such as:

- API Keys
- Usage Plans
- Advanced request/response transformations
- More detailed API Gateway configuration
- Resource-based routing

Example:

```text
Client
  |
  v
REST API
  |
  +--> /users
  +--> /products
  +--> /orders
  |
  v
Backend
```

Use REST API when you need its additional features and configuration capabilities.

---

## 2. HTTP API

HTTP API is the **simpler and generally lower-cost** option.

It is designed for common API use cases such as:

- Lambda-backed APIs
- HTTP backends
- JWT authentication
- Simple routing
- CORS

Example:

```text
Client
  |
  v
HTTP API
  |
  +--> GET /users
  +--> POST /users
  |
  v
Lambda / Backend
```

---

## 3. Main Difference

| Feature | REST API | HTTP API |
|---|---|---|
| Simplicity | More configuration | Simpler |
| Cost | Higher | Lower |
| JWT Authorizer | Supported | Supported |
| API Keys | Supported | Not supported in the same way |
| Usage Plans | Supported | Not supported |
| Request/Response transformation | Advanced | More limited |
| CORS | Supported | Supported |
| Typical use | Feature-rich APIs | Simple/modern APIs |

---

## 4. Which One Should You Choose?

A simple way to remember:

```text
Need simple API
      ↓
   HTTP API
```

```text
Need advanced API Gateway features
      ↓
    REST API
```

For example, if you're building a simple Lambda API with Cognito/JWT authentication:

```text
Client
   ↓
HTTP API
   ↓
JWT Authorizer
   ↓
Lambda
```

HTTP API is usually a good fit.

If you need **API Keys + Usage Plans**:

```text
Client
   ↓
REST API
   ↓
API Key / Usage Plan
   ↓
Backend
```

REST API is the appropriate choice.

---

## 5. Important Mental Model

Don't think:

> REST API = old, HTTP API = new

Instead think:

> **HTTP API = simpler, lower-cost API Gateway option**

> **REST API = more feature-rich API Gateway option**

Both can expose HTTP endpoints and connect clients to backend services.

---

## Key Takeaways

- API Gateway has **REST APIs** and **HTTP APIs**.
- HTTP API is simpler and generally cheaper.
- REST API provides more advanced API Gateway features.
- JWT authentication is supported by HTTP APIs.
- API Keys and Usage Plans are important REST API features.
- Choose based on the **features your application needs**, not simply because one is newer.
