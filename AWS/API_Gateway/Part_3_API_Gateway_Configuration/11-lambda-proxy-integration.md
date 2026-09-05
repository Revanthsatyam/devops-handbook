# Concept 2: Lambda / Proxy Integration

Lambda Proxy Integration explains how **API Gateway and Lambda communicate**.

---

## 1. What is Lambda Proxy Integration?

With **Lambda proxy integration**, API Gateway passes the incoming request information to Lambda with minimal transformation.

```text
Client
  |
  | GET /users/101
  v
API Gateway
  |
  | Request details
  v
Lambda
  |
  | Response
  v
API Gateway
  |
  v
Client
```

API Gateway does not need to manually transform every request and response.

---

## 2. What does Lambda receive?

Lambda receives an **event object** containing information about the request.

Conceptually:

```json
{
  "requestContext": {},
  "rawPath": "/users/101",
  "headers": {},
  "queryStringParameters": {},
  "pathParameters": {
    "id": "101"
  }
}
```

The event can contain:

- HTTP method
- Path
- Path parameters
- Query parameters
- Headers
- Request body
- Other request information

---

## 3. What does Lambda return?

Lambda returns a response containing information such as:

```json
{
  "statusCode": 200,
  "headers": {
    "Content-Type": "application/json"
  },
  "body": "{"message":"User found"}"
}
```

API Gateway uses this response to construct the HTTP response sent to the client.

---

## 4. Why Proxy Integration is Useful

Without proxy integration, request and response mappings may need to be configured manually.

With proxy integration:

```text
Request
   ↓
API Gateway
   ↓
Lambda receives request information
   ↓
Lambda processes it
   ↓
Lambda returns HTTP-style response
   ↓
API Gateway
   ↓
Client
```

This makes the integration **simpler and more flexible**.

---

## Key Takeaways

- Lambda Proxy Integration connects API Gateway directly to Lambda.
- API Gateway passes request information through the event.
- Lambda handles the application logic.
- Lambda returns `statusCode`, headers and body.
- API Gateway sends the Lambda response back to the client.
- Proxy integration reduces the need for manual request/response mapping.
