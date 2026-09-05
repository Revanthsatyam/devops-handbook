# Concept 7: Regional Endpoint

A **Regional endpoint** means your API Gateway API is deployed to a specific AWS Region and clients access the API through an endpoint associated with that Region.

---

## 1. What is a Regional Endpoint?

Suppose your API is deployed in:

```text
ap-south-1
```

API Gateway provides an endpoint associated with that Region.

Conceptually:

```text
Client
  |
  v
API Gateway
(ap-south-1)
  |
  v
Backend
```

The API is primarily served from the selected AWS Region.

---

## 2. Why Does Region Matter?

AWS resources are usually created inside a specific Region.

For example:

```text
API Gateway → ap-south-1
Lambda      → ap-south-1
DynamoDB    → ap-south-1
```

Keeping related services in the same Region can simplify architecture and generally reduces cross-Region communication.

---

## 3. Regional Endpoint vs Custom Domain

A Regional API can also be exposed through your own domain.

Example:

```text
https://api.example.com
```

The flow becomes:

```text
Client
  |
  | https://api.example.com
  v
Route 53
  |
  v
API Gateway Regional Endpoint
  |
  v
Backend
```

This is the pattern used when working with **API Gateway + custom domain + Route 53 + ACM**.

---

## 4. Regional vs Edge-Optimized

For REST APIs, API Gateway supports different endpoint types.

The important distinction here is:

```text
Regional
    ↓
API is served from the selected AWS Region
```

```text
Edge-Optimized
    ↓
API traffic is distributed through CloudFront edge locations
```

A Regional endpoint is useful when you want the API to be accessed directly in its Region or when you are building your own architecture around services such as CloudFront.

---

## 5. Simple Mental Model

```text
Regional API

User
 |
 | Request
 v
AWS Region
 |
 +-- API Gateway
 |
 +-- Lambda
 |
 +-- Other AWS Services
```

Think:

> **Regional endpoint = API Gateway endpoint associated with a specific AWS Region.**

---

## Key Takeaways

- API Gateway APIs are associated with an AWS Region.
- A Regional endpoint serves the API from that Region.
- Your backend services can also be deployed in the same Region.
- Regional APIs can use custom domains.
- Route 53 can point your domain to the API Gateway custom domain.
- For REST APIs, Regional and Edge-Optimized are different endpoint types.

---

## Part 1 Progress

Core API Gateway Fundamentals covered:

1. What is API Gateway?
2. API, Resource, Method and Route
3. Request & Response
4. Path Parameters, Query Parameters and Headers
5. HTTP Status Codes
6. REST API vs HTTP API
7. Regional Endpoint
