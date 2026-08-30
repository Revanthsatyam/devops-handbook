# Amazon API Gateway — Syllabus

## Part 1 — API Gateway Fundamentals

- What is API Gateway?
- API, Resource, Method and Route
- Request & Response
- Path Parameters
- Query Parameters
- Headers
- HTTP Status Codes
- REST API vs HTTP API
- Regional Endpoint

## Part 2 — HTTP Methods

- GET
- POST
- PUT
- PATCH
- DELETE
- OPTIONS
- CORS

## Part 3 — API Gateway Configuration

- Integrations
- Lambda / Proxy Integration
- Stages
- Deployments

## Part 4 — Authentication & API Access

- Authentication vs Authorization
- Cognito
- JWT
- Authorizers
- API Keys
- Usage Plans
- Throttling
- Quotas

## Part 5 — CORS

- Same-Origin Policy
- CORS
- Preflight Requests
- OPTIONS
- Allowed Origins
- Allowed Methods
- Allowed Headers
- Credentials

## Part 6 — Custom Domain & HTTPS

- AWS Certificate Manager (ACM)
- TLS/SSL Certificates
- DNS Validation
- API Gateway Custom Domains
- API Mappings
- Route 53
- Complete Custom Domain Flow

## Part 7 — WAF

- What is AWS WAF?
- Web ACL
- WAF Rules
- Rate-Based Rules
- WAF + API Gateway
- WAF Troubleshooting

> We will learn WAF in much greater depth later as a separate AWS service.

## Part 8 — Monitoring

- API Gateway Metrics
- Request Count
- 4XX Errors
- 5XX Errors
- Latency
- Integration Latency
- Throttling
- Basic CloudWatch Monitoring

> CloudWatch itself will be learned separately.

## Part 9 — Final Architecture

- Complete API Gateway Architecture
- Request Flow
- Authentication Flow
- WAF Flow
- DNS Flow
- HTTPS Flow
- Monitoring Flow
- Troubleshooting Flow

## Part 10 — Production-Style Project

We will eventually apply what we learned to build a production-style API architecture involving:

- Route 53
- ACM
- WAF
- API Gateway
- Authentication / Authorization
- Backend
- Database

---

## Learning Approach

For each concept:

1. Learn the concept
2. Understand why it exists
3. Understand how it works
4. Implement it
5. Test it
6. Troubleshoot it
7. Document it
8. Review it
9. Commit it to GitHub