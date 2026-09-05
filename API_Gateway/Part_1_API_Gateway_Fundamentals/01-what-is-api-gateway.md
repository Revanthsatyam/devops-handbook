# Concept 1: What Is Amazon API Gateway?

**Goal:** Understand what API Gateway is, why we need it, what role it plays between a client and a backend, and the basic terminology around it.

## 1. What Is an API?

API stands for **Application Programming Interface**.

An API is a defined way for one application to communicate with another application or service.

For example, imagine a mobile banking application. When you open your account and see your balance, the mobile app does not directly access the bank's database.

Instead, the request flows through an API:

```text
Mobile App
    |
    | GET /account/balance
    v
   API
    |
    v
Backend Application
    |
    v
Database
```

The API defines things such as:

- What URL should be called
- Which HTTP method should be used
- What data should be sent
- What response will be returned
- What authentication is required

For example:

```http
GET /users/101
```

This could mean:

> Give me the information about user 101.

The response could be:

```json
{
  "id": 101,
  "name": "John",
  "email": "john@example.com"
}
```

So, an API acts as a contract between the client and the backend.

## 2. What Is Amazon API Gateway?

Amazon API Gateway is an AWS service that allows you to create, publish, secure, manage, monitor, and expose APIs.

In simple terms:

> API Gateway is the front door through which clients can communicate with your backend services.

Instead of allowing clients to directly access your backend, you can put API Gateway in front of it.

```text
Client
  |
  v
API Gateway
  |
  v
Backend
```

The backend could be:

```text
API Gateway
     |
     +----> Lambda
     |
     +----> HTTP application
     |
     +----> Other AWS service
     |
     +----> Container/application
```

## 3. Why Do We Need API Gateway?

Imagine you have a Lambda function that retrieves user information. Without API Gateway, you would need some other mechanism for clients to invoke that backend.

With API Gateway:

```text
Client
   |
   | HTTPS request
   v
API Gateway
   |
   | Invoke
   v
Lambda
   |
   v
Response
```

The client does not need to know how the backend works. It only needs to know the API endpoint:

```text
https://api.example.com/users/101
```

This gives us a clean separation:

```text
Client
   |
   | API
   v
API Gateway
   |
   | Backend integration
   v
Application
```

## 4. What Problems Does API Gateway Solve?

API Gateway provides several important capabilities around an API.

### 1. API Exposure

It gives clients an HTTP/HTTPS endpoint.

Example:

```text
https://abc123.execute-api.us-east-1.amazonaws.com/users
```

### 2. Routing

API Gateway determines where a request should go.

```text
GET /users
        |
        v
     Lambda A

GET /orders
        |
        v
     Lambda B

POST /users
        |
        v
     Lambda C
```

### 3. Authentication and Authorization

API Gateway can work with mechanisms such as:

- JWT
- Amazon Cognito
- Authorizers
- API keys

This lets you control who is allowed to access your API.

### 4. Traffic Control

API Gateway can help control API traffic using mechanisms such as:

- Throttling
- Quotas
- Usage plans

This helps prevent clients from overwhelming your API.

### 5. Security

API Gateway can be combined with other AWS services to protect APIs.

For example:

```text
Client
  |
  v
API Gateway
  |
  +---- WAF
  |
  v
Backend
```

### 6. Monitoring

API Gateway publishes metrics that allow you to understand API behavior, including:

- Number of requests
- 4XX errors
- 5XX errors
- Latency
- Integration latency
- Throttled requests

We will learn monitoring separately in much greater detail later.

## 5. API Gateway Is Not the Backend

This is a very important concept.

API Gateway does not normally contain your application's business logic.

For example:

```text
Client
  |
  v
API Gateway
  |
  v
Lambda
  |
  v
Database
```

### API Gateway

API Gateway is responsible for things around the API:

- Receiving requests
- Routing requests
- Authentication and authorization
- Traffic control
- API configuration
- API-level monitoring

### Lambda or Application

The backend is responsible for business logic, such as:

- Find a user's information
- Create an order
- Calculate a payment
- Update a product

So think of it this way:

> API Gateway manages the API. Your backend performs the actual application work.

## 6. Simple Real-World Example

Imagine an e-commerce application. A customer wants to retrieve their orders.

The frontend sends:

```http
GET /orders
```

The request reaches API Gateway:

```text
Customer
   |
   | GET /orders
   v
API Gateway
   |
   | Is the request allowed?
   | Which backend should receive it?
   v
Backend
   |
   v
Database
```

The backend retrieves the orders and returns:

```json
{
  "orders": [
    {
      "id": "ORD-1001",
      "status": "SHIPPED"
    },
    {
      "id": "ORD-1002",
      "status": "PROCESSING"
    }
  ]
}
```

API Gateway then returns that response to the client.

## 7. API Gateway Request Flow

The basic flow to remember is:

```text
REQUEST

Client --------------------> API Gateway
                                  |
                                  | Routing
                                  | Authentication
                                  | Authorization
                                  | Throttling
                                  v
                              Backend
                                  |
                                  v
                             Application
                                  |
                                  v
                              Database

RESPONSE

Client <-------------------- API Gateway
```

A more simplified version:

```text
Client
  v
API Gateway
  v
Backend
  v
Database
```

Then the response returns in the opposite direction:

```text
Database
  v
Backend
  v
API Gateway
  v
Client
```

## 8. Important API Gateway Terminology

We will learn these properly in the upcoming concepts, but you should know the basic vocabulary.

### API

The API represents the interface exposed to clients.

Example: `User Management API`

### Resource or Path

Represents a URL path in a REST API.

Examples:

```text
/users
/users/{id}
```

### Method

Defines the HTTP operation.

Examples:

```text
GET
POST
PUT
PATCH
DELETE
```

For example:

```http
GET /users
```

### Route

HTTP APIs commonly use the concept of a route.

Examples:

```http
GET /users
POST /users
GET /users/{id}
```

A route combines:

> HTTP method + path

### Integration

An integration defines what API Gateway sends the request to.

For example:

```text
API Gateway
     |
     v
Lambda
```

Lambda is the integration.

### Stage

A stage represents a deployment environment or version of an API.

For example:

dev
test
prod

We will study stages and deployments separately.

### Endpoint

The URL through which clients access the API.

Example:

```text
https://abc123.execute-api.us-east-1.amazonaws.com
```

## 9. REST API vs HTTP API

API Gateway provides two major API types:

Amazon API Gateway
       |
       +---- REST API
       |
       +---- HTTP API

### REST API

The more feature-rich API Gateway option.

It supports advanced capabilities such as:

- API keys
- Usage plans
- More advanced API management features
- REST-specific configuration

### HTTP API

A simpler and generally lower-cost option designed for many common HTTP API use cases.

It supports:

- HTTP routes
- Lambda integrations
- JWT authorization
- CORS
- Stages
- Custom domains

### Important

Don't worry about memorizing every difference yet.

For now remember:

REST API = more features and advanced API management

HTTP API = simpler, lower-cost API option for many common use cases

We'll compare them properly when we reach API Gateway configuration.

## 10. Regional Endpoint

API Gateway APIs can be exposed using different endpoint types depending on the API type and configuration.

One important concept is a Regional endpoint.

A Regional API is served from an AWS Region.

For example:

AWS Region
us-east-1
    |
    ↓
API Gateway
    |
    ↓
https://xxxxx.execute-api.us-east-1.amazonaws.com

This means the API is associated with a specific AWS Region.

For the learning labs we've been doing, we've primarily worked with Regional APIs.

## 11. API Gateway vs Application Load Balancer

These services can sometimes appear similar because both can receive HTTP traffic, but their purposes are different.

### API Gateway

Primarily focuses on API management:

API
 |
 +-- Authentication
 +-- Authorization
 +-- Throttling
 +-- API keys
 +-- Usage plans
 +-- Routing
 +-- API management

### Application Load Balancer

Primarily focuses on distributing application traffic:

Users
  |
  ↓
ALB
  |
  +---- EC2
  |
  +---- EC2
  |
  +---- Container

You can even use both in an architecture when appropriate.

## 12. A Simple Mental Model

Whenever you see:

Client → API Gateway → Backend

think:

Client

"I want something from the application."

API Gateway

"I'll receive the request, check it, determine where it goes, and manage the API behavior."

Backend

"I'll actually perform the application operation."

That mental model will make the rest of API Gateway much easier.

## 13. Example Architecture

A basic serverless API could look like:

                 ┌──────────────┐
                 │    Client    │
                 │ Web / Mobile │
                 └──────┬───────┘
                        │
                        │ HTTPS
                        ▼
              ┌───────────────────┐
              │   API Gateway     │
              │                   │
              │ Routes            │
              │ Authorization     │
              │ Throttling        │
              │ API Management    │
              └─────────┬─────────┘
                        │
                        │ Integration
                        ▼
                ┌───────────────┐
                │    Backend    │
                │    Lambda     │
                └───────┬───────┘
                        │
                        ▼
                    Data Store

Note: The data store is shown only to illustrate the architecture. We will learn database services separately.

## 14. What Happens When a Request Arrives?

Suppose the client sends:

GET /users/101

Conceptually:

Step 1 — Client sends request
GET /users/101
Step 2 — API Gateway receives it

API Gateway identifies:

Method = GET
Path   = /users/101
Step 3 — API Gateway determines the route

For example:

GET /users/{id}

matches:

GET /users/101
Step 4 — API Gateway invokes the integration

For example:

Lambda
Step 5 — Backend processes the request

The backend performs the required operation.
Step 6 — Backend returns a response

For example:

{
  "id": 101,
  "name": "John"
}
Step 7 — API Gateway returns the response
Lambda
  ↓
API Gateway
  ↓
Client

## 15. Key Takeaways

Remember these points:

API = a contract/interface that allows applications to communicate.
Amazon API Gateway = AWS service for creating and managing APIs.
API Gateway acts as the front door to backend services.
API Gateway can handle:
Routing
Authentication/authorization
Traffic control
API management
Monitoring
API Gateway is not your application's business logic.
The backend performs the actual application work.
A common architecture is:
Client
  ↓
API Gateway
  ↓
Backend
API Gateway provides both:
REST APIs
HTTP APIs
Regional endpoint means the API is served from a particular AWS Region.

One-line definition

Amazon API Gateway is a managed AWS service that acts as the front door for applications by exposing APIs, routing client requests to backend services, and providing capabilities such as authorization, throttling, API management, and monitoring.