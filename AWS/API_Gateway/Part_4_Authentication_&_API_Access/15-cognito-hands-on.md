# Part 4 — Cognito Hands-On

This document records the **Cognito setup and API Gateway authentication flow we implemented**.

---

## 1. Cognito User Pool

A **Cognito User Pool** was created to manage users and authenticate them.

```text
User
  |
  | Login
  v
Cognito User Pool
```

> **Important:** We used a **User Pool**, not an Identity Pool.  
> User Pools are used for user authentication and issuing tokens.

---

## 2. App Client

A public **App Client** was created for the application.

The App Client identifies the application that is requesting authentication.

```text
Cognito User Pool
       |
       v
   App Client
```

The App Client used in our setup had **no client secret**, so it could be used as a public client.

---

## 3. Test User

A test user was created in the User Pool.

The initial password required a password change, so the user completed the required login flow before obtaining an authorization code.

---

## 4. Cognito Hosted Login

The application redirects the user to Cognito's authorization endpoint:

```text
https://<cognito-domain>/oauth2/authorize
```

The user authenticates with Cognito.

After successful authentication, Cognito redirects the browser back to the application's callback URL with an authorization code:

```text
http://localhost:3000/callback?code=<authorization-code>
```

---

## 5. Authorization Code Flow

The overall flow we implemented:

```text
Browser
   |
   | Login
   v
Cognito
   |
   | Authorization Code
   v
Browser / Application
   |
   | Exchange code
   v
/oauth2/token
   |
   | Tokens
   v
Application
```

The authorization code is:

- Short-lived
- Single-use
- Exchanged for tokens

---

## 6. Token Endpoint

The token exchange is performed against the Cognito **OAuth token endpoint**:

```text
https://<cognito-domain>/oauth2/token
```

It is important not to confuse this with the Cognito User Pool issuer:

```text
https://cognito-idp.<region>.amazonaws.com/<user-pool-id>
```

The issuer identifies the User Pool, while `/oauth2/token` is the OAuth token endpoint on the Cognito domain.

---

## 7. Public App Client

Because our App Client did not have a client secret:

```text
client_id
```

was sent in the request body during the token exchange.

The Postman request needed to avoid an unintended `Authorization` header.

Conceptually:

```text
POST /oauth2/token

grant_type=authorization_code
client_id=<app-client-id>
code=<authorization-code>
redirect_uri=<callback-url>
```

---

## 8. Tokens

After a successful token exchange, Cognito returns tokens.

The **access token** is a JWT.

It contains claims that API Gateway can use when validating the request.

Examples of claims we observed/discussed:

```text
client_id
token_use
scope
iss
exp
```

---

## 9. API Gateway JWT Authorizer

The HTTP API was configured with a JWT authorizer.

Important configuration:

```text
Issuer:
https://cognito-idp.<region>.amazonaws.com/<user-pool-id>

Audience:
<app-client-id>

Identity Source:
$request.header.Authorization
```

The client sends:

```http
Authorization: Bearer <access-token>
```

The API Gateway authorizer validates the JWT before allowing the request to reach Lambda.

---

## 10. Complete Flow We Implemented

```text
User
  |
  | Login
  v
Cognito User Pool
  |
  | Authorization Code
  v
Application
  |
  | /oauth2/token
  v
Cognito
  |
  | Access Token (JWT)
  v
Application
  |
  | Authorization: Bearer <JWT>
  v
API Gateway
  |
  | JWT Authorizer
  |
  | Valid
  v
Lambda
  |
  v
Response
```

If the token is missing or invalid:

```text
Client
  |
  | Request without valid JWT
  v
API Gateway
  |
  | JWT Authorizer
  v
401 Unauthorized
```

---

## 11. Troubleshooting Lessons

### `invalid_client`

For a public App Client:

- Do not send a client secret.
- Send `client_id` in the form body.
- Make sure Postman is not adding an unintended `Authorization` header.

### `invalid_grant`

Common causes encountered:

- Authorization code was already used.
- Authorization code expired.
- PKCE/code-verifier mismatch.

Authorization codes are **single-use and short-lived**.

### Cognito URL Confusion

Remember the difference:

```text
User Pool Issuer
https://cognito-idp.<region>.amazonaws.com/<user-pool-id>

OAuth Token Endpoint
https://<cognito-domain>/oauth2/token
```

---

## Key Takeaways

- **User Pool** → manages users and authentication.
- **App Client** → represents the application.
- **Authorization Code** → temporary code returned after login.
- `/oauth2/token` → exchanges the code for tokens.
- **Access Token** → JWT used to access protected APIs.
- **JWT Authorizer** → validates the token at API Gateway.
- `Authorization: Bearer <token>` → sends the token with the API request.
- Valid token → request reaches Lambda.
- Invalid/missing token → API Gateway rejects the request.

---

## Practical Architecture

```text
Cognito
   |
   | JWT
   v
Client
   |
   | Authorization header
   v
API Gateway
   |
   | JWT Authorizer
   v
Lambda
```
