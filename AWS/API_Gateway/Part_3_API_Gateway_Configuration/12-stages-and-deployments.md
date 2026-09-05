# Concept 3: Stages & Deployments

Stages and deployments are closely connected concepts in API Gateway.

---

## 1. What is a Stage?

A **stage** is a named environment where a deployed API is available.

Common examples:

```text
dev
test
prod
```

You can have:

```text
API
 |
 +-- dev
 +-- test
 +-- prod
```

Each stage can represent a different environment.

---

## 2. Why Use Stages?

Stages let you maintain different versions/environments of an API.

For example:

```text
Development
    ↓
dev stage

Testing
    ↓
test stage

Production
    ↓
prod stage
```

This helps you test changes before exposing them to production users.

---

## 3. What is a Deployment?

A **deployment** publishes your API configuration so that it becomes available through a stage.

Think:

```text
Make API changes
       ↓
Create Deployment
       ↓
Deploy to Stage
       ↓
API becomes available
```

For **REST APIs**, changes generally need to be deployed to a stage before clients can use them.

---

## 4. Simple Example

Suppose you have:

```text
GET /users
```

You make a change to the API.

The flow is:

```text
API Configuration
       ↓
Deployment
       ↓
dev Stage
       ↓
Live API
```

Later, after testing:

```text
Deployment
       ↓
prod Stage
       ↓
Production API
```

---

## 5. Stage + Deployment Relationship

A useful mental model:

> **Stage = where the API is exposed**

> **Deployment = the published version of the API configuration**

```text
API
 |
 | Deployment
 v
Stage
 |
 v
Live Endpoint
```

---

## Key Takeaways

- A **stage** represents an API environment such as `dev`, `test`, or `prod`.
- A **deployment** publishes API configuration.
- REST API changes generally need to be deployed before becoming live.
- Different stages can be used for different environments.
- Think **Deployment → Stage → Live API**.
