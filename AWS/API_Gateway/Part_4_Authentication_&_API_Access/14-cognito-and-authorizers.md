# Part 4 — Concept 2: Cognito + Authorizers

## 1. Cognito's Role

Amazon Cognito can authenticate users and issue tokens.

```text
User
  |
  | Login
  v
Cognito
  |
  | JWT
  v
Client
```

The client then sends the JWT with API requests:

```http
Authorization: Bearer <JWT>
```

---

## 2. What is an Authorizer?

An **Authorizer** is the security layer in API Gateway that decides whether a request is allowed to reach the backend.

```text
Client
  |
  | Request + JWT
  v
API Gateway
  |
  | Authorizer
  | "Is this token valid?"
  |
  +---- No ----> 401
  |
  +---- Yes ---> Lambda
```

---

## 3. JWT Authorizer

For an **HTTP API**, API Gateway can use a **JWT authorizer**.

You configure it with:

```text
Issuer
  ↓
Cognito User Pool

Audience
  ↓
Cognito App Client ID

Identity Source
  ↓
Authorization header
```

The flow:

```text
Cognito
   |
   | Issues JWT
   v
Client
   |
   | Authorization: Bearer JWT
   v
API Gateway
   |
   | JWT Authorizer
   | Validate issuer
   | Validate audience
   | Validate signature
   | Check expiration
   v
Lambda
```

---

## 4. Protecting a Route

You can attach the authorizer to specific routes.

For example:

```text
GET /users
    ↓
JWT Authorizer
    ↓
Lambda
```

But another route could remain public:

```text
GET /health
    ↓
No Authorizer
    ↓
Lambda
```

So authentication does not necessarily have to apply to every route.

---

## Key Takeaways

- **Cognito** authenticates users and issues tokens.
- **Authorizer** protects API Gateway routes.
- A **JWT authorizer** validates JWTs before allowing access.
- The JWT is normally sent in the `Authorization` header.
- Invalid/missing tokens result in **401 Unauthorized**.
- Authorizers can be attached to individual routes.
