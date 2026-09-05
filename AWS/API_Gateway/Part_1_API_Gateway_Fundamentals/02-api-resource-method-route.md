# Concept 2: API, Resource, Method and Route

## API

The **API** is the overall interface exposed to clients.

Example:

```text
User Management API
```

An API can contain multiple paths and operations.

---

## Resource / Path

A **resource** represents what we are working with.

Examples:

```text
/users
/orders
/products
```

A **path** identifies where the request is going.

Examples:

```text
/users
/users/101
```

---

## HTTP Method

The HTTP method defines **what operation we want to perform**.

| Method | Typical Purpose |
|---|---|
| GET | Read data |
| POST | Create data |
| PUT | Replace/update data |
| PATCH | Partially update data |
| DELETE | Delete data |

Examples:

```text
GET /users
POST /users
GET /users/101
DELETE /users/101
```

---

## Path Parameter

A path can contain a dynamic value.

Example:

```text
/users/{id}
```

Here `{id}` is a **path parameter**.

A real request could be:

```text
/users/101
```

So:

```text
id = 101
```

Another request:

```text
/users/250
```

means:

```text
id = 250
```

---

## Route

A **route** is essentially:

```text
HTTP Method + Path
```

Examples:

```text
GET /users
POST /users
GET /users/{id}
DELETE /users/{id}
```

Even though these two routes use the same path:

```text
GET /users
POST /users
```

they are different routes because the HTTP methods are different.

---

## REST API vs HTTP API

The terminology can look slightly different depending on the API type.

### REST API

Common structure:

```text
Resource
   |
   +---- Method
   +---- Method
```

Example:

```text
/users
   |
   +---- GET
   +---- POST

/users/{id}
   |
   +---- GET
   +---- PUT
   +---- DELETE
```

### HTTP API

HTTP APIs commonly use **routes** directly:

```text
GET /users
POST /users
GET /users/{id}
DELETE /users/{id}
```

The important idea is still:

```text
Route = HTTP Method + Path
```

---

## Route Matching

When a request reaches API Gateway, API Gateway looks for a matching method and path.

Suppose we have:

```text
GET /users
GET /users/{id}
POST /users
```

If the client sends:

```text
GET /users/101
```

API Gateway matches it with:

```text
GET /users/{id}
```

The path parameter is:

```text
id = 101
```

If the client instead sends:

```text
POST /users/101
```

there is no matching route if only `GET /users/{id}` exists.

The request will not reach the intended backend.

---

## Integration

A route needs an **integration** that handles the request.

Example:

```text
GET /users/{id}
        |
        v
   API Gateway
        |
        v
      Lambda
```

The Lambda function performs the actual application logic.

---

## Simple Request Flow

```text
Client
  |
  | GET /users/101
  v
API Gateway
  |
  | Match route
  v
GET /users/{id}
  |
  | Integration
  v
Lambda
```

---

## Common Mistake

Always check both:

```text
HTTP Method + Path
```

For example:

```text
Configured:
GET /users/{id}

Request:
POST /users/101
```

The path looks similar, but the method is different.

Therefore, the route does not match.

---

## Key Takeaways

- **API** = overall API interface
- **Resource/Path** = what we are accessing
- **Method** = what operation we want
- **Path Parameter** = dynamic value inside a path
- **Route** = HTTP Method + Path
- **Integration** = backend that handles the route
- `GET /users` and `POST /users` are different routes
- A request must match the correct **method + path** to reach the intended backend
