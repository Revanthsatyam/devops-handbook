# Part 4 — Concept 3: API Keys + Usage Plans + Throttling + Quotas

These four concepts are connected and are mainly used for **controlling API access and usage**.

---

## 1. API Key

An **API key** is a value sent by the client to identify the application making the request.

Example:

```http
x-api-key: abc123
```

Think:

> **API Key → Which application is calling?**

It is **not a replacement for user authentication** like Cognito/JWT.

---

## 2. Usage Plan

A **Usage Plan** lets you define how an API key can use your API.

Conceptually:

```text
API Key
   |
   v
Usage Plan
   |
   +-- Which API/stage?
   +-- Throttling
   +-- Quota
```

You can associate:

```text
Usage Plan
   ↓
API
   ↓
Stage
   ↓
API Key
```

---

## 3. Throttling

**Throttling controls the request rate.**

Example:

```text
10 requests/second
```

If the client sends requests too quickly, API Gateway can throttle them.

```text
Client
  |
  | Too many requests
  v
API Gateway
  |
  | Throttled
  v
429 Too Many Requests
```

Think:

> **Throttling → How fast can you call?**

---

## 4. Quota

A **quota controls the total number of requests allowed over a period**.

Example:

```text
100,000 requests/month
```

Once the quota is reached, additional requests can be rejected until the quota period resets.

Think:

> **Quota → How many calls can you make during a period?**

---

## Quick Comparison

| Concept | Purpose |
|---|---|
| API Key | Identify the calling application |
| Usage Plan | Define API usage rules |
| Throttling | Control request rate |
| Quota | Control total requests over a period |

### Easy Mental Model

```text
API Key
   ↓
"Who is calling?"

Usage Plan
   ↓
"What rules apply to them?"

Throttling
   ↓
"How fast can they call?"

Quota
   ↓
"How many can they call?"
```

---

## Important Distinction

```text
Cognito + JWT
      ↓
User authentication / authorization

API Key + Usage Plan
      ↓
Application access / usage control
```

They solve **different problems** and can potentially be used together.

---

## Key Takeaways

- API Keys identify applications, not users.
- Usage Plans define how API keys can use an API.
- Throttling limits request rate.
- Quotas limit total usage over a period.
- `429 Too Many Requests` is commonly associated with throttling.
- API Keys/Usage Plans are primarily associated with **API Gateway REST APIs**.
