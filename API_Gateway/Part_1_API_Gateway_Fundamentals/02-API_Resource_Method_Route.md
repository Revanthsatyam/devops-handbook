# API, Resource, Method and Route

## API
The overall interface exposed to clients.

Example:
User API

## Resource / Path
Represents what we're working with.

Examples:
/users
/orders
/products

## Method
Defines the operation.

GET    → Read
POST   → Create
PUT    → Replace/update
PATCH  → Partial update
DELETE → Delete

## Route
Route = HTTP Method + Path

Examples:
GET /users
POST /users
GET /users/{id}

## Path Parameter

/users/{id}

Example:
/users/101

id = 101

## Simple Flow

Client
  ↓
GET /users/101
  ↓
API Gateway
  ↓
GET /users/{id}
  ↓
Lambda

## Important

GET /users and POST /users are
different routes because the HTTP method is different.

If the method or path doesn't match,
the request won't reach the intended backend.

## Key Takeaways

- API = overall API
- Resource/Path = what we're accessing
- Method = what operation we want
- Route = Method + Path
- {id} = path parameter