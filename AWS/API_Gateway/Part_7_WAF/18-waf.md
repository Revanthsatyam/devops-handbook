# Part 7 — AWS WAF

AWS WAF (**Web Application Firewall**) helps protect web applications and APIs by inspecting incoming requests and applying rules to allow, block, or count traffic.

---

## 1. Web ACL

A **Web ACL (Web Access Control List)** contains the WAF rules used to inspect traffic.

In our hands-on work, we created:

```text
Web ACL
  |
  +-- Rate-Based Rule
  |
  v
API Gateway
```

---

## 2. WAF Rules

A WAF rule evaluates incoming requests and can take actions such as:

```text
ALLOW
BLOCK
COUNT
```

Rules can be used for different types of protection.

For our API Gateway testing, we used a **rate-based rule**.

---

## 3. Rate-Based Rule

A rate-based rule helps protect an API from clients sending requests at a high rate.

For testing, we reduced the configured limit to:

```text
10 requests
```

Conceptually:

```text
Client
  |
  | High request rate
  v
AWS WAF
  |
  | Rate-Based Rule
  |
  +---- Allowed
  |
  +---- Blocked
```

### Important

WAF rate-based rules are **not an exact request counter**.

The rate is evaluated over a time window, so blocking may not happen immediately after reaching the configured number.

---

## 4. WAF + API Gateway

We associated the WAF Web ACL with the appropriate **API Gateway stage**.

The request flow becomes:

```text
Client
   |
   v
API Gateway
   |
   v
AWS WAF
   |
   | Allow / Block
   v
Backend
```

The important relationship is:

```text
Custom Domain
      |
      v
API Mapping
      |
      v
API + Stage
      |
      v
WAF Association
```

The WAF must protect the API/stage that is actually receiving the traffic.

---

## 5. WAF Testing & Troubleshooting

During testing, we checked WAF activity using **sampled requests**.

Important observations:

- WAF changes can take some time to propagate.
- Rate-based rules may take time to detect and enforce the rate.
- Changing a rate-based rule's limit can reset its counting state.
- Sampled requests only show a sample of recent traffic, not every request.
- A WAF association error can sometimes occur temporarily while AWS resources are propagating.

We encountered:

```text
WAFNonexistentItemException
```

while checking the Web ACL association. This was related to AWS resource propagation rather than the WAF rule itself.

---

## 6. Important Certificate Distinction

During the API Gateway work, we also encountered a certificate error involving a **stage client certificate**.

This is different from the ACM certificate used for an API Gateway custom domain.

```text
Custom Domain
     |
     v
ACM Certificate
     |
     v
HTTPS / TLS
```

versus:

```text
API Gateway Stage
     |
     v
Client Certificate
     |
     v
Backend client authentication
```

Do not confuse the two.

---

## Key Takeaways

- **WAF** protects APIs and web applications by inspecting requests.
- **Web ACL** contains WAF rules.
- Rules can **Allow, Block, or Count** traffic.
- **Rate-based rules** help protect against high request rates.
- WAF can be associated with an **API Gateway stage**.
- Rate-based rules are not precise request counters.
- WAF changes and associations may require propagation time.
- **Sampled requests** help inspect recent WAF traffic.
- The ACM certificate for a custom domain is different from an API Gateway stage client certificate.
