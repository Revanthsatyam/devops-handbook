# Concept 2: CORS

**CORS (Cross-Origin Resource Sharing)** controls whether a browser is allowed to make requests from one origin to another.

---

## 1. Why do we need CORS?

Suppose your frontend is running on:

```text
http://localhost:3000
```

and your API Gateway is:

```text
https://api.example.com
```

These are **different origins**.

The browser applies its **Same-Origin Policy** and may block the request unless the API explicitly allows that origin through CORS.

---

## 2. What does CORS control?

CORS can specify:

- **Allowed Origins**
- **Allowed Methods**
- **Allowed Headers**
- **Credentials**

Example:

```text
Allowed Origin:
http://localhost:3000

Allowed Methods:
GET, POST, PUT, DELETE

Allowed Headers:
Content-Type, Authorization
```

---

## 3. Preflight Request

For certain cross-origin requests, the browser first sends an:

```http
OPTIONS /users
```

This is called a **preflight request**.

The server/API responds with permission information such as:

```text
Access-Control-Allow-Origin: http://localhost:3000
Access-Control-Allow-Methods: GET, POST, OPTIONS
Access-Control-Allow-Headers: Authorization, Content-Type
```

If the response allows the request, the browser sends the actual request.

```text
Browser
   |
   | OPTIONS (preflight)
   v
API Gateway
   |
   | CORS permission
   v
Browser
   |
   | GET /users
   v
API Gateway
```

---

## 4. API Gateway + CORS

API Gateway can be configured to allow specific origins, methods, and headers.

For a local application:

```text
Frontend
http://localhost:3000
        |
        | CORS
        v
API Gateway
        |
        v
Backend
```

If the frontend sends a JWT:

```http
Authorization: Bearer <token>
```

then `Authorization` needs to be allowed as a request header.

---

## Key Takeaways

- **CORS controls browser cross-origin access.**
- Different origins can trigger CORS restrictions.
- `OPTIONS` is commonly used for the browser's **preflight request**.
- CORS configuration defines allowed origins, methods, and headers.
- `Authorization` must be allowed when the browser sends an authentication token.
- CORS is primarily a **browser security mechanism**; tools like Postman are not restricted by browser CORS rules.

---