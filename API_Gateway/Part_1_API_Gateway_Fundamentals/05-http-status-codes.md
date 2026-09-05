# Concept 5: HTTP Status Codes

HTTP status codes tell the client **what happened when the API processed a request**.

---

## 1. What is an HTTP Status Code?

Every API response contains a status code.

Example:

```http
HTTP/1.1 200 OK
```

The code tells the client whether the request:

- succeeded
- failed because of the request
- failed because of the server

---

## 2. Main Status Code Categories

| Range | Meaning |
|---|---|
| `1xx` | Informational |
| `2xx` | Success |
| `3xx` | Redirection |
| `4xx` | Client error |
| `5xx` | Server error |

For API development, **2xx, 4xx and 5xx** are the most important.

---

## 3. Important 2xx Codes

### `200 OK`

Request succeeded.

```text
GET /users/101
→ 200 OK
```

### `201 Created`

A new resource was successfully created.

```text
POST /users
→ 201 Created
```

### `204 No Content`

Request succeeded, but there is no response body.

Common example:

```text
DELETE /users/101
→ 204 No Content
```

---

## 4. Important 4xx Codes

These generally indicate a problem with the **client's request**.

### `400 Bad Request`

The request is invalid.

```text
POST /users
Invalid JSON
→ 400 Bad Request
```

### `401 Unauthorized`

Authentication is missing or invalid.

```text
GET /users
No valid token
→ 401 Unauthorized
```

### `403 Forbidden`

The client is authenticated but **does not have permission**.

```text
Authenticated user
        ↓
Insufficient permissions
        ↓
403 Forbidden
```

### `404 Not Found`

The requested resource or route does not exist.

```text
GET /users/9999
→ 404 Not Found
```

---

## 5. Important 5xx Codes

These indicate a problem on the **server/backend side**.

### `500 Internal Server Error`

The server encountered an unexpected error.

```text
Client
  ↓
API Gateway
  ↓
Lambda
  ↓
Error
  ↓
500
```

### `502 Bad Gateway`

A gateway/proxy received an invalid response from the backend.

### `503 Service Unavailable`

The service is temporarily unavailable.

---

## 6. Simple Mental Model

```text
2xx → "Request worked"

4xx → "Your request has a problem"

5xx → "Something went wrong on the server"
```

---

## 7. API Gateway Context

API Gateway can return status codes based on what happens during request processing.

Example:

```text
Client
  |
  | GET /users/101
  v
API Gateway
  |
  | Route matched
  v
Backend
  |
  | Successful response
  v
API Gateway
  |
  | 200 OK
  v
Client
```

If the route doesn't exist:

```text
Client
  |
  | GET /wrong-route
  v
API Gateway
  |
  | 404
  v
Client
```

If the backend fails:

```text
Client
  |
  v
API Gateway
  |
  v
Backend
  |
  | Error
  v
500 / 502 / 503
```

---

## Key Takeaways

- HTTP status codes describe the result of an API request.
- `2xx` → success.
- `4xx` → client/request problem.
- `5xx` → server/backend problem.
- `200` → successful request.
- `201` → resource created.
- `204` → successful request with no body.
- `400` → bad request.
- `401` → authentication problem.
- `403` → permission problem.
- `404` → resource/route not found.
- `500` → server error.
