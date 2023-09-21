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

### S3 Access Points

- Permissioning for different prefixes
- Each access point has a different DNS FQDN (public internet or VPC)
- VPC
  - VPC Endpoint needs to have s3:GetObject rights to both the bucket and the access point

### S3 Object Lambda

- Useful for redaction, enriching, and/or watermarking

## EC2 Instance Metadata Service -- IMDS

- IMDSv1 -- <http://169.254.169.254/latest/meta-data>
- IMDSv2 (EC2 instances can be configured to require this)
  - ``TOKEN=`curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600"` ``
  - `curl http://169.254.169.254/latest/meta-data/profile -H "X-aws-ec2-metadata-token: $TOKEN"`
- IAM Role name is available, but not the actual policy
  - `curl http://169.254.169.254/latest/meta-data/identity-credentials/ec2/security-credentials/$IAM_ROLE`
  - returns a JSON document with type "AWS-HMAC" and a SecretAccessKey and Token
- Userdata script is similarly available

## CloudFront (CDN)

- Hundreds of edge locations, TTL is about a day
- Protection
  - DDoS protection with Shield
  - (Optional) Web Application Firewall (WAF)
- Origins
  - S3
    - CloudFront Origin Access Control (OAC)
      - (Replaces Origin Access Identity (OAI))
    - Also for uploading
  - HTTP
    - ALB (Application Load Balancer)
    - EC2 instance
    - S3 static website
- "Distribution" options
  - Optionally compress all objects
  - Supports
    - http and https
    - redirect http to https
    - only https
  - Methods
    - Only GET,HEAD
    - Only GET,HEAD,OPTIONS
    - Only GET,HEAD,OPTIONS,PUT,POST,PATCH,DELETE
  - Edge locations
    - Everywhere
    - North America, Europe, Asia, Middle East, Africa
    - North America, Europe
  - Logging to an S3 bucket

### CloudFront Caching

- CreateInvalidation API (all files or a path)
- By default, query strings, http headers, and cookies are not part of the Cache Key
  - But if they are part of the Cache Key then they are passed to the origin
    - Or Origin Request Policy can define headers to pass to the origin even though it's not part of the Cache Key
- CloudFront can only access public ALB or EC2 instances (not VPC)
- CloudFront can exclude by country, etc.

### CloudFront Signed URLs / Signed Cookies

- Trusted signers (AWS accounts)
- Cookies may be for many files
- Two types of signers
  - Trusted key group (recommended)A -- APIs to create and rotate keys
    - Public Key uploaded to CloudFront to verify URLs
    - Private key used by apps (e.g. on EC2) to sign URLs via the Platform API
  - AWS Root Account with a CloudFront Key Pair (not recommended)

### CloudFront Origin Groups

- One primary and one secondary, with failover
  - Perhaps S3 buckets in two different regions (with replication to the secondary)

### CloudFront Field Level Encryption

- Specific fields in POST requests can be encrypted with a public key.

### CloudFront Logs

- Requests sent to Kinesis Data Streams
  - Thence to Lambda for real-time processing
  - Thence to Kinesis Data Firehose for near-real-time processing
- Can restrict to
  - Sampling fraction
  - Specified Cache Behaviors (path patterns) and/or field values

## Containers

### Docker

- Can run inside EC2 instance with a Docker Daemon
  - Limited isolation between containers on a host
- Image repos
  - Private
    - Amazon Elastic Container Registry (ECR)
  - Public
    - <https://hub.docker.com>
- Overview
  - Create Dockerfile, e.g. `FROM ubuntu:18.04; COPY . /app; RUN make /app; CMD python3 /app/app.py`
  - Build image and push to repo
  - Pull-and-run
- Services
  - Elastic Container Registry (ECR) -- Docker images backed by S3
    - Public <https://gallery.ecr.aws>
    - Private
      - EC2 instance role needs rights
    - Also
      - Vulnerability scanning
      - Versioning, Tags, Lifecycle
    - CLI
      - `aws ecr get-login-password --region $REGION | docker login --username AWS --password-stdin $AWS_ACCT_ID.dkr.ecr.$REGION.amazonaws.com`
      - `docker push $AWS_ACCT_ID.dkr.ecr.$REGION.amazonaws.com/$IMAGE:latest`
  - Elastic Container Service (ECS) -- proprietary platform
  - Elastic Kubernetes Service (EKS) -- managed Kubernetes
  - AWS Fargate -- serverless container platform (for both ECS and EKS)

### Elastic Container Service -- ECS

- "Launch ECS Tasks on ECS Clusters"
  - EC2 Launch Type -- Manual provision and maintain the EC2 instances, each running the ECS Agent (which registers the instance into the ECS Cluster)
    - IAM Roles
      - EC2 Instance Profile
        - Used by ECS Agent to interact with ECS service, CloudWatchLogs, pull from ECR, and Secrets Manager and/or SSM Parameter Store
      - ECS Task Role (per task)
        - Defined in the task definition.
    - A dedicated EC2 Auto Scaling Group
      - EC2 instance type (e.g. t2.micro)
      - OS/Arch (e.g. "Amazon Linux 2")
      - Min/Max number of instances
  - Fargate Launch Type -- Just create task definitions
    - Also Fargate Spot
  - ECS Anywhere
    - Even on-premises
- ALB (Application Load Balancer) supported
  - NLB only for very high performance cases, or with AWS Private Link
  - Classic Load Balancer not recommended and doesn't support Fargate
- ECS tasks can mount EFS file systems (but not of course S3)
  - Fargate + EFS is nice

### ECS Tasks

- Task Definition (for up to ten containers)
  - IsEssential
  - Image location
  - Port Bindings (container and host) and networking info
    - EC2
      - Port 0 ==> Dynamic Host Port Mapping (which ALB is fine with -- but not Classic Load Balancer)
        - EC2's Security Group must open all TCP ports
    - Fargate
      - Each task has a unique private IP
      - The ECS ENI (Elastic Network Interface) Security Group just needs to open 80 (or whatever)
  - Memory and CPU requirements
  - IAM Role (per Task Definition)
  - Environment variables
    - Hardcoded
    - SSM (Systems Manager) Parameter Store (e.g. API keys)
    - Secrets Manager (e.g. DB passwords)
    - From an S3 bucket ("bulk")
  - Logging configuration
    - CloudWatch
    - AWSFireLens to Firehose/KinesisStream/OpenSearch/S3
  - Trace Collection (via OpenTelemetry sidecar to AWS X-Ray)
  - Metrics Collection (via OpenTelemetry sidecar to CloudWatch or Managed Prometheus (with either Prometheus or OpenTelemetry libraries))
  - Data Volumes (bind mounts)
    - Share data between containers ("sidecars", e.g. Metrics/Logging) defined in the same Task Definition
    - Works for both EC2 and Fargate
    - Location
      - EC2 instance storage
      - Fargate ephemeral storage (20GiB default, up to 200GiB)
    - Q: So this is per-task and a task can contain multiple containers which scale together?
- Can be linked to one or more Launch Types
- Can be long-running "service" or a standalone "task" (e.g. a batch job)
- Can be fronted by an ALB
- Dedicated vCPU and RAM
- IAM
  - Create a Task role if the task will be using AWS services
  - Create a security group for the ALB allowing connections from users
  - Create a security group for the Tasks allowing traffic from the ALB security group

### ECS Auto Scaling (task level)

- Launch Types
  - Fargate
    - Easier!
  - EC2
    - Option: Auto Scaling Group Scaling -- adds EC2 instances over time
    - Option: ECS Cluster Capacity Provider (paired with an Auto Scaling Group)
- Uses AWS Application Auto Scaling
  - Average CPU
  - Memory utilization
  - ALB Requests per Target
- Target Tracking (CloudWatch metric)
- Step Scaling (CloudWatch alarm)
- Scheduled Scaling

### ECS Rolling Updates

- Min healthy percent (≤100%)
- Maximum percent (≥100%)

### ECS and Events

- Event Bridge
  - S3 Events can run ECS tasks which read S3 objects and write to DynamoDB
  - Scheduled Events can run ECS tasks
  - ECS task starts/stops are also events which can go via EventBridge to SNS (Simple Notification Service)
- SQS Queue
  - Tasks can poll an SQS Queue and ECS Service Auto Scaling can do its thing.

### ECS Tasks Placement

- Fargate doesn't need any of this nonsense.
- Task placement strategy
  - Binpack -- Greedy least available memory/CPU (to allow the fewest number of EC2 instances)
  - Random -- Amongst eligible containers
  - Spread -- Across AZ or instanceId.
  - May be combined, e.g. SpreadByAZ-then-Binpack or SpreadByAZ-then-SpreadByEC2Instance
- Task placement constraints
  - `distinctInstance`
  - `memberOf` (Cluster Query Language, e.g. `attribute:ecs.instance-type =~ t2.*`)
- Placement is best-effort.

### AWS Copilot

- Install from <https://github.com/aws/copilot-cli/releases/latest/download>
- CLI tool for AppRunner, ECS, or Fargate
  - Handles VPC, ELB, ECR
  - Integrates with CodePipeline
- Deploys to multiple environments, can enable logs, health status, etc.
  - Configures AWS using CloudFormation templates
  - Creates `copilot/web-app/manifest.yml`
- YAML-described microservices
- `copilot init`
  - Workload
    - Request-driven (App Runner)
    - Load Balanced (Fargate)
    - Backend (Fargate)
    - Worker (SQS to Fargate)
    - Scheduled (Events to State Machine to Fargate)
- `copilot env init`
- `copilot deploy`
- `copilot app delete`
- Q: is there a "dry mode"?

### Cloud 9

- Requires `AWSServiceRoleForAWSCloud9`

### Amazon Elastic Kubernetes Service (EKS)

- Aso supports EC2 or Fargate
- Node Types
  - Managed Node Groups
    - Nodes in an AutoScalingGroup managed by EKS
    - Supports On-Demand or Spot instances
  - Self-Managed nodes
    - Manually-created nodes registered to EKS cluster (and managed by ASG)
    - Manually-created AMI or Prebuilt Amazon EKS Optimized AMI
    - Supports On-Demand or Spot instances
  - AWS Fargate
    - Fully managed
- Data Volumes
  - `StorageClass` manifest for EKS cluster
  - Requires a Container Storage Interface (CSI) compliant driver
    - Fargate and EC2
      - EFS
    - EC2 only
      - EBS
      - FSx for Lustre
      - FSx for NetApp ONTAP

## Elastic Beanstalk (orchestration for environment creation, etc)

- Uses:
  - EC2 -- Scaled using ASG as usual
  - EIP (Elastic IP) -- Finally exposes <http://myap-$env.$suffix.$AZ.elasticbeanstalk.com>
  - ELB -- Either a dedicated instance or can share in an existing Load Balancer in your account
  - RDS -- The lifecycle of any linked RDS instance is the lifetime of the environment
- Manages: provisioning, load-balancing, scaling, health monitoring, instance configuration, etc
  - (CloudFormation effects stacks which orchestrate creation of resources using templates)
- Free, but you of course pay for everything you use
- Terms
  - "Application" -- EB components (environments, versions, configs, etc)
  - "Application Version"
  - "Environment"
    - Collection of AWS resources running one version at a time
    - Tiers
    - Instances -- Dev, Test, Prod, etc.
    - Modes -- Single-instance (for dev), HA with Load Balancer
      - Also, whether to use spot instances
  ```mermaid

  graph LR
  CreateApp-->UploadVersion-->LaunchEnvironment-->ManageEnvironment
  ```
- Platforms
  - Go, Java SE, Java Tomcat, .NET Core on Linux, .NET on Windows Server, Node.js, PHP, Python, Ruby
  - Docker (Single Container, Multi-Container, Pre-configured)
- Tiers
  - Web Server Environment Tier
    - ELB in from of Security Groups and EC2 instances in multiple AZ
  - Worker Environment Tier
    - EC2 instances pulling from an SQS queue
    - Scale based on SQS message count
    - Of course, Web Server Tier can publish to SQS queues
- IAM
  - EB itself uses an "aws-elasticbeanstalk-service-role", containing
    - Set the EC2 instance profile to be a new "aws-elasticbeanstalk-ec2-role" containing: `AWSElasticBeanstalkWebTier`, `AWSElasticBeanstalkWorkerTier`, `AWSElasticBeanstalkMulticontainerDocker`
      - (Granted `sts:AssumeRole` to the `ec2.amazonaws.com` service as a principal)
- Application update strategies -- <https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.deploy-existing-version.html>
  - Note: applications are typically just `.zip` files
  - Q: How does "Swap environment domain" work with persistent data?
  - All-at-once
  - Rolling
  - Rolling with additional batches
  - Immutable (new ASG, then swap)
    - Q: Why does the slide call this the "Longest deployment"???
  - Blue Green (entirely new environment, canaried and transitioned using Route53)
  - Traffic Splitting (canary testing)
    - One ALB, two ASG (equally-sized)
    - Rapid automated rollback.
- Configuration update strategies
  - Beyond scope of test
- Files
  - `cron.yaml`
    - Can hit an endpoint periodically, for example
  - `.ebextensions/`
    - `logging.config` (yaml)

### EB CLI

- `eb {create,status,health,events,logs,open,deploy,config,terminate}`
- (For automated deployment pipelines)

### EB Deployment Processes

- Describe dependencies
  - Python -- `requirements.txt`
  - Node.js -- `package.json`
    - Q: Shouldn't it really be `package-lock.json`???
- Package as zip and deploy (via S3)
- EB resolves dependencies

### EB Lifecycle Policy

- Stores up to 1000 versions
  - Can keep in S3 even when evicted from EB
    - `.elasticbeanstalk/`, `$ACCOUNT_ID-something.zip`, `resources/`
- Can use same aws-elasticbeanstalk-service-role, etc

### EB Extensions

- YAML or JSON files in `.ebextensions/*.config`
  - `option_settings:aws:elasticbeanstalk:application:environment.{DB_URL,DB_USER}`
- Can add RDS, ElastiCache, DynamoDB, etc -- but they are deleted if the environment is deleted.
  - Any CloudFormation-compliant resources
  - But best practice is to decouple RDS DBs.
  - Q: Is tombstoning required?

### EB Cloning

- Useful for creating a test version

### EB Migration (Load Balancer)

- Ugly
- Create entirely new environment
- Eventually need CNAME swap or Route 53 update.

## CloudFormation -- Infrastructure-as-code

- Automated destruction and recreation of infrastructure
- Automated diagrams
- CloudFormation does ordering and orchestration
- Separation of concerns
  - VPC stacks
  - Network stacks
  - App stacks

### CloudFormation Basics

- Templates uploaded to S3 and never modified.
- Stacks identified by name.
- Deleting a stack deletes all artifacts created by CloudFormation.
- Templates may be edited in CloudFormation Designer (and console to input parameters)
- Templates are editable YAML files which may be deployed via AWS CLI.
- Template Components
  - AWS resources (MANDATORY)
    - Resources to be created or modified (over 200 types!) -- AWS::aws-product-name::data-type-name -- <https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html>
      - Q: Update Udemy course to reflect new format?
    - Resources have `Type` and `Properties` and may reference each other. (No dynamic creation within the yaml itself.)
    - Resources may also have a `Condition` contingent upon a previously-defined condition
  - Parameters (dynamic inputs)
    - Type -- String, Number, CommaDelimitedList, List`<Type>`, AWSParameter (match against existing values in account)
    - Description (boolean)
    - Constraints -- Min/MaxLength, Min/MaxValue, Defaults, AllowedValues (array), AllowedPattern (regexp), NoEcho (boolean)
    - (used via `!Ref`)
      - Also pseudo-parameters: `AWS::{AccountId,Region,StackId,StackName,NotificationARNs,NoValue}`
  - Mappings (static configuration) A
    - (used via `!FindInMap [ MapName, TopLevelKey, SecondLevelKey ]`)
      - Eg `!FindInMap [RegionAndArchToImage, !Ref "AWS::Region", 32]`
        - Q: Update example to have better map name?
  - Outputs (optional references to created resources)
    - (used via `!ImportValue`
    - Value (most likely a `!Ref`)
    - Export.Name (public name) -- must be unique within region for your entire account
    - (Typical use: VPC ID and Subnet IDs)
    - NB: if outputs are used by another stack, that other stack must be deleted first
  - Conditions (for resource creation)
    - Examples: dev/test/prod, region, etc.
    - `Conditions:CreateProdResources: !Equals [!Ref EnvType, "prod"]`
      - `Fn::And`, `Fn::Equals`, `Fn::If` (ternary), `Fn::Not`, `Fn::Or`
  - Metadata
- Template helpers
  - References
  - Functions

### CloudFormation Stacks

- May import resources (beyond scope of exam!)
- Templates may be uploaded or an existing S3 URL.
- Out-of-scope for exam
  - Stack policy (protecting against unintentional updates)
  - Rollback configuration
  - Notification options
  - Stack creation options
- Notes
  - CF adds useful tags to generated resources, e.g. `aws:cloudformation:logical-id`, `aws:cloudformation:stack-id` (full arn), `aws:cloudformation:stack-name`
  - CF shows "Change set preview"!
    - Q: Why the hell doesn't it show the order in which the changes will occur???

### YAML

- `:` -- key/value
- `-` -- list item
- `|` -- multiline string
- `#` -- comments
- `!` -- tags

### CloudFormation Intrinsic Functions

- Q: Not "Intrisic"
- `Fn::Ref` -- resources (physical ID) and parameters
- `Fn::GetAtt`, e.g. `!GetAtt MyEC2Instance.AvailabilityZone`
- `Fn::FindInMap`, e.g. `!FindInMap [RegionAndArchToImage, !Ref "AWS::Region", 32]`
- `Fn::ImportValue`, e.g. `!ImportValue ExportedName`
- `Fn::Join`, e.g. `!Join [":", [x,y,z]]` results in "x:y:z"
- `Fn::Sub`, e.g. `!Sub 'arn:aws:ec2:${AWS::Region}:${AWS::AccountId}:vpc/${vpc}'`
  - (Also can supply specific mapping for local interpolations)
- Condition functions: `Fn::And`, `Fn::Equals`, `Fn::If` (ternary), `Fn::Not`, `Fn::Or`

### CloudFormation Rollbacks

- Stack Creation error 
  - Default is rollback, but can disable for troubleshooting

- Stack Update error
  - Default is rollback to last known-working state, but can allow newly-created resources to stay (for debugging)
  - Logs are useful.

### CloudFormation to SNS Topic

- Once enabled, all events go to the topic (but can filter via Lambda to a 2nd topic)

### CloudFormation -- advanced topics

- ChangeSets -- Essentially `--dry_run`
- Nested stacks (best practice)
  - Component reuse
  - Inner stack is essentially private
- Cross stacks
  - When resources have different life-cycles
  - `Fn:ImportValue`
  - E.g. passing export values to many stacks (e.g. VPC Id)
- StackSets
  - Across multiple accounts and/or regions
- Drift
  - Caused by manual changes
  - Q: Can we periodically monitor all stacks for drift?
- Stack policies
  - Can protect resources (e.g. RDS) from updates, etc.

## Integration and Messaging

- SQS (oldest AWS service -- queue), SNS (pub/sub), Kinesis (real-time streaming)

### SQS -- Simple queueing service

- Producers (calling SendMessage API) and Consumers (EC2 polling for up to 10 at a time, servers, Lambda)
  - Delete-after-processing
- Duration 4 days (max 14)
- Latency < 10ms
- Messages < 256KB, plus attributes
- Types
  - Standard Queue
    - At least once delivery; best-effort ordering
    - Unlimited throughput, unlimited message count
  - FIFO
    - Exactly-once
    - (In-order for each message group ID)
    - Queue Name must end with ".fifo"
    - Throughput limited to 300msg/s without batching; 3000msgs/s with batching
    - Deduplication based on de-dup ID or SHA-256
- Configuration
  - Visibility timeout (30s; 0-12h)
    - Time to process
    - Extend with `ChangeMessageVisibility`
  - Delivery delay (0s; 0-15m)
    - Also can be per-message
  - Receive message wait time (0s; 0-20s) -- Long Polling!
  - Message retention period (4d; 1m-14d)
  - Max message size (256KB; 1KB-256KB)
    - SQS Extended Client (Java) uses S3 as a workaround
    - More generally, just send metadata around
  - DLQ
- Offers an `ApproximateNumberOfMessages` metric suitable for ASG scaling.
  - Also ApproximateAgeOfOldestMessage, ApproximateNumberOfMessagesDelayed, ApproximateNumberOfMessagesNotVisible, ApproximateNumberOfMessagesVisible, ApproximateNumberOfEmptyReceives, NumberOfMessageDeletes, etc.
- In-flight: https
- At-rest: KMS
- SQS Access policies (similar to S3 bucket policies)
  - Cross-account access
  - Delegating rights to services (SNS, S3, and basically everything else)
  - Statement
    - Effect (Allow/Deny)
    - Principal (Accounts, IAM Users, IAM Roles)
    - Action (e.g. `sqs:ReceiveMessage`)
    - Resource (e.g. `an:aws:sqs:us-east-1:4444555556666:myQueue`)
    - Condition (e.g. `ArnLike:"aws:SourceArn":"arn:aws:s3:*:*:bucket1", StringEquals:"aws:SourceAccount":"bucket_owner_id"`)
  - API calls: CreateQueue, DeleteQueue, PurgeQueue, SendMessage, ReceiveMessage, ChangeMessageVisibility DeleteMessage
    - Batch API for SendMessage, DeleteMessage, ChangeMessageVisibility
    - ReceiveMessage can get up to 10 messages

### SNS - Simple Notification Service

- Pub/Sub
- 12,500,000 subs per topic; 100,000 topics per account (or larger!)
  - Also offers FIFO
    - <300 publishes/s
  - Also offers Message Filtering per subscription
    - Message attributes
    - Message body (if JSON)
- Subscribers 
  - Email
    - Q: Can this be disabled...seems like an easy exfiltration modality
  - SMS/Mobile notifications
  - HTTP(S) endpoints
  - SQS
  - Lambda
  - Kinesis Data Firehose (but not Kinesis Data Streams)
- Publishers
  - CloudWatch (alarms)
  - ASG (notifications)
  - CloudFormation (state changes)
  - AWS Budgets
  - S3 Bucket (events)
  - DMS (new Replica)
  - Lambda
  - DynamoDB
  - RDS (events)
- Publishing
  - Topic Publish (using normal SDK)
  - Direct Publish (mobile apps SDK)
- Encryption
  - HTTPS for in-flight
  - At-rest using KMS or AWS-managed keys
- Policies similar to SQS and S3 Buckets
- Examples
  - SNS+SQS fan-out 
    - SQS policy must allow SNS to write
    - Cross-region delivery is quite possible
    - Example: S3 events, since they can have one event rule per event type and prefix
  - SNS to Kinesis Data Firehose to S3

### Kinesis Data Streams

- Kinesis Data Streams - Capture, process, store 
  - Fixed sharding defines throughput 
    - Producer -- 1k msgs/s, 1MB/s
    - Shared Client -- 2k msgs/s, 2 MB/s (across all consumers)
      - Pull -- Usual `GetRecords()` API
      - Minimize cost
    - Enhanced Client -- 2k msgs/s, 2 MB/s (per consumer)
      - Push over HTTP/2 -- New `SubscribeToShard()` API
      - Soft limit of 5 KCL per data stream
  - Modes
    - Provisioned (pay per shard-hour)
    - On-demand (newer)
      - Up to 200MiB/s 
      - Scaled based on 30d peak.
      - Pay per stream-hour and per GB
  - Producers
    - AWS SDK, KPL (Kinesis Producer Library), Amazon Kinesis Agent
      - KPL: C++, Java, batch, compression, retries
      - Kinesis Agent: Tails logs files.
      - PutRecord API; batching reduces cost and increases throughput
      - AWS CLI -- `aws kinesis put-record --stream-name $NAME --partition-key $CUSTOMER_ID`
  - Consumers
    - Apps (KCL, SDK)
      - Kinesis Client Library (KCL)
        - Java library
          - KCL 1.x -- shared consumer
          - KCL 2.x -- shared consumer and enhanced fan-out consumer
        - One to many relationship from KCL client to shards.
        - Progress checkpointed into DynamoDB
        - Runs on EC2,EB, on-premises
        - Record read in order in each shard
    - Managed (Lambda, Kinesis Data Firehose, Kinesis Data Analytics)
      - Lambda (GetBatch()), e.g. to DynamoDB.
        - Both Classic and Enhanced fan-out
    - AWS CLI with `aws kinesis get-shard-iterator` and then `aws kinesis get-records --shard-iterator $SHARD_ITERATOR` (return base-64 payloads _and_ a `NextShardIterator`)
      - NB: PartitionKey is returned with each record
  - VPC Endpoints are available for resources within VPC
  - Records
    - Partition key, sequence number, data blob (<1MB)
      - `Shard # = hash(partition_key) % num_shards`
    - Retention 1-365 days
    - Data Immutability
    - Same partition key --> same shard
  - `ProvisionedThroughputExceeded`
    - Retries with exponential backoff
    - Increase sharding
  - Shard splitting (e.g. if shard #2 is hot, manually transition from `[1,2,3]` to `[1,4,5,3]`. Shard #2 will be deleted once all data in it is expired). Only 1 into 2.
  - Shard merging, e.g. `[1,4,5,3]` to `[6,5,3]`. Only 2 into 1.

### Kinesis Data Firehose

- Load into AWS data stores
  - Fully managed, autoscaled, no replay, pay for usage
- Source: Applications, Clients, SDK, KPL, Kinesis Agent, Kinesis Data Streams, CloudWatch Logs, CloudWatch Events, AWS IoT
  - Only data published after Firehose configured.
- Codeless and fully-managed batch writes
  - S3, Redshift (via S3), Amazon OpenSearch
    - Default S3 prefix of YYYY/MM/dd/HH (UTC)
  - Also 3rd-party destinations: Datadog, Splunk, New Relic, MongoDB
  - Also http endpoint
- Also all (or just failing) data to a backup S3 bucket
- Records <= 1MiB
  - Near-real-time (60s or 1MiB latency)
  - Supports data conversions, transformations, compression -- plus custom Lambda transformations
    - JSON to Apache Parquet (read-optimized, and wider support) or Apache ORC (write-optimized, plus allows update/delete)
      - (based on schema in AWS Glue)
    - But first use Lambda to convert to JSON if necessary
- Dynamic partitions based on partitioning keys (extra charge)
- Buffering
  - Buffer size: 5MB (1MB-128MB)
    - Smaller results in higher costs but lower latency
  - Buffer interval: 300s (60s-900s)
    - Smaller results in higher costs but lower latency
- Compression
  - Optional gzip, snappy, zip, Hadoop-compatible snappy
- Encryption (KMS)
- Can create IAM role with necessary S3 perms
- Up to 50 tags

### Kinesis Data Analytics (KDA) - Analyze with SQL or Apache Flink

- SQL Applications (fully managed, pay for consumption)
  - Source: Kinesis Data Streams or Kinesis Data Firehose
  - SQL can join in reference data stored in S3 for real-time enrichment
  - Target: Kinesis Data Streams (to Lambda or app) or Kinesis Data Firehose
  - Uses: Time-series analytics, real-time metrics and dashboards
- Amazon Managed Service for Apache Flink
  - Use Fink (Java, Scala, or SQL) to process and analyze streaming data
  - Can read in Kinesis Data Streams and Amazon MSK (Managed Kafka)
  - Runs on a managed cluster; provides checkpoints and snapshots
- Q: What type of aggregations are allowed?

### Kinesis Video Streams - Capture, process, and store

## Monitoring, Troubleshooting, Auditing -- CloudWatch, X-Ray, CloudTrail

- CloudWatch
  - Metrics
    - Builtin metrics: CPUUtilization, NetworkIn, ....
      - In the `AWS` namespace
      - Dimensions (up to 30): instance ID, environment, etc
      - Timestamps
        - EC2 default to "every 5m", but can pay for "every 1m" (e.g. t scale ASG faster)
    - Custom metrics
      - `PutMetricData`
        - Accepts 2w in the past to 2h in the future, so TZ matters
          - Non-current data takes much longer to show on graphs, etc.
      - In a custom namespace
      - Dimensions (up to 10): `Instance.id`, `Environment.name`, etc.
      - Resolution
        - Standard: 60s
        - High Resolution: 1s, 5s, 10s, 30s
    - Notes
      - Free Tier allows 10 detailed monitoring metrics
      - EC2 Memory usage must be pushed from inside the instance as a custom metric
  - Logs
    - Log Streams within Log Groups
      - Log Groups, e.g. `/aws-glue/crawlers`, `/aws/datasync`, `/aws/lambda/$LAMBDA`
        - Expiration: never, or 1d-10y
        - Encrypted by default (KMS optional)
    - Sources
      - SDK
      - CloudWatch Logs Agent (older) (EC2 and on-premises)
        - Requires appropriate IAM perms
      - CloudWatch Unified Agent (EC2 and on-premises)
        - Can also send system-level metrics (CPU, Disk, RAM, swap, netstat, processes, etc)
        - Configurable using SSM Parameter Store
      - Elastic Beanstalk
      - ECS containers
      - Lambda
      - VPC Flow Logs
      - API Gateway
      - CloudTrail (filtered)
      - Route53 (DNS queries)
    - Metric Filter
      - Synthetic metrics based on query (up to three dimensions). Not retroactive.A
      - Can trigger CloudWatch Alarm
    - Destinations
      - Export 
        - S3 (CreateExportTask can take up to 12h!)
        - CloudWatch Logs Subscriptions (real-time, supports filtering)
          - Up to two subscription filters per log group (Q: Why so low?)
          - Subscription Destination (from other accounts)
            - "Destination Access Policy" in recipient account, and an IAM Role which can be assumed by the Sender
          - Lambda
          - Kinesis Data Streams (even from multiple subscription filters from multiple accounts)
            - Kinesis Data Analytics (KDA)
            - EC2
            - Lambda
          - Kinesis Data Firehose (KDF)
            - S3 (near real-time)
            - OpenSearch
      - CloudWatch Logs Insights 
        - Auto-detects fields from AWS services and JSON log events
        - Custom query language
          - `fields @timestamp, @messages | sort @timestamp desc | limit 20`
          - `filter @message like /Exception/ | stats count(*) as exceptionCount by bin(5m) | sort exceptionCount desc`
          - `fields @message | filter @message not like /Exception/`
        - Save queries to CloudWatch Dashboards
          - Queries can span Log Groups from multiple AWS accounts
          - Query engine -- not real-time engine
  - Alarms (trigger notification)
    - States: OK, INSUFFICIENT_DATA, ALARM
    - Period (length of time to evaluate metric): 10s, 30s, $$N$$ minutes
    - Actions
      - EC2 (e.g. restarting instance) 
      - EC2 Auto Scaling
      - Amazon SNS
      - Composite Alarms! (AND, OR)
    - `aws cloudwatch set-alarm-state` useful for testing
    - CloudWatch Synthetics Canary
      - Manually reproduce critical user journeys -- checks success, availability, latency
      - Scripts in Node.js or Python
      - Programmatic access to headless Google Chrome instance. (Q: Chromium???)
      - May run periodically
      - Blueprints
        - Heartbeat Monitor
        - API Canary
        - Broken Link CHecker
        - Visual Monitoring vs baseline
        - Canary Recorder (Q: Node.js or Python???)
        - GUI Workflow Builder
  - Amazon EventBridge (formerly "CloudWatch Events")
    - Schema registry (including versioned and inferred schema), which can generate code bindings (Java, TypeSCript, Python, Go)
    - Policies
      - Can aggregate events from other accounts and/or regions.
    - Sandbox
    - Sources 
      - Scheduled (cron)
      - Typical Events (may be filtered)
        - IAM -- root user sign-in
        - EC2 -- instance start
        - CodeBuild -- failure
        - S3 -- object upload event
        - Trusted Advisor -- new finding
        - CloudTrail -- anything
    - Destinations
      - Aggregation: Another event bus
      - Compute: Lambda, AWS Batch, Launch ECS Task
      - Integration: SQS, SNS, Kinesis Data Stream
      - Maintenance: SSM, EC2 Actions
      - Orchestration: Step Functions, CodePipeline, CodeBuild
      - Optionally archive (by default indefinitely), which allows replaying
    - Buses!
      - AWS services publish to the default event bus
      - AWS SaaS Partners (e.g. zendesk, datadog, salesforce) publish to a partner event bus
- X-Ray (Distributed traces)
  - Compatibility: Lambda, Elastic Beanstalk, ECS, ELB, API Gateway, EC2, even on-prem
  - Traces, segments, subsegments, annotations (indexed) and metadata (not indexed)
    - Every request, or %, or rate
    - Default: 1st request each second (reservoir) and 5% of rest (rate)
      - Changeable dynamically
    - Q: Any ways to force resulting calls to also be traced?
  - Enabling
    - Java/Python/Go/Node.js/.NET -- import the X-Ray SDK (and minimal code modification)
      - `AWS_XRAY_DAEMON_ADDRESS` tells the SDK where to publish to (typically port UDP 2000)
      - X-Ray daemon (intercepting UDP) 
      - Manually identify segment boundaries, etc
    - Lambda
      - X-Ray must be imported and "Active Tracing" enabled
    - Elastic Beanstalk
      - `.ebextensions/xray-daemon.config` (`option_settings:aws:elasticbeanstalk:xray:XRayEnabled:true`), or use the console.
      - Application of course needs the X-Ray SDK
      - (Daemon not provided for Multi-container Docker)
    - ECS (Multiple options)
      - XRay Agent per EC2
      - XRay Container as a sidecar for each App Container
      - Fargate -- can only use sidecar
    - Of course, in all cases, needs IAM rights to write to X-Ray
  - API
    - Writing -- `PutTraceSegments`, `PutTelemetryRecords`, `GetSamplingRules`, `GetSamplingTargets`, `GetSamplingStatisticSummaries`
    - Reading -- `GetSampling{Rules,Targets,StatisticSummaries}`, `Get{Group,Groups,ServiceGraph,TimeSeriesServiceStatistics}`, `GetTrace{Graph,Summaries}`, `BatchGetTraces`
- AWS Distro for OpenTelemetry
- CloudTrail
  - API call monitoring
  - AWS Resource audit trails
