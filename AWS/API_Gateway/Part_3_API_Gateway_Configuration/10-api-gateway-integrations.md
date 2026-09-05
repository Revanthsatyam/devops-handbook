# Concept 1: Integrations

An **integration** is the connection between **API Gateway and the backend service** that actually handles the request.

---

## 1. Why do we need an Integration?

API Gateway receives the request, but it usually doesn't perform the application's business logic itself.

For example:

```text
Client
  |
  | GET /users/101
  v
API Gateway
  |
  | Integration
  v
Lambda
  |
  | Process request
  v
Response
  |
  v
API Gateway
  |
  v
Client
```

The integration tells API Gateway **where to send the request**.

---

## 2. Common Integrations

API Gateway can integrate with different backend services.

For our learning, the important ones are:

| Integration | Backend |
|---|---|
| Lambda | AWS Lambda function |
| HTTP | HTTP endpoint |
| AWS service | Other AWS services |

The most common pattern we'll use is:

```text
API Gateway → Lambda
```

---

## 3. Lambda Integration

Suppose we have:

```text
GET /users/{id}
```

and a Lambda function called:

```text
get-user
```

The integration connects them:

```text
GET /users/{id}
       |
       v
API Gateway
       |
       | Integration
       v
get-user Lambda
```

When the client requests:

```text
GET /users/101
```

API Gateway sends the request information to Lambda.

Lambda processes it and returns a response.

---

## 4. Integration vs Route

These are different concepts:

```text
Route
  ↓
"What request should API Gateway handle?"

Integration
  ↓
"Where should API Gateway send that request?"
```

Example:

```text
GET /users/{id}
       |
       | Route
       v
API Gateway
       |
       | Integration
       v
Lambda: get-user
```

---

## Key Takeaways

- **Integration connects API Gateway to a backend.**
- API Gateway receives the request.
- The integration determines where the request goes.
- Lambda is one of the most common integrations.
- **Route = what request?**
- **Integration = where does it go?**
