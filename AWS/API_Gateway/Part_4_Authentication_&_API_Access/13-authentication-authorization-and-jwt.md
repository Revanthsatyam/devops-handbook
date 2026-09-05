# Part 4 — Concept 1: Authentication vs Authorization + JWT

These concepts are closely related.

---

## 1. Authentication

**Authentication = Who are you?**

It verifies the identity of the user/client.

Example:

```text
Client
  |
  | Username + Password
  v
Authentication System
  |
  | Valid
  v
User authenticated
```

Examples:

- Username/password
- Cognito
- JWT
- OAuth 2.0

---

## 2. Authorization

**Authorization = What are you allowed to do?**

After the user is authenticated, the system determines what they can access.

Example:

```text
User authenticated
       |
       v
Is this user allowed
to access /admin?
       |
   +---+---+
   |       |
  Yes      No
   |       |
 Access   403
```

### Easy way to remember

```text
Authentication → Who are you?

Authorization  → What can you access?
```

---

## 3. What is JWT?

**JWT (JSON Web Token)** is a token commonly used to carry information about an authenticated user/client.

A JWT looks roughly like:

```text
xxxxx.yyyyy.zzzzz
```

It has three parts:

```text
Header.Payload.Signature
```

---

## 4. JWT Header

The **Header** describes the token type and signing algorithm.

Example:

```json
{
  "alg": "RS256",
  "typ": "JWT"
}
```

- `alg` → signing algorithm
- `typ` → token type

---

## 5. JWT Payload

The **Payload** contains claims about the token/user/client.

Example:

```json
{
  "sub": "123456789",
  "iss": "https://cognito-idp.ap-south-1.amazonaws.com/...",
  "aud": "client-id",
  "exp": 1750000000
}
```

Common claims:

- `sub` → subject/user identifier
- `iss` → issuer
- `aud` → audience/client
- `exp` → expiration time

---

## 6. JWT Signature

The **Signature** is used to verify that the token has not been tampered with and was signed using the expected key.

Conceptually:

```text
RSASHA256(
  base64url(header) + "." + base64url(payload),
  privateKey
)
```

A JWT therefore looks like:

```text
eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9
.
eyJzdWIiOiIxMjM0NTY3ODkiLCJpc3MiOiJodHRwczovL2NvZ25pdG8taWRwLi4uIn0
.
<signature>
```

### Easy way to remember

```text
Header    → How is this token signed?
Payload   → What information/claims are inside?
Signature → Can we verify it hasn't been tampered with?
```

---

## 7. JWT with API Gateway

A common flow is:

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
 |
 | Authorization: Bearer <JWT>
 v
API Gateway
 |
 | Validate JWT
 v
Lambda
```

If the JWT is valid:

```text
JWT valid
   ↓
Request allowed
   ↓
Backend
```

If the JWT is invalid or missing:

```text
JWT invalid/missing
       ↓
Request rejected
       ↓
401 Unauthorized
```

---

## 8. Authentication vs Authorization in Our Architecture

Using Cognito + API Gateway:

```text
Cognito
   |
   | Authenticates user
   v
JWT issued
   |
   v
Client
   |
   | Bearer JWT
   v
API Gateway
   |
   | JWT Authorizer
   | validates token
   v
Lambda
```

So:

- **Cognito** → helps authenticate the user and issue tokens.
- **JWT** → carries authentication information/claims.
- **API Gateway Authorizer** → validates the token before allowing the request.
- **Backend** → processes the authorized request.

---

## Key Takeaways

- **Authentication** answers: *Who are you?*
- **Authorization** answers: *What are you allowed to access?*
- **JWT** is a token commonly used to carry authentication claims.
- A JWT has three parts: **Header, Payload, Signature**.
- The client typically sends the JWT using the `Authorization` header.
- API Gateway can validate JWTs before forwarding requests to the backend.
- Invalid/missing authentication commonly results in **401 Unauthorized**.
