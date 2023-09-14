# AWS Developer (Associate, DBA-C02)

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

### Task 4 -- Code deployment

- Git, AWS CodeCommit, labels and branches
- AWS CodePipeline workflow and approvals
- AppConfig and SecretsManager for app configs
- CI/CD workflow
- API Gateway -- stages, custom domains
- Strategies -- canary, blue/green, rolling, rollbacks, dynamic deployments

---

## Domain 4 -- Troubleshooting and Optimization

### Task 1 -- Root Cause

- Logging and monitoring -- CloudWatch Logs Insights, CloudWatch Embedded Metric Format (EMF)
- Visualizations, AWS X-Ray
- Code analysis tools
- HTTP codes, Common SDK exceptions

### Task 2 -- Instrumentation

- Structured logging, distributed tracing (and annotations)
- Logging, monitoring, observability
- Application metrics -- custom, embedded, builtin
- Notification alerts (e.g. quota, or deployment completion)

### Task 3 -- Optimization

- Caching (using request headers), concurrency, messaging (Simple Queue Service, Simple Notification Service, filtering)
- Profiling -- determining memory and compute requirements

---

## Scope

### Features in scope

- Analytics -- Athena, Kinesis, OpenSearch
- App Integration -- AppSync, EventBridge, Simple Notification Service, Simple Queue Service, Step Functions
- Compute -- EC2, Elastic Beanstalk, Lambda, Serverless Application Model (SAM)
- Containers -- Copilot, Elastic Container Registry (ECR), Elastic Container Service (ECS), Elastic Kubernetes Service (EKR)
- Database -- Aurora, DynamoDB, ElastiCache, MemoryDB for Redis, RDS
- Developer -- Amplify, Cloud9, CloudShell, CodeArtifact, CodeBuilt, CodeCommit, CodeDeploy, CodeGuru, CodePipeline, CodeStar, CodeWhisperer, X-Ray
- Management/Governance -- AppConfig, CLI, Cloud Development Kit, CloudFormation (CDK), CloudTrail, CloudWatch, CloudWatch Logs, System Manager
- Networking -- API Gateway, CloudFront, Elastic Load Balancing (ELB), Route 53, VPC
- Security/Identity/Compliance -- Certificate manager (ACM), Cognito, IAM, Key Management Service (KMS), Private Cert Authority, Secrets Manager, Security Token Service (STS), WAF
- Storage -- Elastic Block Store (EBS), Elastic File System (EFS), S3, S3 Glacier

### Features out of scope

- Analytics -- QuickSight
- Business -- Chime, Connect, WorkMail
- EndUser -- AppStream 2.0, Workspace
- Frontend -- Device Farm
- Game -- GameLift
- ML -- Lex, ML, Polly, Rekognition
- Management/Governance -- AWS Managed Services (AMS), Service Catalog
- Media -- Elastic Transcoder
- Migration/Transfer -- Application Discover Service, Application Migration Service, Database Migration Service
- Security/Identity/Compliance -- AWS Shield
- Storage -- Snow, Storage Gateway

## AWS Infrastructure -- <https://aws.amazon.com/about-aws/global-infrastructure/>

- Regions, Availability Zones "AZ" (3-6 per region, a-f), Data Centers, Edge/PoP
- Most services are region-scoped
  - Global: IAM, Route 53 (DNS), CloudFront CDN, Web App Firewall (WAF)
- Regions: compliance, proximity, availability, pricing

### IAM

- Groups contain (many-to-many) users
- Users and groups can be assigned (many-to-many) JSON policy documents (containing statements -- Effect(Allow,Deny)/Principal(account/user/role)/Action(service:method)/Resource triples)/Condition
- Roles assign permissions to trusted entities -- AWS service (e.g., EC2 or Lambda), AWS account, Web Identity, SAML 2.0 federation, etc --  to act on our behalf.
- Account-level IAM Credentials Report (.csv)
- User-level IAM Access Advisor
- Responsibility shared between AWS and account owner

### SDK

- JS, Python, PHP, .NET, Ruby, Java, Go, Node.js, C++
- Mobile -- Android, iOS
- IoT -- Arduino, Embedded C, ...

## EC2 -- Elastic Compute Cloud

- VMs -- EC2 (Linux, Windows, MacOS)
  - Bootstrap scrip -- EC2 User Data -- Installing updates, packages, common files
- Network attached storage -- EBS or EFS
- Hardware storage -- EC2 Instance Store
- Virtual Drives -- EBS
- Load Distribution -- ELB
- Autoscaling Group -- ASG
- Network: speed, Public IP, firewall
  - Security Groups allow network traffic (i.e., no "deny" rules)
  - Security Groups are for a specific region/VPC (Virtual Private Cloud) combination
  - Permitting another SG really permits EC2 instances with the other SG.
  - 21 (FTP), 3389 (RDP)

### Instance types (<https://instances.vantage.sh/>)

- t2.micro (1CPU, 1GiB, EBS only) -- 750 hours/month free
- t2.xlarge (4CPU, 16GiB, ESB only)
- r5.16xlarge (64CPU, 512GiB, 400NVMe SSD, 10Gbps network, 4.75Gbps EBS)
- Prefixes
  - C -- Compute
  - R,X,Z -- RAM (e.g. databases, etc)
  - I,D,H -- High bandwidth sequential read/write local storage

### Amazon Machine Image (AMI)

- Amazon Linux is free-tier-eligible

## Storage

### Elastic Block Storage (EBS) Volume

- Persistent network drive bound to an AZ (Availability Zone), unless you snapshot it, etc.
- Provisioned in GBs and IOPS (and billed by provisioned capacity, which can increase over time).
- May be "delete-on-termination" (default for root volumes)
- Multi-attach is beyond the scope of this class.
