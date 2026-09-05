# Part 6 — Custom Domain & HTTPS

This part connects **ACM, HTTPS/TLS, API Gateway Custom Domains, API Mappings, and Route 53**.

---

## 1. Why Custom Domains & HTTPS?

API Gateway provides a default endpoint such as:

```text
https://abc123.execute-api.ap-south-1.amazonaws.com
```

A custom domain gives the API a simpler hostname:

```text
https://api.example.com
```

HTTPS is used to secure communication between the client and API.

---

## 2. AWS Certificate Manager (ACM)

**AWS Certificate Manager (ACM)** manages TLS/SSL certificates.

For a custom API domain, ACM provides the certificate for the hostname.

Example:

```text
ACM
 |
 | TLS Certificate
 v
API Gateway Custom Domain
```

The certificate allows clients to establish a trusted HTTPS connection.

---

## 3. DNS Validation

Before ACM issues a public certificate, it needs to verify that you control the domain.

With DNS validation, ACM provides a DNS record that must be added to your DNS service.

Route 53 can create this validation record.

```text
Route 53
   |
   | DNS validation record
   v
ACM
   |
   | Domain validated
   v
Certificate issued
```

---

## 4. TLS / HTTPS

```text
HTTP
 ↓
Normal HTTP communication

HTTPS
 ↓
HTTP + TLS encryption
```

The TLS certificate allows the client to establish a secure connection with the API Gateway custom domain.

Basic flow:

```text
Client
  |
  | HTTPS
  v
API Gateway
  |
  | TLS Certificate
  v
Secure connection
```

---

## 5. API Gateway Custom Domain

A **Custom Domain Name** lets you use your own hostname instead of the default API Gateway URL.

Example:

```text
api.example.com
       |
       v
API Gateway Custom Domain
       |
       v
API
```

For a Regional API, the custom domain is associated with the API's Region and uses an ACM certificate.

---

## 6. API Mapping

An **API Mapping** connects the custom domain to a specific API and stage.

Example:

```text
api.example.com
       |
       | API Mapping
       v
API: api-learning
Stage: prod
```

Then a request such as:

```text
https://api.example.com/users
```

is routed to the mapped API and stage.

### Important distinction

> **Custom Domain = the hostname**

> **API Mapping = which API + stage receives the request**

---

## 7. Route 53

**Route 53** provides DNS.

A DNS record can point your custom hostname to the API Gateway custom-domain target.

Flow:

```text
User
 |
 | api.example.com
 v
Route 53
 |
 | DNS resolution
 v
API Gateway Custom Domain
```

For a Route 53 hosted zone, an Alias record can be used to point the domain to the API Gateway custom-domain target.

---

## 8. Complete Architecture

```text
                    ACM
                     |
              TLS Certificate
                     |
                     v
User ---> Route 53 ---> API Gateway
          DNS          Custom Domain
                           |
                      API Mapping
                           |
                         Stage
                           |
                           v
                        Lambda
```

### Complete Request Flow

```text
https://api.example.com/users
              |
              v
          Route 53
              |
              v
     API Gateway Custom Domain
              |
         API Mapping
              |
            Stage
              |
              v
          API Gateway
              |
              v
            Lambda
```

---

## 9. Key Takeaways

- **ACM** → provides the TLS/SSL certificate.
- **DNS validation** → verifies domain ownership for the certificate.
- **HTTPS/TLS** → secures communication.
- **Custom Domain** → provides a friendly API hostname.
- **API Mapping** → connects the hostname to an API + stage.
- **Route 53** → handles DNS resolution.
- **API Gateway** → receives and processes the API request.

### Final Mental Model

```text
Route 53
   ↓
"Where is api.example.com?"

ACM
   ↓
"Is the HTTPS connection trusted?"

Custom Domain
   ↓
"What hostname does the API use?"

API Mapping
   ↓
"Which API + stage should receive it?"

API Gateway
   ↓
"Process the request"

Backend
   ↓
"Execute business logic"
