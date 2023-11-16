# Security

- Authn - identity
- Authz - authorization

## OWASP (Open Worldwide Application Security Project) 

### 2023 Top 10 API

- BOLA -- Broken Object Level Authz
  - Cause: APIs expose object IDs
  - Fix: Object-level authz for untrusted object IDs
- Broken Authn
- Broken Object Property Level Authz
- Unrestricted Resource Consumption
- Broken Function Level Authz
- Unrestricted Access to Sensitive Business Flows
- Server-Side Request Forgery
  - E.g., block `169.254.0.0/16` from user-supplied webhooks, etc.
- Security Misconfiguration
- Broken Inventory Management
- Unsafe API consumption

### 2021 Top 10 Web Application

- Broken Access Control
- Cryptographic Failures
- Injection (and XSS)
- Insecure Design
- Security Misconfiguration
- Vulnerable/Outdated Components
- Identification/Authentication Failures
- Software and Data Integrity Failures
- Security Logging and Monitoring Failures
- Server-Side Request Forgery

### SAMM 2.0 (Software Assurance Maturity Model, 2020)

- Governance
  - Strategy and Metrics
  - Policy and Compliance
  - Education and Guidance
- Design
  - Threat Assessment
  - Security Requirements
  - Security Architecture
- Implementation
  - Secure Build
  - Secure Deployment
  - Defect Management
- Verification
  - Architecture Assessment
  - Requirements-driven Testing
  - Security Testing
- Operations
  - Incident Management
  - Environment Management
  - Operational Management