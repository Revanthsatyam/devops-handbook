# Part 8 — API Gateway Monitoring

API Gateway provides metrics that help us understand **API traffic, errors, latency, and throttling**.

> CloudWatch itself will be learned separately. This document only covers API Gateway monitoring.

---

## 1. Monitoring Flow

```text
Client
   |
   v
API Gateway
   |
   v
Backend
   |
   v
CloudWatch Metrics
```

API Gateway publishes metrics that can be viewed in CloudWatch.

---

## 2. Important API Gateway Metrics

| Metric | What it tells you |
|---|---|
| `Count` | Number of API requests |
| `4XXError` | Number of client-side errors |
| `5XXError` | Number of server/backend errors |
| `Latency` | Total time taken for the API request |
| `IntegrationLatency` | Time spent waiting for the backend integration |
| `ThrottledRequests` | Number of requests that were throttled |

---

## 3. Count

`Count` shows how many requests the API received.

```text
More requests
     ↓
Higher Count
```

Useful for understanding API traffic.

---

## 4. 4XX Errors

`4XXError` indicates errors caused by the request/client side.

Examples:

```text
400 → Bad Request
401 → Unauthorized
403 → Forbidden
404 → Not Found
```

Useful for identifying invalid requests or authentication/access problems.

---

## 5. 5XX Errors

`5XXError` indicates server-side or backend problems.

Examples:

```text
500 → Internal Server Error
502 → Bad Gateway
503 → Service Unavailable
```

Useful for identifying backend or integration failures.

---

## 6. Latency vs Integration Latency

### Latency

The overall time taken to process the API request.

```text
Client
  |
  |----------------------|
  |       Latency        |
  |----------------------|
  v
API Response
```

### IntegrationLatency

The time API Gateway spends waiting for the backend integration.

```text
API Gateway
     |
     |---- IntegrationLatency ----|
     v
   Lambda
```

A useful way to think about it:

```text
Latency
  ↓
Overall API request time

IntegrationLatency
  ↓
Backend integration time
```

---

## 7. ThrottledRequests

Shows requests that were throttled by API Gateway.

```text
Client
  |
  | Too many requests
  v
API Gateway
  |
  | Throttling
  v
Request rejected
```

This can help identify traffic exceeding configured limits.

---

## 8. Basic Troubleshooting

Metrics can help narrow down API problems.

```text
High 4XX
   ↓
Check client request,
route, authentication, etc.
```

```text
High 5XX
   ↓
Check backend/integration
```

```text
High Latency
   ↓
Check overall API performance
```

```text
High IntegrationLatency
   ↓
Check backend performance
```

```text
High ThrottledRequests
   ↓
Check throttling configuration
and request traffic
```

---

## Key Takeaways

- API Gateway metrics are available through CloudWatch.
- `Count` → request volume.
- `4XXError` → client/request errors.
- `5XXError` → server/backend errors.
- `Latency` → overall request time.
- `IntegrationLatency` → backend integration time.
- `ThrottledRequests` → throttled traffic.
- These metrics are useful for basic API Gateway monitoring and troubleshooting.
