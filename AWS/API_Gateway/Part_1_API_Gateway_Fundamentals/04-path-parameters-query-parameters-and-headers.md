# Concept 4: Path Parameters, Query Parameters and Headers

> **Goal:** Understand the three common ways information is passed in an API request.

---

## 1. Path Parameters

A **path parameter** is a dynamic value included directly in the URL path.

Example:

```text
/users/{id}
```

A real request:

```text
/users/101
```

Here:

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

### When to use

Use path parameters when identifying a **specific resource**.

Examples:

```text
/users/101
/products/50
/orders/5001
```

---

## 2. Query Parameters

A **query parameter** is additional information provided after `?` in the URL.

Example:

```text
/users?role=admin
```

Here:

```text
role = admin
```

Multiple query parameters:

```text
/users?role=admin&status=active
```

Here:

```text
role   = admin
status = active
```

### When to use

Query parameters are commonly used for:

- Filtering
- Searching
- Sorting
- Pagination

Examples:

```text
/products?category=mobile
/users?role=admin
/products?sort=price
/products?page=2
```

---

## 3. Headers

**Headers** carry additional information about the request.

Example:

```http
Authorization: Bearer <token>
Content-Type: application/json
```

Headers can be used for:

- Authentication
- Content type
- Client/request information

Example:

```http
GET /users/101
Authorization: Bearer <token>
```

Here:

```text
Path   → /users/101
Header → Authorization
```

---

## 4. Path vs Query vs Header

| Type | Purpose | Example |
|---|---|---|
| Path Parameter | Identify a specific resource | `/users/101` |
| Query Parameter | Provide optional/filtering information | `/users?role=admin` |
| Header | Provide request metadata | `Authorization: Bearer <token>` |

---

## 5. Example Using All Three

A request could look like:

```http
GET /users/101?details=true
Authorization: Bearer <token>
Content-Type: application/json
```

Break it down:

```text
Path:
 /users/101
 id = 101

Query Parameter:
 details = true

Headers:
 Authorization = Bearer <token>
 Content-Type = application/json
```

So the same request can contain all three.

---

## 6. How API Gateway Receives Them

Conceptually:

```text
Client
  |
  | GET /users/101?details=true
  | Authorization: Bearer <token>
  v
API Gateway
  |
  +-- Path Parameter → id = 101
  |
  +-- Query Parameter → details = true
  |
  +-- Header → Authorization
  |
  v
Backend
```

The backend can then use these values to process the request.

---

## 7. Simple Mental Model

Think of them like this:

```text
PATH
"What resource?"

/users/101
       ↑
    specific user


QUERY
"How should I retrieve it?"

/users?role=admin
        ↑
      filter


HEADER
"Additional information about the request"

Authorization: Bearer <token>
       ↑
   request metadata
```

---

## 8. Key Takeaways

- **Path parameter** → identifies a specific resource.
- **Query parameter** → provides additional/filtering information.
- **Header** → provides metadata about the request.
- Path parameters are part of the URL path.
- Query parameters come after `?`.
- Headers are sent separately from the URL.
- A single request can contain all three.

### Quick Example

```text
/users/101?details=true
Authorization: Bearer <token>
```

```text
/users/101
    ↓
Path Parameter → id = 101

?details=true
    ↓
Query Parameter → details = true

Authorization: Bearer <token>
    ↓
Header → authentication information
```
