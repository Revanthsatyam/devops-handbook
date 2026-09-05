# Concept 3: Request and Response

**Goal:** Understand what an HTTP request and response contain and how they move through API Gateway.

---

## 1. What Is a Request?

A **request** is the message a client sends to an API asking it to perform an operation.

Example:

```http
GET /users/101
```

The client is essentially saying:

> Give me the details of user 101.

---

## 2. What Does a Request Contain?

A request can contain several parts:

```text
Request
 |
 +-- HTTP Method
 |
 +-- Path
 |
 +-- Headers
 |
 +-- Query Parameters
 |
 +-- Body
```

### HTTP Method

Defines the operation.

```text
GET
POST
PUT
PATCH
DELETE
```

Example:

```http
GET /users/101
```

### Path

Identifies the resource being accessed.

```text
/users/101
```

### Headers

Provide additional information about the request.

Example:

```http
Authorization: Bearer <token>
Content-Type: application/json
```

Headers can be used for things such as:

- Authentication
- Content type
- Client information

### Query Parameters

Provide additional information through the URL.

Example:

```text
/users?role=admin
```

Here:

```text
role = admin
```

is a query parameter.

### Body

Contains data sent with the request.

For example, when creating a user:

```http
POST /users
Content-Type: application/json
```

Body:

```json
{
  "name": "John",
  "email": "john@example.com"
}
```

---

# 3. What Is a Response?

A **response** is the message returned by the backend/API to the client after processing a request.

For example:

```http
HTTP/1.1 200 OK
```

```json
{
  "id": 101,
  "name": "John"
}
```

---

## 4. What Does a Response Contain?

A response commonly contains:

```text
Response
 |
 +-- Status Code
 |
 +-- Headers
 |
 +-- Body
```

### Status Code

Tells the client what happened.

Examples:

```text
200 → Success
201 → Created
400 → Bad Request
401 → Unauthorized
403 → Forbidden
404 → Not Found
500 → Internal Server Error
```

### Headers

Provide additional information about the response.

Example:

```http
Content-Type: application/json
```

### Body

Contains the actual response data.

Example:

```json
{
  "id": 101,
  "name": "John"
}
```

---

# 5. Simple Request and Response Example

Client sends:

```http
GET /users/101
```

API Gateway receives the request and sends it to the backend.

The backend processes it and returns:

```http
200 OK
```

with:

```json
{
  "id": 101,
  "name": "John"
}
```

The complete flow:

```text
Client
  |
  | GET /users/101
  v
API Gateway
  |
  v
Backend
  |
  | 200 OK + JSON
  v
API Gateway
  |
  v
Client
```

---

# 6. Request With Body

For operations such as creating a user, the client may send data in the request body.

```http
POST /users
Content-Type: application/json
```

```json
{
  "name": "John",
  "email": "john@example.com"
}
```

The backend processes the data and could return:

```http
201 Created
```

```json
{
  "id": 101,
  "name": "John",
  "email": "john@example.com"
}
```

---

# 7. Important Status Codes

You don't need to memorize every HTTP status code.

For API Gateway, these are the important ones to remember:

| Status Code | Meaning |
|---|---|
| `200` | Request succeeded |
| `201` | Resource created |
| `400` | Bad request |
| `401` | Authentication required/failed |
| `403` | Access forbidden |
| `404` | Resource/route not found |
| `500` | Internal server error |

### Easy Grouping

```text
2XX → Success
4XX → Client/request problem
5XX → Server/backend problem
```

---

# 8. API Gateway's Role

API Gateway sits between the client and backend.

```text
Client
   |
   | Request
   v
API Gateway
   |
   | Forward
   v
Backend
   |
   | Response
   v
API Gateway
   |
   | Return
   v
Client
```

API Gateway can inspect and manage parts of the request before sending it to the backend.

It can also return the backend's response to the client.

---

# 9. Key Takeaways

- **Request** = message sent by the client.
- **Response** = message returned to the client.
- A request can contain:
  - Method
  - Path
  - Headers
  - Query parameters
  - Body
- A response commonly contains:
  - Status code
  - Headers
  - Body
- `2XX` = success
- `4XX` = client/request problem
- `5XX` = server/backend problem
- API Gateway sits between the client and backend.

### Simple Mental Model

```text
Request:

Client
  |
  v
API Gateway
  |
  v
Backend


Response:

Backend
  |
  v
API Gateway
  |
  v
Client
```

---

## Concept Status

- [x] What is a Request?
- [x] Request components
- [x] What is a Response?
- [x] Response components
- [x] Request/Response flow
- [x] Request body
- [x] Important status codes
- [x] API Gateway's role
- [x] Key takeaways

---

**Next:** Part 1 → Concept 4 — **Path Parameters, Query Parameters and Headers**
