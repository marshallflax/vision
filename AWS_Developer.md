# AWS Developer (Associate)

## Knowledge

- Develop Using
  - Service APIs
  - CLI
  - SDKs
- Deploy Using
  - CI/CD pipelines

---

## Domain 1 -- Development

### Task 1 -- Application code

- Patterns
  - Microservices vs monolithic, Fan-out, Idempotency, stateful, stateless, loose coupling
  - Event-driven, choreography and orchestration, synchronous and asynchronous
  - Fault-tolerance, e.g. exponential back-off, jitter, DLQ
  - Streaming
- APIs: response/request, validation, status codes
- Serverless Application Model (AWS SAM) unit testing
- Messaging, APIs, SDKs

### Task 2 -- Lambda code

- Testing, Event source mapping, event-driven architectures, scalability, stateless apps, unit testing, VPC private resources
- Lambda function configuration via parameters/environment variables -- memory, concurrency, timeout, runtime, handler, layers, extensions, triggers, destinations, tuning.
- Integration with services, destinations, DLQ

### Task 3 -- Data stores

- SQL and NoSQL, CRUD, ephemeral vs persistent
- Keys -- partition, DynamoDB
- Cloud storage -- file, object, DB
- Consistency -- strong, eventual
- Caching -- write-through, read-through, lazy, TTL.
- Query vs scan
- S3 tiers
- Serialization/deserialization
- Data life-cycles

---

## Domain 2 -- Security

### Task 1 -- Authentication and Authorization

- Federated -- Security Assertion Markup (SAML), OpenID Connect (OIDC), Amazon Cognito
- Tokens -- JSON Web Token (JWT), OAuth, AWS Security Token Service (STS)
- Cognito -- user pools and identity pools
- Policies -- resource, service, principal, Role-based access control (RBAC), ACL, least privilege, customer vs AWS managed

### Task 2 -- Encryption at rest and in transit

- Certificates -- AWS Private Cert Authority
- Key Management -- rotation, AWS-managed, customer-management
- SSH keys

### Task 3 -- Sensitive data

- Classification -- PII, PHI
- Env variables
- Secrets management -- AWS Secrets Manager, AWS Systems Manager Parameter Store
- Credential handling
- Sanitization

---

## Domain 3 -- Deployment

### Task 1 -- Artifacts

- Configuration data -- AWS AppConfig, Secrets Manager, Parameter Store
  - Dependencies -- container images, config files, env variables
  - Memory, cores
- Lambda -- packaging, layers, configuration
- Git -- AWS CodeCommit
- Container Images

### Task 2 -- Dev environments

- Application deployment
- Mock endpoints
- Lambda aliases, development endpoints
- Deploying to existing environments

### Task 3 -- Automated deployment testing

- API Gateway states
- CI/CD branches and actions
- Automated testing -- unit, mock
- Creating test events
- Integration testing -- Lambda aliases, container image tags, AWS Amplify branches, AWS Copilot envs.
- Infrastructure as Code (IaC) -- SAM templates, CloudFormation templates
- API Gateway environments

### 