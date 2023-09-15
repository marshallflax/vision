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
  - I,D,H -- High bandwidth sequential read/write local storage ("Instance Store")

### Amazon Machine Image (AMI)

- Amazon Linux is free-tier-eligible

---

## Storage

### Elastic Block Storage (EBS) Volume

- Persistent network drive bound to an AZ (Availability Zone), unless you snapshot it, etc.
  - Only attachable to EC2 instances in same AZ, of course.
  - Detachment "recommended" before snapshotting. Snapshotting required to copy EBS volume to another AZ.
  - Snapshots can be "archived" with 24h-72h recall but 75% discount.
  - Deleted snapshots (and deleted AMIs) reside 1d-365d (per Retention Rule) in the "Recycle Bin"
  - Fast Snapshot Restore is expensive.
- Provisioned in GBs and IOPS (and billed by provisioned capacity, which can increase over time).
- May be "delete-on-termination" (default for root volumes)
- Multi-attach is beyond the scope of this class.
- Volume Types -- (Most have 0.1% - 0.2% annual failure rate)
  - gp2/gp3 (SSD) (eligible as a boot volume)
    - gp2 1GiB to 16TiB; 3 IOPS/GiB
    - gp3 3k IOPS to 16k IOPS; 125 MiB/s to 1000 MiB/s (independently)
  - io1/io2 (high-performance SSD) -- low-latency and/or high-throughput (eligible as a boot volume)
    - Up to 32k IOPS (and 64k for Nitro EC2)
    - io2 has better durability (0.001% annual failure) and higher IOPS/GiB
    - "io2 Block Express" has latency <1ms, and up to 256k IOPS.
    - Supports EBS Multi-attach (in same AZ)
      - Up to 16 R/W attachments, e.g. Teradata, but only with cluster-aware filesystem (e.g. Elastic File System) -- not XFS, EXT4, etc.
  - st1 (HDD) -- low-cost throughput-intensive
    - Max 500 IOPS, 500 MiB/s -- data warehouses, log processing
  - sc1 (HDD) -- "cold" lowest-cost less-frequently accessed
    - Max 250 IOPS, 250 MiB/s -- data warehouses, log processing

### Amazon Machine Image

### EC2 Instance Store

- Ephemeral data store physically connected to the EC2 instance.

### Elastic File System -- EFS

- Only for Linux instances.
- POSIX-compatible managed NFSv4.1, surrounded by a Security Group
  - Scales automatically to PiB, supports 1000s of concurrent NFS clients, >10GiB/s throughput
- Available cross-AZ to Linux instances (not Windows)
- 3x cost of gp2 (but EFS-IA is available) -- pay-per-use
  - content management, web serving, data sharing, Wordpress, etc.
- Optional KMS encryption-at-rest
- Performance Modes (not for Elastic)
  - General purpose (default)
  - Max I/O -- for highly-parallel work (higher throughput but higher latency)
- Throughput can be:
  - Bursting
  - Provisioned (but throughput bill can be big bucks)
  - Elastic (Recommended) up to 3GiB/s read and 1GiB/s write
- Storage classes
  - Standard
  - Infrequent Access (EFS-IA) -- can be moved to this tier after N (7/14/30/60/90) days without being accessed and then automatically returned to Standard upon access.
- Availability/Durability
  - Standard (a/k/a Regional) -- Multi-AZ (for prod)
  - Single-AZ (backup enabled by default). Also EFS One Zone-IA is 90% cost savings.
- Automatic backups are extra.

### Network access to EFS File Systems

- Recommendation: one mount target per AZ, each controlled by its own Security Group
- File System Policies
  - May protect against root access.
  - May be readonly.
  - May prevent anonymous access.
  - May require in-flight encryption.
- Typically mounted to `/mnt/efs/fs1` (not for `/`)
- Typically requires `yum install efs-utils` (at least behind-the-scenes)

## Scalability and High Availability

- Vertical vs horizontal scaling.
