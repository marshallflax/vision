# AWS Developer (Associate, DBA-C02)

## Overview

- Develop Using
  - Service APIs
  - CLI
  - SDKs
- Deploy Using
  - CI/CD pipelines

---

### Domain 1 -- Development

#### Task 1 -- Application code

- Patterns
  - Microservices vs monolithic, Fan-out, Idempotency, stateful, stateless, loose coupling
  - Event-driven, choreography and orchestration, synchronous and asynchronous
  - Fault-tolerance, e.g. exponential back-off, jitter, DLQ
  - Streaming
- APIs: response/request, validation, status codes
- Serverless Application Model (AWS SAM) unit testing
- Messaging, APIs, SDKs

#### Task 2 -- Lambda code

- Testing, Event source mapping, event-driven architectures, scalability, stateless apps, unit testing, VPC private resources
- Lambda function configuration via parameters/environment variables -- memory, concurrency, timeout, runtime, handler, layers, extensions, triggers, destinations, tuning.
- Integration with services, destinations, DLQ

#### Task 3 -- Data stores

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

### Domain 2 -- Security

#### Task 1 -- Authentication and Authorization

- Federated -- Security Assertion Markup (SAML), OpenID Connect (OIDC), Amazon Cognito
- Tokens -- JSON Web Token (JWT), OAuth, AWS Security Token Service (STS)
- Cognito -- user pools and identity pools
- Policies -- resource, service, principal, Role-based access control (RBAC), ACL, least privilege, customer vs AWS managed

#### Task 2 -- Encryption at rest and in transit

- Certificates -- AWS Private Cert Authority
- Key Management -- rotation, AWS-managed, customer-management
- SSH keys

#### Task 3 -- Sensitive data

- Classification -- PII, PHI
- Env variables
- Secrets management -- AWS Secrets Manager, AWS Systems Manager Parameter Store
- Credential handling
- Sanitization

---

### Domain 3 -- Deployment

#### Task 1 -- Artifacts

- Configuration data -- AWS AppConfig, Secrets Manager, Parameter Store
  - Dependencies -- container images, config files, env variables
  - Memory, cores
- Lambda -- packaging, layers, configuration
- Git -- AWS CodeCommit
- Container Images

#### Task 2 -- Dev environments

- Application deployment
- Mock endpoints
- Lambda aliases, development endpoints
- Deploying to existing environments

#### Task 3 -- Automated deployment testing

- API Gateway states
- CI/CD branches and actions
- Automated testing -- unit, mock
- Creating test events
- Integration testing -- Lambda aliases, container image tags, AWS Amplify branches, AWS Copilot envs.
- Infrastructure as Code (IaC) -- SAM templates, CloudFormation templates
- API Gateway environments

#### Task 4 -- Code deployment

- Git, AWS CodeCommit, labels and branches
- AWS CodePipeline workflow and approvals
- AppConfig and SecretsManager for app configs
- CI/CD workflow
- API Gateway -- stages, custom domains
- Strategies -- canary, blue/green, rolling, rollbacks, dynamic deployments

---

### Domain 4 -- Troubleshooting and Optimization

#### Task 1 -- Root Cause

- Logging and monitoring -- CloudWatch Logs Insights, CloudWatch Embedded Metric Format (EMF)
- Visualizations, AWS X-Ray
- Code analysis tools
- HTTP codes, Common SDK exceptions

#### Task 2 -- Instrumentation

- Structured logging, distributed tracing (and annotations)
- Logging, monitoring, observability
- Application metrics -- custom, embedded, builtin
- Notification alerts (e.g. quota, or deployment completion)

#### Task 3 -- Optimization

- Caching (using request headers), concurrency, messaging (Simple Queue Service, Simple Notification Service, filtering)
- Profiling -- determining memory and compute requirements

---

### Scope

#### Features in scope

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

#### Features out of scope

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

### API/SDK

- Many SDKs
  - JS, Python, PHP, .NET, Ruby, Java, Go, Node.js, C++
  - Mobile -- Android, iOS
  - IoT -- Arduino, Embedded C, ...
- Official CLI uses Python (boto)
- SDK defaults to `us-east-1`

### CLI Notes

- `aws configure --profile newProfile`
  - `aws s3 ls` vs `aws s3 ls --profile newProfile`
- MFA Requires `aws sts get-session-token --serial-number mfa-device-arn --token-code current-code-from-token --duration-seconds 3600` ... then copy results into an `aws configure --profile myMFA` -- and then in `.aws/credentials` add an `aws_session_token` line to the `myMFA` stanza
- Credentials Provider Chain, in decreasing priority
  - `--region`, `--output`, `--profile`
  - `$AWS_ACCESS_KEY_ID`, `$AWS_SECRET_ACCESS_KEY`, `$AWS_SESSION_TOKEN`
  - `~/.aws/credentials`
  - `~/.aws/config`
  - Container credentials (for ECS tasks)
  - Instance profile credentials (for EC2)
- SDK (e.g. Java)
  - Java system properties -- `aws.accessKeyId` and `aws.secretKey`
  - `$AWS_ACCESS_KEY_ID`, `$AWS_SECRET_ACCESS_KEY`
  - `~/.aws/credentials`
  - `~/.aws/config`
  - Container credentials (for ECS tasks)
  - Instance profile credentials (for EC2)
- Best practices
  - EC2 Instance Roles for EC2 instances
  - ECS Roles for ECS tasks
  - Lambda Roles for Lambda functions
  - Environment variables or named-profiles when working outside of AWS

### AWS Limits (Quotas)

- API Rate Limits
  - EC2.DescribeInstances -- 100/s
  - S3.GetObject -- 5500 GET/s per prefix
  - Exponential Backoff is your friend when you receive a `ThrottlingException` or 5xx
    - (Built into the SDK)
  - Quota increases can be requested
- Service Limits
  - 1152 vCPU on-demand standard instances
  - Open a ticket
- Service Quotas
  - Use the Service Quotas API

### Signing AWS requests

- SDK and CLI do this for you; some public S3 requests don't need to be signed
- SigV4 (Signature v4)
  - Can be provided in `-H Authorization:`
  - Can be query string (pre-signed URL, good for up to 12h (S3 console) or 7d (CLI)) -- `X-Amz-Security-Token`, `X-Amz-Algorithm`, `X-Amz-Credential`, `X-Amz-Date`, `X-Amz-Signature`, and `X-Amz-Expires`

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

## Block Storage

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
- HA (in at least two AZ)

### Load Balancing

- Elastic Load Balancer (managed)
  - Single DNS entry, e.g., `XXX.region.elb.amazonaws.com`
  - Client IP in `X-Forwarded-For` and `X-Forwarded-Proto`
  - Downstream health check (typically `http:4567/health`)
  - SSL termination
  - HA over AZ
  - May serve public or private.
    - EC2 instances should be configured to allow traffic only from load balancer security group.
  - Integrates with EC2, EC2 Autoscaling, ECS (elastic container), AWS Cert Manager, CloudWatch, Route53, AWS WAF (firewall), AWS Global Accelerator (global IP endpoints)
- Four flavors
  - v1 -- Classic Load Balancer (deprecated) -- HTTP, HTTPS, TCP, SSL
  - v2 -- Application Load Balancer (Layer 7) -- HTTP, HTTPS, HTTP/2, WebSocket
    - Can route traffic to "target groups" (which may be a container within an EC2 instance, even with a dynamic port)
      - Or ECS Tasks, or Lambda functions, or any Private IP (on-premises)
    - By URL (path or host), query string, headers
      - Rules have priorities
      - <100 rules/ALB, <=5 condition values/rule, <=5 wildcards/rule, <=5 weighted target groups/rule
    - Can redirect `http:` to `https:`
    - Choice of HTTP1, HTTP2, or gRPC
    - Stickiness w/ cookies (but that of course can unbalance load)
      - Application cookies generated by target (per target group) (eschew AWSALB, AWSALBAPP, AWSALBTG)
      - Load balancer cookie (AWSALBAPP)
      - Load balancer duration-based (AWSALB)
    - By default does cross-zone balancing (no additional charge!)
  - v2 -- Network Load Balancer (Layer 4) -- TCP, TLS, UDP
    - High performance
    - One static IP per AZ (perhaps from Elastic IP)
    - Can direct to an ALB
    - Downstream health checks can be TCP, HTTP, HTTPS
      - Healthy, unhealthy threshold -- 2-10
      - Timeout -- 10s (2s-120s)
      - Interval -- 30s (5s-300s)
    - By default does not do cross-zone balancing (additional charge)
  - v2 -- Gateway Load Balancer -- IP (GENEVE encapsulation -- port 6081)
    - Route to 3rd-party appliances: Firewalls, IDS, Deep Packet Inspection, Payload manipulation, etc.
    - By default does not do cross-zone balancing (additional charge)

### SSL/TLS Certs

- Load balancer terminates SSL with X.509 cert, usually from AWS Certificate Manager (ACM)
- SNI (Server Name Indication)
  - Supported by ALB, NLB, CloudFront -- not Classic LB
- De-registration Delay (a/k/a Connection Draining) -- 300s (1s-3600s)

### Auto Scaling Group (ASG)

- Horizontal EC2 scaling ("scale-out" and "scale-in")
  - Integrated with AWS load balancers, which can restart unhealthy instances
- Launch Template
  - AMI
  - EC2 User Data (script)
  - EBS Volumes
  - Security Groups
  - SSH Keys
  - IAM Roles
  - Network and subnets
  - Load Balancer
  - Min/Max/Initial Size
  - Dynamic Scaling policies (with CloudWatch)
    - Average CPU across all instances in group, custom metric, etc.
      - Target Tracking Scaling
      - CloudWatch, e.g. "add two units if CPU > 70%"
    - Metrics
      - `CPUUtilization`, `RequestCountPerTarget`, Average Network In (or Out)
  - Predictive Scaling (based on _predicted_ metrics)
  - Scheduled Actions (based on known or expected usage patterns)
  - (Capacity Optimized allocation might help with Spot instances.)
  - Cooldown after scaling (default 300s)
  - Supports "Instance Refresh" for transition (with minimum healthy percentage and warmup time) to updated launch template.

## Amazon Relational Database Service (RDS)

- Managed: Postgres, MySQL, MariaDB, Oracle, Microsoft SQL Server, Aurora (AWS proprietary)
- Auto-provisioned, patched, Point-in-time restore, monitoring, read replicas, Multi-AZ, maintenance windows
- Horizontal and vertical scaling, backed by EBS (gp2 or io1)
- Can be configured to have connectivity to an EC2 "compute resource" and/or EV2 instances
- Authentication can require IAM or Kerberos
- Logs can be exported into CloudWatch

### RDS Autoscaling

- Storage
  - Maximum Storage Threshold
- Increase storage if free space is <10% over >5min and >6h since last increase.

### RDS Read Replicas

- Up to 15 replicas (in any AZ or region -- but pay for cross-region network)
- Async replica, so eventual consistency
- Replicas can be promoted to standalone r/w db instances.

### RDS Multi-AZ for DR

- Behind a single DNS alias for availability -- synchronous replication.
- Not for read replicas, though a single instance can be both ("Multi-AZ Cluster")
- No downtime necessary to promote DB to multi-AZ

### RDS Aurora

- API-compatible with Postgres (3x improvement) or MySQL (5x improvement)
- Storage grows in 10GiB increments up to 128TiB
- <10ms replica lag for up to 15 replicas (autoscaled!) via a Reader Endpoint.
- <30s master failover (DNS) -- Writer Endpoint
- 6 copies across 3AZ; 4 copies needed for writes, 3 for reads
  - Striped across 100s of volumes
- Support for Cross-Region Replication
- Optional ("Backtrack") Point-in-time restores without backups
- CloudWatch: audit, error, general, slow query

### RDS/Aurora security

- Encryption-at-rest using AWS KSM at initial launch (or snapshot-and-restore)
- In-flight encryption -- supports AWS TLS root certs
- IAM authentication or username/password
- No SSH except RDS custom.
- Audit logs may be sent to CloudWatch Logs (longer retention)

### RDS Proxy

- Only VPC connections
- Serverless, autoscaling, HA, faster failover
- Connection pooling, especially for Lambda functions
- Allows non-Aurora instances to require IAM authentication

### Amazon Elastic Cache

- Managed Redis or Memcached
- Can be on-premise (using AWS Outposts)
- Requires application code (and especially an invalidation strategy)
- Redis
  - Multi AZ transactional log, autofailover, read replicas (up to 5), durability (using append-only files), Sets and Sorted Sets.
  - 10's of GiB to 100's of TiB
- Memcached
  - Sharded, no replication, no persistence, no backup/restore
- <https://aws.amazon.com/caching/best-practices/>
  - Lazy Loading (a/k/a Cache-Aside, Lazy Population)
  - Write-through (for smaller keyspaces)
  - Russian-doll
  - Eviction Strategies
    - allkeys-{lfu,lru,random} (Least Frequently Used, Least Recently Used)
    - volatile-{lfr,lru,ttl,random}
    - no-eviction
  - Thundering herd

## Route 53 (DNS)

- A, AAAA, CNAME (not for Zone Apex), NS
- CAA, DS, MX, NAPTR, PTR, SOA, TXT, SPF, SRV
- Zone File, Name Serve, TLD, SLD (second-level domain), FQDN, protocol, URL
- Route 53 as authoritative DNS
- Hosted Zones (public or private, within VPC), 0.50 USD/month per hosted zone
- Aliases records (free) for ELB, CloudFront, API Gateway, Elastic Beanstalk, S3, VPC Interface Endpoints, Global Accelerator, Route 53 in same zone -- but not EC2
- Routing policies
  - Simple (no health checks)
  - Weighted (proportional, useful for canarying)
  - Latency (network latency)
  - Failover (active/passive)
  - Geo-location
  - Multi-value (up to 8 healthy records returned) -- "not a substitute for ELB"
  - IP-based (CIDR)
  - Geo-proximity (positive bias increases Voronoi region size; -99 to +99)
  - May be nested (e.g. multiple failover strategies, each with a different weight)
- Health checks
  - Endpoints (apps, servers, AWS resources)
    - Interval 30s (10s more expensive)
    - HTTP, HTTPS, TCP
    - At least 18% must be healthy
    - Threshold of 3 (default)
    - 2xx, 3xx, or based on text in the first 5k of the response.
    - Must allow Route53 Health Checkers access to endpoints -- <https://ip-ranges.amazonaws.com/ip-ranges.json> (ROUTE53_HEALTHCHECKS)
  - Calculated
    - Based on up to 256 child checks
  - CloudWatch Alarms
    - Necessary for endpoints only accessible from VPC

## Virtual Private Cloud (VPC)

- VPC (per Region), Subnet (per AZ) (public or private), Internet Gateway (used by public-subnet resources)
  - Private subnets: NAT Gateway (AWS-managed) and NAT Instances (user-managed) -- both reside in public subnet
- Security Groups
  - Stateful -- always allows return traffic
  - ALLOW rules only based on both IP addresses and security groups
  - Elastic Network Interface and EC2 instances
- Network ACL (NACL)
  - Stateless
  - ALLOW and DENY rules based only on IP addresses
  - Everything in the subnet
- VPC Flow Logs, Subnet Flow Logs, Elastic Network Interface Flow Logs
  - Also Managed resources: ELB, ElastiCache, RDS, etc.
  - Can be stored in S3, CloudWatch Logs, Kinesis Data Firehose
- VPC Peering
  - Cross-account and/or cross-region
  - CIDR must not overlap
  - Peering is not transitive, so $O(n^2)$
- Private access to AWS services: VPC Endpoint Gateway (S3 and DynamoDB) and VPC Endpoint Interface
- Site-to-site VPN (over public internet), Direct Connect (DX) (takes >1 month to establish)

### Typical 3-tier architecture

- Tier 1 -- Public subnet -- ELB
- Tier 2 -- Private subnet -- Autoscaling group with one subnet per AZ (Linux, Apache, PHP)
- Tier 3 -- Data subnet -- EBS, EFS, ElastiCache, RDS (MySQL)

## S3 (Simple Storage Service)

- Infinitely-scaling storage and backbone
- Backup, storage, DR, archive, Hybrid Cloud storage, App hosting, Media, Data lakes, Big Data, Software delivery, static websites
- Objects in globally-uniquely-named buckets (created in a specific region)
  - Bucket naming: no uppercase, no underscore, 3-63 chars, not an IP.
  - Key is full path
  - Objects <5000GB (but multi-part upload for >5GB) with metadata key/value pairs, up to ten unicode key/value tags, and perhaps version ID.

### S3 Security

- User-based (API calls per IAM user)
  - DENY in IAM Policy trumps bucket policy
- Resource-based
  - Bucket-wide rules, even cross-account
    - JSON-based
    - List of Statements (Effect/Principal/Actions/Resources (e.g. `arn:aws:s3:::myBucket/*`))
      - Effect: Allow or Deny
      - Principal: IAM User or IAM Role (e.g. EC2 instances)
      - Action, e.g. `s3:GetObject`, `s3:PutObject`
      - Condition, e.g. `"Null"`, `"StringEquals"`, `"ArlLike"`, `"ForAllValues:StringNotEquals"` with parameters, `"s3:x-amz-server-side-encryption-aws-kms-key-id": "true"`, `"s3:x-amz-acl": ["public-read"]`, `"aws:SourceArn": "arn:aws:s3:::EXAMPLE-SOURCE-BUCKET"`, or `"aws:PrincipalServiceNamesList": "logging.s3.amazonaws.com"`
    - Can mandate encryption
    - Can be prevented from granting access to public, even for the entire account
  - Object ACL
  - Bucket ACL (uncommon)

### S3 Versioning

- Enabled at bucket level (even after bucket created)
- Protects against unintended deletes, allows rollback to previous version (even if versioning is subsequently disabled)
- Deleting an object actually adds a "delete marker" (visible when "show versions" is enabled)

### S3 Replication

- Asynchronous
  - Requires versioning in source and destination; version IDs are replicated
  - Chaining not allowed.
  - Only objects created after replication enabled; S3 Batch Replication for existing objects.
  - Deletion replication is optional; permanent deletions blocked
- Same-region (SRR) and Cross-region (CRR)
  - May apply to all objects in bucket, or whose which pas a filter
  - Target can have different ownership, storage class, encryption, etc.
  - RTC (Replication Time Control) does 99.99% within 15m -- but more USD
  - Cloudwatch metrics available -- but more USD
- S3 must be granted necessary IAM perms (usually to an auto-created IAM Role)
- Q: Many-to-one???

### S3 Storage classes (per Object)

- Standard -- 99.99% availability
- Standard IA (Infrequent Access) -- 99.9% availability
  - Lower fixed cost, but cost-upon-retrieval
- Single Zone Infrequent Access -- 99.5% availability
  - Data lost if AZ destroyed
- Glacier (minimum storage duration 90 days)
  - Instant Retrieval -- millisecond retrieval for data used once / quarter
  - Flexible Retrieval
    - Expedited (1m - 5m)
    - Standard (3h - 5h)
    - Bulk (5h-12h)
  - Deep Archive (minimum 180 days)
    - Standard (12h)
    - Bulk (48h)
- Intelligent Tiering
  - No retrieval charges, but does have monitoring and auto-tiering fees
  - Frequent tier
  - Infrequent tier after 30d
  - Archive Instant after 90d
  - Archive (optional) after 90+d
  - Deep Archive (optional) after 180+d

### S3 Durability

- Multi-AZ and single-AZ: 11 9's.
- Object Lock prevents deletion or overwriting; governance mode even blocks root user.

### S3 Lifecycle rules

- Parameters
  - All objects or filtered
  - All objects or specific tags
  - All objects or minimum/maximum size
  - Days after object creation
  - Current versions; current versions; deleted noncurrent versions; incomplete multipart uploads
- (Per-byte and per-object) cost for transitioning to Glacier Flexible or Glacier Deep
- Q: Are lifecycle rules just JSON?

### S3 Moving between Storage Classes

- Transition to lower classes some number of days after creation
  - Separate rules for current and noncurrent versions
- Expiration policies to delete objects
  - Days after creation
  - Old versions
  - Incomplete multi-part uploads
  - Can filter by prefix and/or tags
- Examples
  - Transition non-current versions to Standard IA and then Glacier Deep Archive
- Storage Class Analysis
  - Available only for Standard and Standard IA (not single-AZ or Glacier)
  - Updated daily, with 24h-48h lag

### S3 Event Notification

- s3:TestEvent, s3:ObjectCreated:{Put,Post,Copy,CompleteMultipartUpload}, s3:ObjectRemoved:{Delete,DeleteMarkerCreated}, s3:ObjectRestore, s3:Replication...
  - May be filtered by object name
- Sent to SNS, SQS, or Lambda
  - Typically O(seconds) but could be minutes or longer
- SNS/SQS (Simple Notification/Queue Service)
  - SNS/SQS Resource Policy (giving s3.amazonaws.com right to `SNS:Publish`/`SQS:SendMessage` to a given topic/queue from an `aws:SourceArn` of the relevant bucket)
- Lambda
  - Lambda Resource Policy (giving s3.amazonaws.com right to "lambda:InvokeFunction" to a given function from an `aws:SourceArn` of the relevant bucket)
- Or via Amazon EventBridge
  - Allows filtering by metadata, object size, name
  - Multiple destinations
    - Step Functions
    - Kinesis Streams/Firehose
    - etc
  - Allows: Archive, Replay, Reliable delivery

### S3 Baseline Performance

- Typical latency is 100ms-200ms latency
- Typical rate is 3.5k/s PUT/COPY/POST/DELETE or 5.5k/s GET/HEAD per prefix
- Many prefixes per bucket
- Multipart uploads can be parallelized
  - Suggested >100MB, required >5GB

### S3 Transfer Acceleration

- Compatible with multi-part upload
- Cache in edge location

### S3 Byte-Range Fetches

- Parallelizable
- Only part of the file

### S3 Select and Glacier Select

- Server-side filtering using SQL!
- Input in CSV or JSON (optionally gzip or bzip2), or Apache Parquet
- Output in CSV or JSON
- Server-side encrypted objects

### S3 User-Defined Object Metadata + Object Tags

- Metadata
  - `Content-Length`, `Content-Type`, and user Metadata names `x-amz-meta-`
- Object Tags
  - Fine-grained permissions
  - Analytics
- Filtering not possible for either metadata or tags

### S3 Object Encryption

- In transit
  - Use https, duhA
  - Can be enforced by Denying traffic with `aws:SecureTransport` of `false`
- Server-Side Encryption
  - (Default) SSE-S3 -- Keys managed by S3
    - Requires `x-amz-server-side-encryption: AES256`
  - SSE-KMS -- Keys stored in KMS
    - Requires `x-amz-server-side-encryption: aws:kms`
    - Beware of KMS quotas (5.5k/10k/30k per sec depending on region)
    - Allows auditing using CloudTrail
    - "Bucket Keys" reduce KMS calls
  - DSSE-KMS -- double encryption
  - SSE-C -- Keys provided by customer
    - Encryption key passed in every https header
    - https of course mandatory
- Client-Side

### S3 MFA Delete

- Only root account can enable/disable
- Only for versioned buckets

### S3 Access Logs

- All requests -- both authorized or denied
- Logged to a 2nd (to avoid infinite loops) S3 bucket in same region
  - BucketOwner, Bucket, Time, IP, Requestor, RequestID, Operation (SOAP.op, REST.method.resource_type, WEBSITE.method.resource_type, BATCH.DELETE.OBJECT, S3.action.resource_type), Key (object name), RequestURI, HTTPStatus, S3ErrorCode, BytesSent, ObjectSize, TotalTime, S3Time, Referrer, UserAgent, VersionId, HostId, SignatureVersion, CipherSuite, AuthType, HostHeader, TLSVersion, AccessPointARN, AclRequired

### CORS (browser-based) and CSRF (server-based)

- "Origin" -- protocol + domain + port
- Browser blocks traffic to second site unless second site has a matching Access-Control-Allow-Origin response to a preflight request
- Remember: S3's API is mostly just GET
- CORS Policy -- JSON document with Allowed{Headers,Methods,Origins} and MaxAgeSeconds

## EC2 Instance Metadata Service -- IMDS

- IMDSv1 -- <http://169.254.169.254/latest/meta-data>
- IMDSv2 (EC2 instances can be configured to require this)
  - ``TOKEN=`curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600"` ``
  - `curl http://169.254.169.254/latest/meta-data/profile -H "X-aws-ec2-metadata-token: $TOKEN"`
- IAM Role name is available, but not the actual policy
  - `curl http://169.254.169.254/latest/meta-data/identity-credentials/ec2/security-credentials/$IAM_ROLE`
  - returns a JSON document with type "AWS-HMAC" and a SecretAccessKey and Token
- Userdata script is similarly available
