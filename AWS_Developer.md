# AWS Notes

## Overview

### Courses

- CCP -- Certified Cloud Practitioner
- SAA -- Certified Solutions Architect Associate
- DVA -- Certified Developer Associate (DVA-C02)
  - <https://d1.awsstatic.com/training-and-certification/docs-dev-associate/AWS-Certified-Developer-Associate_Exam-Guide.pdf>
  - Develop Using: Service APIs, CLI, SDK
  - Deploy Using: CI/CD pipelines
- SOA -- Certified SysOps Administrator Associate
- ANS -- Advanced Networking Specialty (ANS-C01)
- SAP -- Certified Solutions Architect Professional
- DOP -- Certified DevOps Engineer Professional (DOP-C02)

### Domain 1 -- Development (DOP-C02)

#### Task 1 -- Application code

- Patterns
  - Microservices vs monolithic, Fan-out, Idempotency, stateful, stateless, loose coupling
  - Event-driven, choreography (event-driven) and orchestration (centrally-controlled), synchronous and asynchronous
  - Fault-tolerance, e.g. exponential back-off, jitter, DLQ
  - Streaming
- APIs: response/request, validation, status codes
- Serverless Application Model (AWS SAM) unit testing
- Messaging, APIs, SDKs

#### Task 2 -- Lambda code

- Testing, Event source mapping, event-driven architectures, scalability, stateless apps, unit testing, VPC private resources
- Lambda function configuration via parameters/environment variables -- memory, concurrency, timeout, runtime, handler, layers (for deployment), extensions, triggers, destinations, tuning.
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

### Domain 2 -- Security

#### Task 1 -- Authentication and Authorization

- Federated -- Security Assertion Markup (SAML, using XML), OpenID Connect (OIDC, using JWTs), Amazon Cognito
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

### Scope

#### Features in scope

- Analytics -- Athena, Kinesis, OpenSearch
- App Integration -- AppSync, EventBridge, Simple Notification Service (SNS), Simple Queue Service (SQS), Step Functions
- Compute -- EC2, Elastic Beanstalk, Lambda, Serverless Application Model (SAM)
- Containers -- Copilot, Elastic Container Registry (ECR), Elastic Container Service (ECS), Elastic Kubernetes Service (EKR)
- Database -- Aurora, DynamoDB, ElastiCache, MemoryDB for Redis, RDS
- Developer -- Amplify, Cloud9, CloudShell, CodeArtifact, CodeBuilt, CodeCommit, CodeDeploy, CodeGuru, CodePipeline, CodeStar, CodeWhisperer, X-Ray
- Management/Governance -- AppConfig, CLI, Cloud Development Kit, CloudFormation (CDK), CloudTrail, CloudWatch, CloudWatch Logs, System Manager
- Networking -- API Gateway, CloudFront, Elastic Load Balancing (ELB), Route 53, VPC
- Security/Identity/Compliance -- Certificate manager (ACM), Cognito, IAM, Key Management Service (KMS), Private Cert Authority, Secrets Manager, Security Token Service (STS), Web Application Firewall (WAF ... before ALB, CloudFront, or API Gateway)
- Storage -- Elastic Block Store (EBS), Elastic File System (EFS), S3, S3 Glacier

#### Features out of scope

- Analytics -- QuickSight
- Business -- Chime, Connect, WorkMail
- Comm -- Pinpoint (SMS gateway)
- EndUser -- AppStream 2.0, Workspace
- Frontend -- Device Farm
- Game -- GameLift
- ML -- Lex, ML, Polly, Rekognition
- Management/Governance -- AWS Managed Services (AMS), Service Catalog
- Media -- Elastic Transcoder
- Migration/Transfer -- Application Discover Service, Application Migration Service, Database Migration Service
- Security/Identity/Compliance -- AWS Shield
- Storage -- Snowball, Storage Gateway

## AWS Infrastructure -- <https://aws.amazon.com/about-aws/global-infrastructure/>

- Regions, Availability Zones "AZ" (3-6 per region, a-f), Data Centers, Edge/PoP
  - But really does vary by region -- us-west-1 only [has](https://devopslearning.medium.com/aws-ebs-volumes-gp2-vs-gp3-io1-vs-io2-which-one-to-choose-7177e59fff3c) two available AZ.
- Most services are region-scoped, VPC-scoped, or VPC+AZ-scoped
  - Global: IAM, Route 53 (DNS), CloudFront CDN, Web App Firewall (WAF)
  - Regional: S3, DynamoDB, SNS, SQS
  - VPC: ELB, VPN, Internet Gateway
  - VPC+AZ: EC2, RDS, RedShift (data warehouse), EBS
- Regions: compliance, proximity, availability, pricing

### IAM

- Groups contain (many-to-many) users
- Users and groups can be assigned (many-to-many) JSON policy documents (containing statements -- Effect(Allow,Deny)/Principal(account/user/role)/Action(service:method)/Resource triples)/Condition
- Roles assign permissions to trusted entities -- AWS service (e.g., EC2 or Lambda), AWS account, Web Identity, SAML 2.0 federation, etc -- to act on our behalf.
- Account-level IAM Credentials Report (.csv)
- User-level IAM Access Advisor
- Responsibility shared between AWS and account owner
- Principles
  - Any explicit `DENY`, then any `ALLOW`, else `DENY`
  - S3 bucket policies are unioned in and then the above rule applies
- Types
  - AWS-Managed -- Good for power users and admins
    - Automatically includes new services when appropriate
  - Customer-managed
    - Version-controlled
    - Q: CloudTrail
    - May apply to many principals
    - May be dynamic, e.g. `"Resource": ["arn:aws:s3:::myCompany/home/${aws:username}/*"]`
  - Inline
    - 1-1 one relationship between policy and principal
    - Not shown in IAM console!
- IAM roles passed to AWS Services
  - Requires `iam:PassRole` (and `iam:GetRole` to see what's being passed)
  - Requires that the service trusts (i.e. `sts:AssumeRole`) the role
- "AWS Directory Service" (AD as a forest of trees of objects -- Users, Computers, Printers, File Shares, Security Groups)
  - AWS Managed Microsoft AD
    - Supports MFA
    - May have mutual-trust with on-prem AD
  - AD Connector
    - Supports MFA
    - Proxies on-prem AD
  - Simple AD (Samba)
    - AD-compatible
    - Standalone
- Principals
  - `"principal": {"AWS": "$ACCT"}` -- every principal in the account
  - `"principal": {"AWS": "arn:aws:iam::$ACCT:root"}`
  - `"principal": {"AWS": "arn:aws:iam::$ACCT:user/$USER"}`
  - `"principal": {"AWS": "arn:aws:iam::$ACCT:role/$ROLE"}`
  - `"principal": {"AWS": "arn:aws:sts::$ACCT:federated-user/$USER"}`
  - `"principal": {"AWS": "arn:aws:sts::$ACCT:assumed-role/$ROLE/$ROLE_SESSION"}`
  - `"principal": {"Federated": "cognito-identity.amazonaws.com"}`
  - `"principal": {"Federated": "arn:aws:iam::$ACCT:saml-provider/$PROVIDER"}`
  - `"principal": {"Service": ["ecs.amazonaws.com", "elasticloadbalancing.amazonaws.com"]}`

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
  - Mounted on only a single EC2 instance at a time.
  - Bound to a specific AZ
  - Has a provisioned capacity (and that's how you're billed)
  - Root EBS volumes are (by default) deleted when the EC2 instance terminates
  - Snapshot-able (and hence copying across AZ or region)
    - Snapshot archive (24h-72h to restore)
    - Recycle bin (optional)
    - Fast Snapshot Restore (FSR) ($$$)
- Load Distribution -- ELB
- Autoscaling Group -- ASG
- Network: speed, Public IP, firewall
  - Public IPs are chosen randomly from AWS's pool of public IPv4 addresses.
    - "Elastic IP" if you need static public IPs (billed when unused until released)
  - Security Groups allow network traffic (i.e., no "deny" rules)
  - Security Groups are for a specific region/VPC (Virtual Private Cloud) combination
  - Permitting another SG really permits EC2 instances with the other SG.
  - 21 (FTP), 3389 (RDP)
  - | Security Group     | Network ACL                                      |
    | ------------------ | ------------------------------------------------ |
    | Per instance       | Entire subnet (additional layer of defense)      |
    | Only ALLOW         | ALLOW and DENY                                   |
    | Looks at all rules | Lowest-number matching rule applies              |
    | Stateful           | Stateless. Must explicitly allow return traffic. |

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
  - CLI
    - `aws ec2 describe-volume-status --volume-ids $VOLUME_ID`
    - `aws ec2 modify-volume -volume-type gp3 -iops 10000 -size 500 -volume-id $VOLUME_ID`
  - gp2/gp3 (SSD) (eligible as a boot volume)
    - gp2 1GiB to 16TiB; 3 IOPS/GiB
    - gp3 3k IOPS to 16k IOPS; 125 MiB/s to 1000 MiB/s (independently)
      - Usually cheaper and [preferable](https://devopslearning.medium.com/aws-ebs-volumes-gp2-vs-gp3-io1-vs-io2-which-one-to-choose-7177e59fff3c) to gp2
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

- Only for Linux instances -- NFSv4.1
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
  - Downstream health check (typically `http:/health`)
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
    - IP-and-port-based stickiness

### SSL/TLS Certs

- Load balancer terminates SSL with X.509 cert, usually from AWS Certificate Manager (ACM)
  - Traffic between CloudFront and Origins may be HTTPS-only or match-viewer
- SNI (Server Name Indication)
  - Supported by ALB, NLB, CloudFront -- not Classic LB
- De-registration Delay (a/k/a Connection Draining) -- 300s (1s-3600s)

### Auto Scaling Group (ASG)

- Horizontal EC2 scaling ("scale-out" and "scale-in")
  - Integrated with AWS load balancers, which can restart unhealthy instances
- Launch Template
  - AMI + Instance Type
  - EC2 User Data (script running as root)
  - EBS Volumes
  - Security Groups
  - SSH Keys
    - Default linux user is `ec2-user`
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

- But CPU is manually-determined by the underlying EC2 type.
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
- MariaDB, Oracle, SQL Server, PostgreSQL, and MySQL support Single-AZ and Multi-AZ with one standby.
  - PostgreSQL and MySQL also support two readable standbys

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
- No SSH access except RDS custom.
- Audit logs may be sent to CloudWatch Logs (longer retention)

### RDS Proxy

- Pools database connections
- Only connections from within VPC
- Serverless, autoscaling, HA, faster failover
- Connection pooling, especially for Lambda functions
- Allows non-Aurora instances to require IAM authentication

### Amazon Elastic Cache

- Managed Redis or Memcached
  - Redis
    - Multi AZ transactional log, autofailover, read replicas (up to 5), durability (using append-only files), Sets and Sorted Sets.
    - 10's of GiB to 100's of TiB
  - Memcached
    - Sharded, no replication, no persistence, no backup/restore
- Can be on-premise (using AWS Outposts)
- Requires application code (and especially an invalidation strategy)
- <https://aws.amazon.com/caching/best-practices/>
  - Lazy Loading (a/k/a Cache-Aside, Lazy Population)
  - Write-through (for smaller keyspaces)
  - Russian-doll caching
    - Cache both entities and their aggregations, etc.
  - Eviction Strategies
    - allkeys-{lfu,lru,random} (Least Frequently Used, Least Recently Used)
    - volatile-{lfr,lru,ttl,random}
    - no-eviction
  - Thundering herd
    - Prewarm cache when possible
    - Jitter TTL and expiration date/time

## Route 53 (DNS)

- (DNS traditionally uses TCP/UDP port 53)
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
  - IP-based (CIDR ranges)
  - Geo-proximity (positive bias increases Voronoi region size; -99 to +99)
  - May be nested (e.g. multiple failover strategies, each with a different weight)
- Health checks
  - Endpoints (apps, servers, AWS resources)
    - Interval 30s (10s more expensive)
    - HTTP, HTTPS, TCP
    - Must be healthy at least 18% of the time
    - Threshold of 3 (default)
    - 2xx, 3xx, or based on text in the first 5k of the response.
    - Must allow Route53 Health Checkers access to endpoints -- <https://ip-ranges.amazonaws.com/ip-ranges.json> (ROUTE53_HEALTHCHECKS)
  - Calculated
    - Based on up to 256 child checks
  - CloudWatch Alarms
    - Necessary for endpoints only accessible from VPC

## Virtual Private Cloud (VPC)

- VPC (per Region), Subnet (per AZ) (public or private), Internet Gateway (used by public-subnet resources)
  - For a given account, each region has a default VPC
  - Private subnets: NAT Gateway (AWS-managed) and NAT Instances (user-managed) -- both reside in public subnet
    - NAT Gateway -- Highly-Available within AZ
      - Reside within public subnets
      - NAT Gateway is resilient within AZ, but requires an instance within each AZ for fault-tolerance
      - Requires an Elastic IP
      - Pay per hour, usage, bandwidth
      - 5 Gps up to 100 Gbps
      - Only NACL matters (not security groups)
      - Supports TCP, UDP, ICMP
    - NAT Instance on EC2
      - Can use Amazon Linux NAT (AMI) ... choose the appropriate EC2 size for your workload
        - Can optionally provide port forwarding and act as a bastion server
        - Can have its own Security Group
      - Requires Elastic or Public IP
      - Must disable Source/Destination checks on the EC2 instance, e.g. `aws ec2 modify-instance-attribute --no-source-dest-check --instance-id=${EC2_INSTANCE_ID}`
  - Up to 200 subnets per VPC
- CIDR ("Classless Inter-Domain Routing")
  - Defines both VPC and subnets
    - Default VPC is `172.31.0.0/16` (part of RFC1918's `172.16.0.0/12`)
      - Default subnets typically `172.31.0.0/20`, `172.31.16.0/20`, `172.31.32.0/20`, etc.
      - Default VPC may be deleted (and then perhaps recreated)
      - VPCs may have one primary and four secondary IPv4 ranges (each between `/16` and `/28`) and one IPv6 range
        - Secondary ranges useful when the primary range is full, or when your primary range overlaps with some other VPC you wish to peer to.
        - Cannot mix RFC1918 ranges -- `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`, but can add "normal" CIDRs
        - `100.64.0.0/10` is a shared address space for ISPs used for carrier-grade NAT.
          - Note the `/10` ... it extends through `100.127.255.255`
      - NB: If primary subnet lies in `100.64.0.0/10`, then RFC1918 CIDRs may _not_ be secondary ranges
    - Your VPCs might be `10.xx.0.0/16`
  - Default IPv4 subnets are `/20`
    - IPv4 is mandatory for VPCs; IPv6 is optional.
      - All IPv6 VPC blocks are `/56` and all IPv6 subnets are `/64`
      - IPv6 addresses not supported by Amazon-provided DNS, AWS Site-to-Site VPN and customer gateways, NAT, and VPC endpoints.
  - `0.0.0.0/0` useful for opening to entire Internet
  - Subnets can be configured to automatically get a public IPv4.
    - Subnets can be associated with a routing table. Those without a routing table use the VPC's routing table.
  - _Five_ reserved IPs within a subnet (so a `/28` has 11 rather than 16)
    - 0th -- Network address (router)
    - 1st -- Reserved for VPC router
    - 2nd -- Reserved for AWS-provided DNS
    - 3rd -- Reserved for future AWS use
    - Last -- Reserved for broadcast (but broadcast not allowed)
- Security/Firewalls
  - Routing
    - Each VPC has a "local" router responsible for traffic between subnets
    - By default, all subnets may communicate
    - Don't forget to associate routing tables with subnets
    - Targets include
      - Egress Only Internet Gateway
      - Instance (i.e. NAT EC2 Instance)
      - Internet Gateway
      - NAT Gateway
      - Network Interface
      - Outpost Local Gateway
      - Peering Connection
      - Transit Gateway
      - "Blackhole" -- Nonexistent targets, e.g. deleted NAT gateways
  - Security Groups
    - Operates at the EC2 (actually ENI) granularity
    - Stateful -- always allows return traffic
    - Default behavior is to block inbound and allow outbound
    - ALLOW rules only based on both IP addresses and security groups
    - Elastic Network Interface and EC2 instances
    - Denial results in connection timeout.
  - Network ACL (NACL)
    - Operates at the subnet granularity
    - Stateless
    - ALLOW and DENY rules based only on IP addresses. Lowest-number rule which matches wins.
    - Everything in the subnet
- VPC Flow Logs, Subnet Flow Logs, Elastic Network Interface Flow Logs
  - Also Managed resources: ELB, ElastiCache, RDS, etc.
  - Can be stored in S3, CloudWatch Logs, Kinesis Data Firehose
- VPC Peering
  - One VPC requests peering and the other accepts
    - Q: How can this be orchestrated with CDK???
  - Cross-account and/or cross-region
  - CIDR must not overlap; only one peering between any two VPCs
- Peering among VPCs is not transitive, so $O(n^2)$; Direct Connect traffic doesn't go to peers (and ditto for Internet Gateway, NAT Gateway, S3/DynamoDB VPC Endpoint, etc)
- Private access to AWS services
  - VPC Endpoint Gateway (S3 and DynamoDB)
  - VPC Endpoint Interface
  - VPC PrivateLink
  - Transit VPC
  - Transit Gateway
- Site-to-site VPN (over public internet), Direct Connect (DX) (takes >1 month to establish)
- AWS PrivateLink (VPC Endpoint Services) (SaaS)
  - Connectivity between VPCs, AWS, on-premises, and AWS partner solutions without using public internet.
    - No overlapping-CIDR restrictions
  - Allows exposing a Network Load Balancer-fronted service to 1000s of VPCs (your or other accounts)
    - VPC Peering only scales to 100s.
    - An "Endpoint Service" exposes an NLB
  - On the other side of the NLB we of course can also have on-prem services via VPN or DX
    - We call the VPC containing the NLB an "Edge VPC"
  - | PrivateLink                | VPC Peering            |
    | -------------------------- | ---------------------- |
    | Expose single service      | Expose entire VPC      |
    | Overlapping CIDR fine      | CIDRs must be disjoint |
    | No limit                   | Limit of 125 peerings  |
    | Consumer initiates traffic | Bidirectional          |
- Note: Private subnets can't even do a `yum update`, so it is vital to have up-to-date AMI
- Q: What's a Virtual Private Gateway (VGW)???

### Elastic Network Interface (ENI)

- A virtual network interface (`Eth0`, `Eth1`, ...), typically for an EC2 instance, with:
  - One primary private IPv4 and perhaps secondary IPv4 as well (up to 8, depending on machine type)
  - Each private IPv4 may have an Elastic IP as well
  - Up to one public IPv4A
  - Perhaps multiple private and IPv6 addresses (e.g. for hosting containers)
  - One or more security groups attached to each ENI
  - One MAC address
  - One source/destination check flag
- Secondary ENIs may be created independently and then moved from EC2 to EC2 within an AZ.
  - Some AWS services (e.g. RDS, Elastic Kubernetes Control Plane, AWS Workspaces, AWS Appstream) automatically create their own (requestor-managed) ENIs
    - This is essentially just an example of cross-account ENIs
  - Maximum number of ENIs determined by EC2 machine type
    - Q: Does this include the requestor-managed ENIs?
- Use cases
  - Management network
  - Dual-homing
  - HA (High Availability) -- just move ENI to hot standby
- Not supported
  - NIC teaming

#### Bring Your Own IP

- e.g., to keep your IP reputation (e.g. whitelisting or warm standby)
- Regional Internet Registry (RIR), e.g. ARIN/RIPE/APNIC, must create a ROA (Rout Origin Authorization) to ASN 16509 and ASN 14618
- Limits
  - IPv4 -- Only `/24` or larger
  - IPv6 -- Only `/48` or larger (publicly advertised) or `/56` or larger for non-publicly-advertised (but Direct Connect is possible)
  - Up to five ranges per region per account

### VPC with Single Public Subnet

- Single Public Subnet
  - Suppose VPC is `10.100.0.0/16` and public subnet is `10.100.0.0/24` with an Internet gateway attached to it, and an EC2 instance has an Elastic IP.
  - | Destination     | Target  |
    | --------------- | ------- |
    | `10.100.0.0/16` | local   |
    | `0.0.0.0/0`     | igw-xxx |
- And then perhaps also a private subnet
  - Add a 2nd subnet (private), perhaps `10.100.1.0/24`
  - Security group allows 22 from `10.0.0.0/24`
  - | Destination     | Target  |
    | --------------- | ------- |
    | `10.100.0.0/16` | local   |
    | `0.0.0.0/0`     | nat-xxx |

### Typical 3-tier architecture

- Tier 1 -- Public subnet -- ELB
  - Still has private primary CIDR, but routing table allows access to an Internet Gateway instance
- Tier 2 -- Private subnet -- Autoscaling group with one subnet per AZ (Linux, Apache, PHP)
- Tier 3 -- Data subnet -- EBS, EFS, ElastiCache, RDS (MySQL, Aurora), DynamoDB

### VPC Endpoints

- Removes the need for IGW, NAT GW, etc to access AWS; avoids public internet
  - Managed, horizontally-scaled, redundant, highly-available, without bandwidth constraint
- VPC Gateway Endpoints
  - Allow connectivity from VPC to S3/DynamoDB without an internet gateway or NAT device
  - Does not use AWS PrivateLink; no additional charge
  - Requires a routing-table entry to an `pl-xxxx` destination
    - Outbound 80 and 443 traffic to the `pl-xxxx` destination must be enabled in Security Group
  - Has attached IAM policy (e.g. `AmazonS3FullAccess`)
    - Can restrict which S3/DynamoDB actions (or buckets/tables, etc) are allowed
    - Default is permissive
    - S3 bucket policies can conversely filter on `aws:sourceVpce` (and actually `aws:sourceVpc` as well) using `StringNotEquals` conditions, but that's uglier.
      - Note: S3 bucket policies can filter by public IP or elastic IP but not private IP.
- VPC Gateway Interface
  - Creates an ENI in each private subnet
  - A typical local router might have
    | Destination | Target |
    | ----------- | ------ |
    | `10.10.0.0/16` | Local |
    | `0.0.0.0/0` | Internet Gateway |
    - Each subnet can have its own route table
- VPC Interface Endpoint
  - Only supports IPv4; only supports TCP
  - AWS creates Regional and Zonal (useful for avoiding inter-AZ charges) DNS entries for the presented service
  - Uses Security Groups (inbound rules, of course; 443, of course)
  - 0.01 USD/hour + 0.01 USD/GiB
  - Uses a subnet IP
    - For HA (and to avoid inter-AZ data charges) be sure to create an Interface Endpoint per AZ
    - Can be accessed via DC (Direct Connection), AWS-managed VPN, VPC peering
      - But for "private DNS name" to work the peered VPC needs to be attached to a custom Route53 Private Hosted Zone
      - Or an on-prem DNS server needs to forward requests to a Route53 Resolver
      - See <https://docs.aws.amazon.com/whitepapers/latest/building-scalable-secure-multi-vpc-network-infrastructure/welcome.html>
  - Q: Can one Interface Endpoint present multiple AWS services?
- Exposing your services to other VPCs
  - Service must be behind a Network Load Balancer
  - Service can be behind a VPN/DX to on-premise
- Note: "Private DNS name" (really enabling a private DNS zone in Route53) for a VPC Interface Endpoint actually just overrides the public DNS (e.g., `athena.us-east-1.amazonaws.com`)
  - Requires "Enable DNS hostnames" and "Enable DNS Support
  - Alternative is to specify an explicit `--endpoint-url`

### VPC DNS and DHCP

- Typical Public DNS: "ec2-13-232-197-112.ap-south-1.compute.amazonaws.com" (but "compute-1" rather than "us-east-1.compute")
- Typical hostname and Private DNS: "ip-10-10-0-211.ap-south-1.compute.internal" (but ."ec2" rather than "us-east-1.compute")
  - `/etc/resolv.conf` (populated based on `domain-name` and `domain-name-servers` within the VPC's DHCP Options Set)
    - `search ap-south-1.compute.internal` (created by `/usr/sbin/dhclient-script`)
    - `options timeout:2 attempts:5`
    - `nameserver 10.10.0.2` (+2 entry is always AmazonProvidedDNS "Route 53 Resolver")
      - Resolves both public and private Route53 zones and otherwise forwards to Public DNS
      - Nameserver can also be set to a custom Active Directory or DNS server (e.g. `yum install bind bind-utils`) within the VPC with a SG allowing UDP 53.
        - `/var/named/corp.internal.zone`, `/etc/named.conf`
  - VPC's DHCP Options Set also contains
    - NetBIOS node type
    - `enableDnsSupport` (default `true`)
      - Allows bootstrapping via 169.254.169.253
    - `enableDnsHostName` (default `true` for default VPC; default `false` for non-default VPC)
      - If both `enableDnsSupport` and `enableDnsHostName` are true and instance has a public IP, then hostname based on public IP.
      - Private hosted zones in Route53 requires both `enableDnsSupport` and `enableDnsHostName`
    - NTP servers IPs
  - DHCP Options Sets are immutable but new ones can be created and attached to the VPC
    - Changes require DHCP lease to expire -- but `sudo dhclient -r eth0` can force (or just reboot EC2 instances)
    - If no DHCP Option Sets are associated with a VPC, then no DNS resolution
    - `domain-name` can point to "private hosted" Route53 domain, but it is prepended to `/etc/resolv.conf`'s `search` configuration (e.g., `search corp.internal ap-south-1.compute.internal`)

#### Hybrid DNS scenarios

- DNS server on-prem; DNS server in AWS; Bidirectional
  - Inbound resolver endpoint: On-prem forwards DNS requests to R53 resolver
  - Outbound resolver endpoint: Conditional forwarders for AWS to On-prem

### VPC Network Performance and Optimization

- Terms:
  - Flows
    - Typically limited to 5Gbps, but 10Gbps within Cluster Placement Group
  - Bandwidth (Gigabit/s)
    - EC2
    - Internet Gateway, other regions, Direct Connect -- 5Gbps (unless current gen EC2 with at least 32 vCPUs, in which case 50% of network bandwidth)
    - NAT Gateway -- 45Gbps/gateway
    - VPC Peering
    - Site-to-site aggregate bandwidth 1.25Gbps
    - Transit Gateway limited to 50Gbps aggregate
  - Throughput (Gigabit/s)
  - Latency
    - Place EC2 instances in a placement group
    - EBS Optimized Instances have dedicated bandwidth to EBS (rather than sharing)
    - Enhanced Networking -- Over 1M PPS using SR-IOV with PCI passthrough (avoiding hypervisor)
      - Requires AMI and Instance Type support
      - Intel "ixgbevf" driver (Intel 82559 VF up to 10Gps)
        - `ethtool -i ${ETH}`
      - Elastic Network Adapter (up to 100Gbps aggregate, 5-10Gbps/flow)
        - Elastic Fabric Adapter (only Linux)
      - ENI configuration no longer matters
  - Jitter
  - Packets/s (PPS)
    - MTU (typically 1500, but Jumbo of up to 9001 bytes are possible and enabled by default in VPC)
      - View with `ip link show $ETH`; change with `sudo ip link set dev $ETH mtu 9001`
        - `tracepath $IP`
      - Not all EC2 types support Jumbo frames
      - MTU per ENI
      - Jump frames within EC2 cluster placement groups for highest bandwidth
      - AWS Direct Connect supports jumbo frames
      - Internet Gateway 1500 bytes; VPC Endpoints 8500 bytes; intra-region VPC peering: 9001 bytes; inter-region VPC peering 1500; DirectConnect over TransGateway: 8500 bytes
    - MTU Path discovery of course requires ICMP
- IPv4 packet
  - 0-3 Version (e.g. "4"))
  - 4-7 IHL (Internet Header Length -- number of 32-bit words, minimum is 5 for 20 bytes, max is 15 for 60 bytes)
  - 8-13 DSCP (QoS-like field)
  - 14-15 ECN (congestion signalling)
  - 16-31 Total Length (header and data)
  - 32-47 Identification (for reassembling fragments, etc)
  - 48-48 Reserved flag
  - 49-49 Don't Fragment (DF)
  - 50-50 More Fragments (50)
  - 51-63 Fragment offset -- units of 8 bytes.
  - 64-71 TTL
  - 72-79 Protocol (0x08 is TCP, 0x01 is ICMP, 0x11 is UDP, 0x29 is IPv6 encapsulation, etc)
  - 80-95 Header checksum
  - 96-127 Source IP
  - 128-159 Destination IP
  - Perhaps Options (rarely used)
- Intel Data Place Development Kit (DPDK) for faster packet processing
- Network I/O Credits
  - For r4, c5 instance families, etc.
  - Makes benchmarking more difficult

### VPC Monitoring

- VPC Flow Logs -- IP traffic going in/out ENI
  - VPC Flow Logs, Subnet Flow Logs, ENI Flow Logs (including ELB, RDS, ElastiCache, Redshift, WorkSpaces, etc)
  - Store
    - S3 (and eventually query with Athena)
    - CloudWatch (and query with CloudWatch Log Insights
      - `fields @timestamp, @message | sort @timestamp desc | limit 20`
      - `stats sum(packet) as packetSum by srcAddr, dstAddr | sort packetSum desc | limit 15`
      - `filter action="ACCEPT" | stats sum(packet) as packetSum by srcAddr, dstAddr | sort packetSum desc | limit 15`
  - Hints
    - If return packets are rejected, then it's NACL rather than SG
    - Wireshark, tcpdump, traceroute, telnet, nslookup, ping (if SG/NACL permit it) are available
  - Excluded traffic
    - To/From native DNS services
    - 169.254 traffic
    - DHCP
    - Windows license activation server
- Format
  - Version, account-id, interface-id, srcaddr, stdaddr, srcport, dstport, protocol, packets, bytes, start, end, action, log-status
- Configuration
  - Accept/Reject/all
  - Maximum aggregation interval (1m or 10m)
  - Destination (S3 or CloudWatch Logs)
    - Destination Log group and IAM role (and grouped by ENI instance)
  - Format (AWS Default or custom)

### VPC Traffic Mirroring

- All traffic (or filtered subset) from an ENI can be sent to out-of-band monitoring (content inspection, threat monitoring, troubleshooting)
  - Filters: Inbound/Outbound, Accept/Reject, L4 Protocol, Source/Destination Port Range, Source/Destination CIDR
  - Target can be another ENI or a Network Load Balancer (UDP 4789)
    - Target can be a peered cis-region VPC or transit gateway (even belonging to another AWS account)

### Transit Gateway (TGW)

- Connections from VPCs, VPN Connection, AWS Direct Connect
  - May be peered with Transit Gateways in other regions
- Supports AWS Network Firewall (both North/South and East/West)
- Attachments
  - VPCs (disjoint CIDRs)
    - TGW routing table automatically gets route to attached VPC, but each attached VPC needs a manual route to the TGW
  - VPN
    - Each VPN limited to 1.25Gbps, but ECMP (equal-cost multi-part) can give you 50Gbps
- Supports VRF (Virtual Routing and Forwarding)

## S3 (Simple Storage Service)

- Infinitely-scaling storage and backbone
- Typical uses
  - Backup, storage, DR, archive, hybrid cloud storage, app hosting, media, data lakes, big data, software delivery, static websites
- Objects in globally-uniquely-named buckets (created in a specific region)
  - Bucket naming: no uppercase, no underscore, 3-63 chars, not an IP.
  - Key is full path
  - Objects <5000GB (but mandatory multi-part upload for >5GiB and suggested multi-party upload for >128MiB) with metadata key/value pairs, up to ten unicode key/value tags, and perhaps version ID.
  - Objects have an ETag (typically just MD5 for smaller files) based solely on their contents (and perhaps KMS, etc)
    - Very useful when passed via `If-None-Match` HTTP header (`304` -- "Not Modified")
    - `If-None-Match: *` useful for PUT to block overwriting

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
      - Condition, e.g. `"Null"`, `"StringEquals"`, `"ArnLike"`, `"ForAllValues:StringNotEquals"` with parameters, `"s3:x-amz-server-side-encryption-aws-kms-key-id": "true"`, `"s3:x-amz-acl": ["public-read"]`, `"aws:SourceArn": "arn:aws:s3:::EXAMPLE-SOURCE-BUCKET"`, or `"aws:PrincipalServiceNamesList": "logging.s3.amazonaws.com"`
    - Can mandate encryption
    - Can be prevented from granting access to public, even for the entire account
  - Object ACL (only when specific objects need special protection)
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
  - May apply to all objects in bucket, or whose which pass a filter
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

- Compatible with (and probably a good idea for) multi-part upload
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
  - Use https, duh
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

### S3 CORS (browser-based) but not CSRF (server-based)

- "Origin" -- protocol + domain + port
- Browser blocks traffic to second site unless second site has a matching Access-Control-Allow-Origin response to a preflight request
- Remember: S3's API is mostly just GET
- CORS Policy -- JSON or XML document with Allowed{Headers,Methods,Origins} and MaxAgeSeconds
  - Can also ExposeHeaders

### S3 Access Points

- Permissioning for different prefixes
- Each access point has a different DNS FQDN (public internet or VPC)
- VPC
  - VPC Endpoint needs to have `s3:GetObject` rights to both the bucket and the access point

### S3 Object Lambda Access Point

- Useful for redaction, enriching, and/or watermarking
- Transforms GET/LIST data
- Requires a supporting access point

## EC2 Instance Metadata Service -- IMDS

- Link-local (`169.254.0.0/16`)
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
- Origins (supports multiple simultaneously)
  - S3
    - CloudFront Origin Access Control (OAC)
      - (Replaces Origin Access Identity (OAI))
    - Also for uploading
  - HTTP
    - ALB (Application Load Balancer)
    - EC2 instance
    - S3 static website
  - "Behaviors" define which endpoints go to which origins
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

### Global Accelerator

- Two global static public IPs
- Traffic automatically routed to nearest healthy endpoint

### CloudFront Caching

- `CreateInvalidation` API (all files or a path)
- By default, query strings, http headers, and cookies are not part of the Cache Key
  - But if they are part of the Cache Key then they are passed to the origin
    - Or Origin Request Policy can define headers to pass to the origin even though it's not part of the Cache Key
- CloudFront can only access public ALB or EC2 instances (not VPC)
- CloudFront can exclude by source country, etc.

### CloudFront Signed URLs / Signed Cookies

- Trusted signers (AWS accounts)
- Signed cookies may permission an entire site. Signed URLs have higher priority.
- Two types of signers
  - CloudFront key group (recommended) -- APIs to create and rotate keys
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
- `/etc/ecs/ecs.config` should be populated with `ECS_CLUSTER` by a userdata script
  - Might also want to populate `ECS_ENGINE_AUTH_TYPE=docker`, `ECS_LOGLEVEL=debug`, etc.
- | Dimension    | ECS Container Instance | Fargate          |
  | ------------ | ---------------------- | ---------------- |
  | Host OS      | Linux, Windows         | Linux            |
  | Max vCPU     | 448                    | 4                |
  | Max RAM      | 26 TB                  | 30 GB            |
  | CPU Bursting | Linux                  | No               |
  | Pricing      | Per EC2 instance       | Per Task         |
  | EFS          | Yes                    | Yes              |
  | EBS          | Hard                   | No               |
  | Networking   | Lots of options        | One ENI per task |

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
    - AWSFireLens (for ECS and Fargate)
      - Firehose/KinesisStream/OpenSearch/S3
      - Streams logs to CloudWatch, Kinesis Data Firehose, and Fluentd/Fluent Bit
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

- Cloud-hosted IDE
- Requires `AWSServiceRoleForAWSCloud9`

### Amazon Elastic Kubernetes Service (EKS)

- Also supports EC2 or Fargate
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
      - Web server environment tier (servicing HTTP)
      - Worker environment tier (pulls from SQS)
    - Instances -- Dev, Test, Prod, etc.
    - Modes -- Single-instance (for dev), HA with Load Balancer
      - Also, whether to use spot instances
    - ```mermaid
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
- Deployment strategies
  - All-at-once
  - Rolling
  - Rolling with additional batches
  - Immutable (new ASG, then swap)
    - Q: Why does the slide call this the "Longest deployment"???
  - Blue/Green (entirely new environment, canaried and transitioned using Route53)
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
- Stacks may be nested (just a resource of Type `AWS::CloudFormation::Stack`)
  - Useful for repeated patterns
  - Useful for common components
  - Never directly update a sub-stack

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
  - But doesn't know if changes will succeed
- DeletionPolicy
  - `Retain`
  - `Snapshot`
    - EBS Volume (`AWS::EC2::Volume`), ElastiCache Cluster, ElastiCache ReplicationGroup, RDS DBInstance, RDS DBCluster, Redshift Cluster
  - `Delete`
    - Default (except for `AWS::RDS::DBCluster`)
- TerminationProtection
  - Requires `cloudformation:UpdateTerminationProtection`
- UserData
  - Encoded through `Fn::Base64`, but limited in size
    - ```sh
      #!/bin/bash -xe
      yum install -y aws-cfn-bootstrap
      /opt/aws/bin/cfn-init -v --stack ${AWS::StackName} --region ${AWS::Region} --resource $RESOURCE 
      /opt/aws/bin/cfn-signal  --stack ${AWS::StackName} --region ${AWS::Region} --resource $RESOURCE -e $? 
      ```
  - Output log in `/var/log/cloud-init-output.log`
  - Alternative (python scripts)
    - `cfn-init`
      - Retrieves metadata and initialization config from CloudFormation
      - Logs to `/var/log/cfn-init.log`
    - `cfn-signal`
      - `WaitCondition` blocks until signal received
      - `CreationPolicy` (EC2, ASG)
        - Specify `Timeout` (e.g. `PT5M`) and `Count` (e.g. `1`)
    - `cfn-get-metadata`
    - `cfn-hup` -- daemon checking for metadata updates, and executes hooks accordingly
  - `AWS::CloudFormation::Init::{config,configSet}`
    - `packages`
    - `groups`, `users`
    - `sources`, `files`
    - `commands`
    - `services`
- Nested stacks (best practice)
  - Component reuse
  - Inner stack is essentially private
- Cross stacks
  - When resources have different life-cycles
  - `Fn:ImportValue`
  - E.g. passing export values to many stacks (e.g. VPC Id)
- StackSets
  - Deploy across multiple accounts and/or regions
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
- Messages < 256KiB (of text), plus attributes
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
  - Visibility timeout (default 30s; 0-12h)
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
  - DMS (Database Migration Service, new Replica)
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
      - (based on schema in AWS Glue -- Fully-managed ETL)
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

### CloudWatch

- Metrics
  - Builtin metrics: CPUUtilization, NetworkIn, ....
    - In the `AWS` namespace
    - Dimensions (up to 30): instance ID, environment, etc
    - Timestamps
      - EC2 default to "every 5m", but can pay for "every 1m" (e.g. to scale ASG faster)
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
      - KMS optional (and specified at the Log Group Level)
        - `associate-kms-key`, `create-log-group`
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

### CloudWatch Alarms

- Alarms (trigger notification)
  - States: OK, INSUFFICIENT_DATA, ALARM
  - Period (length of time to evaluate metric): 10s, 30s, $N$ minutes
  - Actions
    - EC2 (e.g. restarting instance)
    - EC2 Auto Scaling
    - Amazon SNS
    - Composite Alarms! (AND, OR)
  - `aws cloudwatch set-alarm-state` useful for testing

### CloudWatch Synthetics Canary

- Manually reproduce critical user journeys -- checks success, availability, latency
- Scripts in Node.js or Python
- Programmatic access to headless Google Chrome instance. (Q: Chromium???)
- May run periodically
- Blueprints
  - Heartbeat Monitor
  - API Canary
  - Broken Link Checker
  - Visual Monitoring vs baseline
  - Canary Recorder (Q: Node.js or Python???)
    - GUI Workflow Builder

### EventBridge (formerly CloudWatch Events)

- EventBridge
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
      - Compute: Lambda, AWS Batch, Launch ECS (Elastic Container Service) Task
      - Integration: SQS, SNS, Kinesis Data Stream
      - Maintenance: SSM, EC2 Actions
      - Orchestration: Step Functions, CodePipeline, CodeBuild
      - Optionally archive (by default indefinitely), which allows replaying
    - Buses!
      - AWS services publish to the default event bus
      - AWS SaaS Partners (e.g. zendesk, datadog, salesforce) publish to a partner event bus

### X-Ray

- X-Ray (Distributed traces)
  - Compatibility: Lambda, Elastic Beanstalk, ECS, ELB, API Gateway, EC2, even on-prem
  - Traces, segments, subsegments, annotations (indexed) and metadata (not indexed)
    - Every request, or %, or rate
    - Default: 1st request each _second_ (reservoir) and 5% of rest (rate)
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
    - Writing -- `PutTraceSegments`, `PutTelemetryReco/0x08rds`, `GetSamplingRules`, `GetSamplingTargets`, `GetSamplingStatisticSummaries`
    - Reading -- `GetSampling{Rules,Targets,StatisticSummaries}`, `Get{Group,Groups,ServiceGraph,TimeSeriesServiceStatistics}`, `GetTrace{Graph,Summaries}`, `BatchGetTraces`

### OpenTelemetry

- AWS Distro for OpenTelemetry
  - Auto-instrumentation agents!
  - Send to X-Ray, CloudWatch, Amazon Managed Service for Prometheus, or Partner Monitoring Solutions
  - Instruments apps on AWS or on-premises

### CloudTrail

- CloudTrail
  - Governance, compliance, audit ... enabled by default
  - API call monitoring
    - Management events (default logged -- both read events and write events)
    - Data events (not logged by default)
    - CloudTrail Insights (optional service which compares against baseline) events
  - AWS Resource audit trails
  - Retained for 90d; Can be sent into S3 (for Athena to inspect later) or CloudWatch Logs

## Lambda

- Initially, FaaS (Functions as a Service)
  - S3 for static content
  - API Gateway to Lambda to DynamoDB for REST API
  - Cognito for Identity
  - SNS, SQS, Kinesis Data Firehose
  - Aurora Serverless
  - Step Function
  - Fargate
- For short (or at least shortish) executions (under 15m)
  - Can set timeout to any number of seconds
  - Q: So sync is far cheaper than async?
- Runs on-demand, scaling is automated
  - Pay per request and compute
    - Free tier is 1,000,000 Requests and 400,000 GB-seconds of compute
  - Can get up to 10GB RAM or as little as 128MB
    - (CPU and network scales with RAM)
  - CloudWatch monitoring
- Many languages
  - Node.js, Python, Java, C# (.NET core), golang, C# / Powershell, Ruby
  - Custom Runtime API -- Rust, etc.
  - Lambda Container Image implementing Lambda Runtime API
    - But use ECS/Fargate for arbitrary Docker images
- Integrations
  - API Gateway
  - Kinesis
  - DynamoDB (triggers written in Lambda!)
  - S3
  - CloudFront
  - CloudWatch Events EventBridge, CloudWatch Logs, SNS, SQS, Cognito
- Creation
  - Author from scratch
  - Use a blueprint
  - Container image
  - Browse repo
- Concurrency
  - Reserved (for that function as opposed to others) -- Free
  - Provisioned (always-available) -- Decidedly not free

### Lambda -- Synchronous

- User-invoked
  - CLI (`aws lambda list-functions`)
    - `aws lambda invoke --function-name hello-world --cli-binary-format raw-in-base64-out --payload '{"key1:"value1"}' --region $REGION response.json`
  - ALB (Application Load Balancer)
    - Lambda function registered in the target group (via the "EC2 > Target groups" console, even though it's not EC2)
    - Request: `requestContext`, `headers` and `queryStringParameters` sub-documents; `httpMethod`, `path`, `body` (perhaps base64) fields.
      - ALB "Multi-Header Values" setting uses arrays for repeated query string fields, e.g. `"queryStringParameters":{"name":["foo","bar"]}`
      - Q: Why is the lecture talking about this field for "responses" ... it's clearly about "requests"!
    - Response: `headers` sub-document (especially `"Content-Type"`), `statusCode` and `body` fields
      - Q: Why also a `statusDescription` field?
  - SDK
  - API Gateway
  - CloudFront (Lambda@Edge)
- Service-invoked
  - Cognito, ASW Step FunctionsA
- Other
  - Lex, Alexa, Kinesis Data Firehose

### Lambda -- Asynchronous

- Returns `202 Accepted` HTTP response
- Sources
  - S3 events
    - In addition to SNS topics and SQS queues (which can itself call Lambda), S3 events can directly trigger an async Lambda (even with an SQS DLQ)
    - Enable bucket versioning to get _every_ update on an object
    - Typical case: write S3 metadata to DynamoDB or even RDS
    - All intra-region, of course
  - SNS
  - CloudWatch Events
  - CloudWatch EventBridge
  - CodeCommit
  - CodePipeline
  - CloudWatch Log,
  - Simple Email Service
  - CloudFormation
  - AWS Config
  - AWS IoT
  - AWS IoT Events
- Data
  - Event
    - `id`, `source`, `account`, `time`, `region`, `resources`, `detail` (EventCategories, SourceType, SourceArn, Message, etc), etc
  - Context
    - `aws_request_id`, `function_name`, `invoked_function_arn`, `function_version` (`$LATEST`), `memory_limit_in_mb`, `log_group_name`, `client_context`, etc
- Retry strategy: max 3 tries total -- T+1m, T+3m
  - Lambda function should be idempotent
  - Should define and SNS or SQS DLQ (and of course this requires IAM perms)
    - But is there any replay ability?
- EventBridge had "Schedule" rules, but there's also a new "EventBridge Scheduler" service

### Lambda -- Event Source Mapping (synch)

- Polling
  - Streams (Kinesis Data Streams and DynamoDB Streams)
    - One iterator for each shard, and parallel up to ten batches per shard (respecting partition keys)
    - Start from: (1) the beginning, (2) from a timestamp, or (3) only new items.
    - Can delay processing until "batch window" is full
    - NB: By default, if single item fails, entire batch is reprocessed until success or expiration. (Necessary for in-order processing)
      - Can discard old events to a "Destination"
      - Restrict retries
      - Split-batch-on-error (useful for timeout issues)
  - Queues (SQS and SQS FIFO)
    - Long Polling (1-10 messages in batch for FIFO, 1-10000 for standard queues -- but batch window must be at least 1 sec if size is over 10)
    - Recommendation: queue visibility timeout should be 6x the Lambda timeout
    - DLQ is created on the SQS queue, not the Lambda instance (which is only for async)
      - Or invoke a 2nd lambda for failures
    - For standard queues, retries will likely be in totally different batches than before
      - Single element might be processed twice, even if there's no error
    - Scaling
      - SQS -- Lambda adds up to 60 more instances per minute (up to 1000 simultaneous batches) for scaling
      - SQS Fifo -- Lambda scales to the number of active message groups
    - `AWSLambdaSQSQueueExecutionRole` (aws-managed)
- Lambda invoked synchronously

### Lambda Destinations

- For Asynchronous or Batch invocation
- Define up to two destinations -- for success and/or failure
- SQS, SNS, EventBridge, and even another Lambda!
- (More flexible than classic DLQ)

### Lambda Security

- Every lambda function has a role, which is granted policies (or roles)
- Managed roles for event source mapping
  - `AWSLambdaBasicExecutionRole` -- upload logs to CloudWatch
  - `AWSLambdaKinesisExecutionRole` -- read from Kinesis
  - `AWSLambdaDynamoDBExecutionRole` -- read from DynamoDB Streams
  - `AWSLambdaSQSQueueExecutionRole` -- read from SQS
- Other managed roles
  - `AWSLambdaVPCAccessExecutionRole` -- Deploy in VPC
  - `AWSXRayDaemonWriteAccess` -- Send traces
- Resource-based policies
  - Granting other accounts/services access to Lambda
  - (Similar to S3 bucket policies)

### Lambda Environment Variables

- String key/value pairs available to code.
- May include KMS-encrypted secrets
- Language-specific
  - Python: `import os` and `os.getenv("ENV_VAR")`

### Lambda Logging/Monitoring

- CloudWatch Logs -- enabled by default
- CloudWatch Metrics -- Invocations, durations, concurrent executions, error count, success rates, throttling, async delivery failures, iterator age (streams)
- X-Ray tracing
  - Enable "Active Tracing"
  - Grant AWSXRayDaemonWriteAccess
  - Use X-Ray SDK
  - Environment variables
    - `_X_AMZN_TRACE_ID` (tracing header)
    - `AWS_XRAY_CONTEXT_MISSING` (default `LOG_ERROR`)
    - `AWS_XRAY_DAEMON_ADDRESS` (IP:PORT)

### CloudFront Functions and Lambda@Edge

- Serverless and deployed globally; pay for what you use
  - Client-->CloudFront ("Viewer Request")
  - CloudFront-->Origin ("Origin Request")
  - Origin-->CloudFront ("Origin Response")
  - CloudFront-->Client ("Viewer Response")
- CloudFront Functions
  - Native CloudFront feature
  - Only JavaScript
    - Less than 2MB of memory; package size < 10KB
    - No network access, filesystem access, message body access.
  - High-scale (millions/s), latency-sensitive (sub-millisecond) CDN content customizations. 1/6th price of @Edge
  - Modified _either_ viewer request _or_ viewer response
- Lambda@Edge
  - NodeJS or Python
  - Scales to (thousands/s); latency up to 5s-10s
  - Can modify both viewer and origin, both requests and responses
  - Authored in one region (typically us-east-1) and then CloudFront replicates to other regions
- Typical use-cases
  - CloudFront
    - Cache key normalization
    - Header manipulation
    - URL rewrites/redirects
    - Create/validate user-generated tokens (e.g. JWT -- JSON Web Tokens)
  - Lambda@Edge
    - > 1ms allowed
    - Access to HTTP message body allowed
    - CPU, Memory, AWS SDK, File Systems,
  - Website security/privacy
  - Dynamic Webapps at the Edge
  - SEO
  - Intelligent routing
  - Bot mitigation
  - A/B Testing
  - Authentication/authorization
  - User prioritization
  - User tracking/analytics

### Lambda networking

- By default Lambdas run in a separate VPC
- If you tell the Lambda: VPC ID, Subnets, Security Groups, then Lambda will create an ENI (elastic network interface) in your subnets
  - Requires `AWSLambdaVPCAccessExecutionRole` or equivalent
  - Internal resource (e.g. RDS) needs to allow access from the Lambda security group
  - Access to the public internet requires a NAT Gateway or Instance
  - Access to DynamoDB requires NAT or VPC endpoint.
  - Probably also need `AWSLambdaENIManagementAccess`
  - CloudWatch Logs doesn't require anything, fortunately

### Lambda Function Configuration

- vCPU not directly available...instead RAM from 128MB to 10GB.
  - 1792MB (1.750 GiB) <==> 1 vCPU
    - Any more requires threading
- Timeout: 3s (1s-900s)
- Execution context is saved for some time to speed the next call
  - So do your `db.connect(os.getenv("DB_URL"))` outside of the handler itself.
- `/tmp` (10GB) available for checkpointing -- but use S3 for permanent persistence
  - Use KMS Data Keys if you need.

### Lambda Layers

- Custom runtimes
  - C++, Rust, etc.
- Externalize static dependencies for reuse
- AWS-provided
  - Perl5
  - Python 3.8 + SciPy 1.x
  - AppConfig-Extension
  - LambdaInsightsExtension
  - AWS CodeGuru Profiler
- Up to five layers and a total of 250MB of immutable storage (but store in S3 if >50MB)
  - No additional charge
  - Node.js -- `npm` and `node_modules/`
    - CloudShell already contains `npm`
    - `npm install aws-xray-sdk; chmod a+r *; zip -r function.zip .; aws lambda create-function --zip-file fileb://function.zip --function-name lambda-xray-with-deps --runtime nodejs14.x --handler index.handler --role arn:aws:iam::$ACCT:role/DemoLambdaWithDependencies`
      - (Yes, `fileb://` is for binary local files!)
      - `const AWSXRay = require("aws-xray-sdk-core")`
      - `const AWS = AWSXRay.captureAWS(require("aws-sdk"))`
      - `const s3 = new AWS.S3()`
      - `exports.handler = async function(e) {return s3.listBuckets().promise()}`
  - Python -- `pip --target`
  - Java -- include `.jar` files
  - Native libraries -- compile under Amazon Linux
  - AWS SDK already included
- Lambda Container Images allow images of up to 10GB from ECR
  - Base image must implement Lambda Runtime API
    - Python, Node.js, Java, .NET, Golang, Ruby
  - Testable using Lambda Runtime Interface Emulator
  - Sample Dockerfile: `FROM amazon/aws-lambda-nodejs:12; COPY app.js package*.json ./; RUN npm install; CMD ["app.lambdaHandler"]`
    - AWS-provided base images are pre-cached by Lambda service
    - Use Multi-Stage builds (i.e. discard intermediate artifacts)

### Lambda EFS

- Requires EFS Access Points
- Beware EFS connection limits during bursts of activity

### Lambda Concurrency

- By default up to 1000 concurrent executions across _all_ functions
  - Open a support ticket to get above 1000
- Synchronous
  - Return 429 "Too Many Requests"
- Asynchronous
  - Retry and then DLQ

### Lambda Cold Starts and Provisioned Concurrency

- Lots of dependencies, the initial setup can take a few seconds.
- Application Auto Scaling can have "Provisioned Concurrency"
  - Scheduled or Target Utilization
- Can reserve concurrency for a specific function -- <https://docs.aws.amazon.com/lambda/latest/dg/configuration-concurrency.html>
  - A "zero" reserved concurrency disables the function.

### Lambda and Cloudformation

- inline (without dependencies) `Resources:$FunctionName:Code:ZipFile: |`
- Via S3 `Resources:$Function:Properties:Code:{S3Bucket,S3Key,S3ObjectVersion}`
  - Version the Bucket or be confused!
  - Q: Why aren't versioned buckets mandatory here?
  - Allows deployment to multiple accounts.

### Lambda Version

- Published versions are immutable
- Aliases (e.g. "dev", "test", "prod") are mutable
  - Canary deployment via _weighting_
  - Aliases cannot reference other aliases (except `$LATEST`)
  - Aliases can point to up to two versions at a time

### Lambda and CodeDeploy

- Can automate traffic shift for Lambda aliases.
- Integrated with SAM (Serverless Application Model) framework
- Strategies
  - Linear10PercentEvery3Minutes
  - Linear10PercentEvery10Minutes
  - Canary10Percent5Minutes (10% then 100%)
  - Canary10Percent30Minutes (10% then 100%)
  - AllAtOnce
- AppSpec.yml : name, alias, currentVersion, targetVersion

### Lambda -- Function URL

- No need for API Gateway
- https://$URLID.lambda-url.$REGION.on.aws (IPv4 and IPv6)
  - Only accessible from public URL
  - Supports CORS and Resource-based policies
  - Can only export function aliases or `$LATEST`
- AuthType
  - NONE -- Allow unauthenticated access (but Resource-based policy must grant public access)
  - AWS_IAM -- Both IAM and Resource-based policies are checked
  - Intra-account -- Either policy must allow
  - Inter-account -- Both policies must allow

### Lambda -- CodeGuru Profiling

- Supports Java and Python
- Activate from Lambda Console; creates Profiler Group

### Lambda -- Per-Region limits

- Execution
  - Memory: 128MiB-10GiB (1MiB increments)
  - Execution Time: 900s
  - Env variables: 4k
  - /tmp: 512MB - 10GB
  - Concurrency: 1000 (or larger)
- Deployment
  - Compressed: 50MB, uncompressed: 250MB

### Lambda Best Practices

- Lambda functions should never call themselves!!!

## DynamoDB

- Reminder: RDS only scales horizontally using sharding or RDS Read Replicas
- Regional managed NoSQL database with replication across AZ
  - No need to "create a database" ... just create tables.
    - Tables internally partitioned. (One partition per 3000 RCU and per 1000 WCU, or per 10GB of storage, whichever is larger.)
    - RCUs and WCUs then spread _evenly_ across partitions
  - Scales to trillions of rows, 100s of TB storage, millions of requests per second
    - String, Number, Binary, Boolean, Null
    - List and Map; String Set, Number Set, Binary Set
    - Records up to 400KB
  - Standard and Infrequent Access (IA) table classes
- Tables
  - Primary Key -- fixed (binary, number, or string)
    - Option: Well-distributed partition key (usually a hash of a high-cardinality field)
    - Option: Partition Key + Sort Key ("HASH + RANGE")
      - Really, just a special instance of composite key, but sort-key is sortable
  - Attributes -- can increase over time, and nullable
  - Explicit types `{"user_id": {"S": "54867546"}, "first_name": {"S": "John"}, "last_name": {"S": "Doe"}}`
    - `S`: String, `N`: Number: `B`: Base64 binary, `BOOL`: `true`/`false`, `L`: List, `M`: Map, `SS`: String Set, `NS`: Number Set, `BS`: Base64 Binary Set
- Optional
  - Secondary indices
    - Local Secondary Index (LSI)
      - Still within the same Partition Key as base table
      - Uses the WCUs and RCUs of the main table
      - One scalar attribute (String, Number, Binary)
      - Up to five per table
      - Defined at table creation time
      - Attribute projections: KEYS_ONLY, ALL, INCLUDE (specific)
    - Global Secondary Index
      - Alternative Primary key from the base table
      - Index must have its own provisioned RCUs and WCUs (typically same as underlying table)
        - If writes are throttled to a GSI, then writes to the main table are also throttled.
      - Can be added/modified after table creation
      - Attribute projections typically include the main table's partition key
      - Q: Must be unique???
  - Point-in-time recovery (PITR)
  - Per-record Time-to-Live (TTL)
    - Simply specify an attribute (perhaps `expire_on`) in Unix Epoch seconds.
    - No WCUs to expire, but take 48h after the Unix Epoch end-of-life time.
    - Once finally deleted, removed from LSIs and GSIs.
    - Deletion events do show up in DynamoDB streams.
  - DynamoDB stream
  - Replication Regions
- Modes
  - Eventually-consistent read (default) vs strongly-consistent read
    - Set `ConsistentRead` in API calls (`GetItem`, `BatchGetItem`, `Query`, `Scan`)
    - Higher latency and consumes twice the RCU
  - Transactional
- Capacity
  - Provisioned (default)
    - Can be free tier
    - Read (RCU) and write (WCU) capacity separately specified
      - Autoscaling possible, with min/max "capacity units" and a default-70% target utilization
      - (Or just a fixed number of "capacity units")
      - Burst Capacity, and then "ProvisionedThroughputExceededException" and exponential-backoff
      - WCU
        - One write/s up to 1KB
      - RCU
        - One Strongly-consistent read or two Eventually-consistent read per second, up to 4KB in size.
        - DynamoDB Accelerator (DAX) can cache reads, queries, and scans
          - No application logic changes -- API-compatible with DynamoDB
          - Multi-AZ (min 3 nodes recommended)
            - Port 8111 (plaintext over wire) or 9111 (encrypted in transit)
          - 5m Query TTL and 5m Item TTL
          - KMS, VPC, IAM, CloudTrail, etc.
          - Can store aggregation results in Amazon ElastiCache
  - On-demand (pay per read/write) -- no further configuration necessary
    - 2.5 more expensive than provisioned
    - Read/Write Request Units (RRU, WRU) same as RCU/WCU definitions
  - Can switch between Provisioned and On-demand once every 24 hours
- Encryption
  - DynamoDB-owned, or KMS, or manually-managed
- Operations
  - `PutItem` -- create or replace
  - `UpdateItem` -- modify only specified attributes
    - Also supports "Atomic Counters"
  - Conditional Writes/Update/Delete
    - For `PutItem`, `UpdateItem`, `DeleteItem`, and `BatchWriteItem`
    - Expressions
      - `attribute_exists`, `attribute_not_exists`, `attribute_type`, `IN`
        - e.g., `attribute_not_exists(partition_key)` or `attribute_not_exists(partition_key) AND attribute_not_exists(sort_key)` useful for preventing overwriting at all
      - string-specific: `contains`, `begins_with`, `size`
      - numeric: `between`
      - Composable with `AND`, `OR`, `NOT`, and `()`
      - Expression can contain `:variable`, populated by an `--expression-attribute-values file://values.json`
    - No performance hit; supports concurrency
  - `GetItem` -- based on primary key
    - Default: Eventually-Consistent Read
    - `ProjectionExpression` -- retrieve only some attributes
  - `Query`
    - `KeyConditionExpression`
      - Mandatory: Exact match on partition key value
      - Optional: Inequalities and "beginsWith" on the sort key value
    - `FilterExpression` (optional)
      - Non-key attributes
    - `Limit`
      - Max number of records to return
      - (Also limited to 1MB of data)
      - Pagination supported
    - May query: tables, Local Secondary Index, or Global Secondary Index
  - `Scan` -- read entire table and then filter (inefficient and uses lots of RCU)
    - Returns up to 1MB (and then pagination)
    - Supports Parallel Scan
    - Supports `ProjectionExpression` and `FilerExpression` (no change to RCU)
  - `DeleteItem`
    - Optionally conditional
  - `DeleteTable`
    - Far cheaper than one-by-one `DeleteItem`
  - (Batching available and performed in parallel, but partial success is possible)
    - `BatchWriteItem`
      - Up to 25 `PutItem` and/or `DeleteItem` -- but _not_ `UpdateItem`
      - Up to 16MB data in total; up to 400K per item
      - Returns `UnprocessedItems` list -- backoff or add WCU
    - `BatchGetItem`
      - Multiple tables!
      - Up to 100 items, total 16MB of data
      - Returns `UnprocessedKeys` -- backoff or add RCU
  - PartiQL -- SQL-compatible query language
    - SELECT, INSERT, UPDATE, DELETE -- but _no_ joins.
    - Use AWS Management Console, NoSQL Workbench for DynamoDB, DynamoDB APIs, AWS CLI, AWS SDK
- Paging with `--max_items` and `NextToken` to be passed as the next `--starting-token`

### DynamoDB Streams

- Ordered stream of CRUD operations on a table
  - Retained up to 24h
  - Sharded (like Kinesis Data Streams) but shards provisioned by AWS
- Content (not retroactive)
  - KEYS_ONLY
  - NEW_IMAGE
  - OLD_IMAGE
  - NEW_AND_OLD_IMAGES
- Reading
  - Sent to Kinesis Data Streams (and hence to Kinesis Data Firehose)
  - Read by AWS Lambda (synchronous)
    - Need to define an Event Source Mapping
    - Grant Lambda IAM permissions to read from underlying DynamoDB table
      - `AWSLambdaDynamoDBExecutionRole`
        - "dynamodb:DescribeStream", "dynamodb:GetRecords", "dynamodb:GetShardIterator", "dynamodb:ListStreams"
        - "logs:CreateLogGroup", "logs:CreateLogStream", "logs:PutLogEvents"
      - `AmazonDynamicDBReadOnlyAccess`
        - Lots more -- <https://docs.aws.amazon.com/aws-managed-policy/latest/reference/AmazonDynamoDBReadOnlyAccess.html>
    - Choose batch size (1-10000)
      - event.Records[].{eventID,eventName}
  - Read by Kinesis Client Library apps
- Uses
  - React in real-time
  - Analytics
  - Insert into derived tables
  - Insert into OpenSearch service
  - Implement cross-region replication

### DynamoDB Transactions

- ACID (atomicity, consistency, isolation, durability)
- Read Modes: Eventual, Strong, Transactional
- Write Modes: Standard, Transactional
- Consumes 2x WCU and 2x RCU (prepare and commit)
  - TransactGetItems
  - TransactWriteItems

### DynamoDB as a Session State Cache

- Common use-case
- Remember: DynamoDB is serverless whereas ElastiCache is in-memory
- Remember: EFS must be attached to EC2 instances
- Remember: Instance Store is only for local caching -- not shared caching
- Remember: S3 is higher-latency and meant for larger objects

### DynamoDB Write Sharding

- Add a suffix to the partition key value
  - Random or Calculated suffix
  - Q: How to read afterwards?

### DynamoDB Write Types

- Concurrent Writes
  - Last wins
  - Conditional writes gives you "first wins"
- Atomic Writes
  - "Increase by 1"
  - Q: I think the example just wraps the read and write into a transaction, as opposed to some `(v) => v+1` stuff.
- Batch Writes
  - Many puts/updates at a time.

### DynamoDB Large Object Patterns

- Store in S3 and store a URL thereof in DynamoDB thereof
- We can have Lambda triggers listening to S3 events and storing object metadata in DynamoDB. (Making up for S3 searching limitations.)

### DynamoDB Operations

- Table Cleanup
  - Option (slow) -- Scan and DeleteItem
  - Option (faster) -- Drop-and-recreate
- Table duplication
  - Option -- AWS Data Pipeline can use an EMR (Elastic MapReduce) Cluster to read from DynamoDB and write to S3. Then read from S3 and write to a second DynamoDB table.
  - Option -- Backup-and-restore (can take some time)
  - Option -- Write code and do Scan + BatchWriteItem

### DynamoDB Security

- Security
  - VPC Endpoints available.
  - Access controlled via IAM
  - Encryption at rest using KMS and in transit using https
- Backup and Restore
  - PointInTimeRestore (PITR)
  - No performance impact
- Global Tables
  - Can replicate to multiple regions using DynamoDB Streams
- DynamoDB Local
  - Local instance for testing purposes
- Database Migration Service (AWS DMS)
  - Helps migration to DynamoDB from MongoDB, Oracle, MySQL, S3, etc
- Direct user interaction (e.g., from a browser)
  - Get temporary AWS credentials from an identity provider
    - Amazon Cognito Identity Pools (authorization)
      - Amazon Cognito User Pools (identification)
    - Web Identity Federation
      - Google, Facebook, OpenID Connect, SAML
  - `"Condition":"ForAllValues:StringEquals":"dynamodb:LeadingKeys":["${cognito-identity.amazonaws.com:sub}"]`

## API Gateway

- IAM permissions required to directly call Lambda functions
- API Gateway
  - Exposes Lambda and internal HTTP endpoints (on premises, Application Load Balancer, etc)
    - Can also expose any AWS Service!
      - For example, allow users to publish messages to Kinesis Data Streams
  - Supports WebSocket
  - Supports API versioning
    - Versions are deployed to "stages", e.g. "dev"
    - Stage variables (similar to environment variables) -- `lambda-api-gateway-proxy-root-get:${stageVariables.lambdaAlias}`
      - For example, specify which Lambda alias to use.
      - HTTP endpoints
      - Parameter mapping templates
  - Supports multiple environments
  - Authentication and Authorization
    - Authentication
      - IAM Roles (for internal apps)
      - Cognito (e.g., mobile users)
      - Custom Authorizer (Lambda function)
    - HTTPS Termination (AWS Certificate Manager -- ACM)
      - Edge-Optimized endpoints must have certificate in "us-east-1"
      - Regional endpoints must have certificate in the API Gateway region
      - CNAME or alias record in Route 53
  - API Keys
  - Per-stage
    - Rate limiting; cache responses
    - Web Application Firewall (optional)
    - SSL certificate
    - CloudWatch Logs and/or Metrics
    - X-Ray tracing
    - Canary deployment (with different stage variables)
      - Blue/green deployment
  - Promotion
    - Deploy API to prod stage, or
    - Update a stage variable name from test to prod
  - Import API from Swagger / OpenAPI v3
    - TODO -- Read <https://en.wikipedia.org/wiki/OpenAPI_Specification>
  - Generate SDK and API specs -- Export as Swagger or OpenAPI 3 (optionally with Postman)
  - Transform and validate requests/responses
- Q: What is Lambda Proxy Integration?
- Q: Can one "un-promote" Canary somehow?

### API Gateway Endpoint Types

- Edge-Optimized (default)
  - API Gateway in one region, but requests routed through CloudFront Edge locations
- Regional
  - Clients in a single region.
  - May be manually combined with CloudFront
- Private
  - Only accessible from your VPC (definable using Resource Policy)

### API Gateway Types

- HTTP API
  - OIDC, OAuth2
  - CORS
  - Presents Lambda and HTTP backends
- Websocket API
  - Presents Lambda HTTP backends, AWS Services
- REST API (optionally private)
  - Finer-grained control
  - Presents Lambda, HTTP backends, AWS Services (from any region), Mocks, and VPC Links
  - Max timeout 29s
  - Note: the `AWS:SourceArn` in the Resource-based policy specify the endpoint verb

### API Integration Types

- Mock
- HTTP / AWS
  - Mapping templates for both request and response
  - Uses VTL (Velocity Template Language) -- <https://velocity.apache.org/engine/2.3/user-guide.html>, <https://en.wikipedia.org/wiki/Apache_Velocity>
    - Can rename/modify query string params
    - Modify body content, even convert JSON to XML for a SOAP backend.
    - Add headers, etc
    - Filter response
    - Sample response mapping
      - `#set($inputRoot=$input.path('$')) {"renamedField":$inputRoot.example}`
- AWS_PROXY (Lambda Proxy)
  - No mapping templates, etc -- Lambda function itself handles the raw headers, etc.
  - Request: `"resource"`, `"path"`, `"httpMethod"`, `"headers"`, `"multiValueHeaders"`, `"queryStringParameters"`, `"multiValueQueryStringParameters"`, `"pathParameters"`, `"stageVariables"`, `"requestContext"`, `"body"`, `"isBase64Encoded"`
  - Response: `"statusCode"`, `"headers"`, `"multiValueHeaders"`, `"body"`, `"isBase64Encoded"`
- HTTP_PROXY
  - No mapping templates -- request passed onto a backend.
  - Can add HTTP headers (e.g. API key)

### API Gateway -- Open API Spec

- Import
  - Method
    - Can perform basic validation before calling backend
    - Conformance to a specified JSON Schema -- TODO <https://json-schema.org/>
      - Q: Who pays for validation CPU?
      - `x-amazon-apigateway-request-validators`
  - Method Request
  - Integration Request
  - Method Response
  - AWS Extensions and options thereof
- Export as OpenAPI spec (e.g. to generate client code)
  - Android, Javascript, iOS (Objective-C), iOS (Swift), Java, Ruby
- YAML or JSON

### API Gateway Caching

- Expensive
- Per-stage
- Settings per-method
- TTL 300s (0s-3600s)
- Max cached response: 1MiB
- Capacity 0.5GB, 1.6GB, 6.1GB, 13.5GB, 28.4GB, 58.2GB, 118GB, and perhaps 237GB
- Invalidate cache entry with `"Cache-Control: max-age=0"` (with IAM auth, of course)
  - By default, _any_ client can invalidate the cache!

### API Gateway -- Usage Plans and API Keys

- Charging for our API!
- API Keys, throttling (and burst), quota (per day/week/month)
  - Q: Can a single API Key have different quotas for different endpoints?
  - Associate API stages and API keys with a usage plan
  - `X-API-Key` header
    - Can sell on AWS marketplace!

### API Gateway -- Logging/Tracing

- CloudWatch Logs
  - Likely to contain sensitive info
- CloudWatch Metrics (per stage)
  - `CacheHitCount`, `CacheMissCount`, `Count`, `IntegrationLatency` (i.e. backend latency), `Latency` (end-to-end, must be less than 29 seconds), `4XX` and `5XX`
    - 4XX include:
      - 400 -- Bad request
      - 403 -- Access Denied or WAF filtered
      - 429 -- Quota exceeded or throttled
    - 5XX include:
      - 502 -- Bad Gateway (perhaps Lambda returned unparseable data)
      - 503 -- Service Unavailable
      - 504 -- Integration Failure (including 29-second timeout)
- X-Ray

### API Gateway -- Throttling

- Default 10000 requests/s across all APIs
  - Raise a ticket to increase
- Can set Stage limits and Method limits

### API Gateway -- CORS

- OPTIONS
  - request contains: `Origin`
  - response contains: `Access-Control-Allow-Methods`, `Access-Control-Allow-Headers`, `Access-Control-Allow-Origin`
- Note: `LAMBDA_PROXY` and `HTTP_PROXY` modes need to _manually_ set the response fields

### API Gateway -- Security

- Authentication and Authorization via IAM -- best way from within AWS
  - Sig v4 in HTTP headers.
    ```Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20130524/us-east-1/s3/aws4_request, SignedHeaders=host;range;x-amz-date, Signature=fe5f80f77d5fa3beca038a248ff027d0445342fe2855ddc963176630326f1024`
  - Resource Policies (especially for cross-account access)
    - Can filters by source IP or VPC
- Authentication: Cognito User Pools (database of users); Authorization: In your code
- Authentication: External; Authorization: Lambda Authorizer (JWT or OAuth token, plus headers, query string, stage vars, etc)
  - Lambda returns an IAM Principal and IAM policy for the user (and API Gateway will cache)

### API Gateways

- HTTP API
  - Low-latency, cheap, no data mapping
  - OIDC and OAuth 2.0
  - CORS
  - No API Keys, usage plans, resource policies
- REST API
  - All features except OIDC/OAuth 2.0/JWT
- WebSocket API
  - `npm install -g wscat` is like `curl`
  - Two-way, so allows push
  - "onConnect" Lambda function (passed a `connectionId` useful for DynamoDB to store session info)
  - "sendMessage" Lambda function ("frames")
  - "onDisconnect" Lambda function
  - Works with AWS Services (Lambda, DynamoDB) or HTTP endpoints
  - `wss://$UNIQUE_ID/execute-api.$REGION.amazonaws.com/$STAGE_NAME`
  - API Gateway provides a callback connection UTL (`https://$UNIQUE_ID.execute-api.$REGION.amazonaws.com/$STAGE_NAME/@connections/$CONNECTION_ID`)
    - POST -- send messages to client
    - GET -- gets current status of the specified client
    - DELETE -- disconnect the connected client
  - Incoming request routing, e.g. `$request.body.action` ... if not matched, then `$default` route.
  - Sample: <https://github.com/aws-samples/websocket-chat-application>

### API Gateway Architecture

- API Gateway to:
  - S3 Bucket
  - ELB to ECS Cluster
  - ELB to EC2 Servers
- Route 53 can simplify this to our users, of course

## AWS CI/CD ("CICD")

- Continuous Integration (CodeBuild, Jenkins CI, etc)
- Continuous Delivery (CodeDeploy, Jenkins CD, Spinnaker, etc)
- Tools
  - CodeSuite
    - CodeCommit (or GitHub or Bitbucket)
    - CodeBuild (or Jenkins CI)
    - CodeDeploy (or Jenkins CD or Spinnaker, etc) (to EC2 instances, Lambda, ECS, or even on-prem)
    - CodePipeline (orchestrates all of the above)
  - CodeStar (management of the above, but EOL is 2024-07 -- to be replaced by CodeCatalyst)
  - CodeArtifact
  - CodeGuru (automated code reviews)

### CodeSuite

- CodeCommit, CodeBuild, CodeDeploy, CodePipeline

#### CodeCommit

- Source-code version control
  - Collaboration
  - Backup
  - Auditable
- CodeCommit -- private Git repos, Minimal UI
  - Alternatives: GitHub, GitLab, Bitbucket, etc
  - Authentication
    - SSH Keys (not available for root IAM user)
    - HTTPS
      - Git Credentials for IAM user (preferred) -- creates codecommit-specific username/password
      - AWS CLI Credential Helper
      - HTTPS(GRC) for `git-remote-codecommit`
  - Authorization -- IAM policies
    - Cross-account -- IAM Rol + AWS STS (`AssumeRole` API)
    - Can deny modification of `refs/heads/main`, etc.
      - `codecommit:{GitPush,DeleteBranch,PutFile,Merge{Branches,PullRequest}By{FastForward,Squash,ThreeWay}}`
    - Note: Resource-style policies not yet supported
  - Encryption -- KMS available
    - HTTPS or SSH is mandatory
  - Pull Requests
    - May define Pull Request Approval Rules
      - Pool of approvers (IAM users, federated users, IAM Roles, IAM Groups)
      - Number of approvals required
      - Approval rule templates, e.g. for dev vs prod
  - SNS (or AWS Chatbot for Slack) notifications
    - May be "full" or "basic"
      - "Basic" -- same info as sent to EventBridge or CloudWatch
    - `codecommit-repository-{comments-on-{commits,pull-requests},approvals-{status-changed,rule-override},pull-request-{created,source-updated,status-changed,merged},branches-and-tags-{created,updated,deleted}}`
    - Useful for cross-region replication
    - SNS topic
  - SNS (or Lambda) triggers (all events, push to branch, create branch/tag, delete branch/tag)
    - Triggers may be per-branch (up to 10 named branches)
    - Triggers may apply to all branches
    - May contain a custom data string

#### CodeBuild

- CodeBuild (building and testing)
  - Alternatives
    - Jenkins CI
    - CodeBuild Local Build (for deep troubleshooting) -- requires Docker and CodeBuild Agent
  - `buildspec.yml` (in project root by default, in CodeCommit, S3, BitBucket, GitHub)
    - `env`
      - `variables` (plaintext, e.g. `JAVA_HOME`)
      - `parameter-store` (SSM (systems manager) Parameter Store, including KMS-encrypted secrets)
      - `secrets-manager` (AWS Secrets Manager)
    - `phases` (each contains a `commands` array)
      - `install`, e.g. `apt-get update && apt-get install -y maven` Q: Why not in the image?
      - `pre-build`, e.g. `docker login`
      - `build`, e.g. `mvn install`
      - `post-build`, e.g. packing artifacts
    - `artifacts`
      - `files`, e.g. which artifacts to upload to S3 (encrypted with KMS)
    - `cache`
      - `paths`, e.g. `"/root/.m2/**/*"`
  - Environment variables
    | Env Variable | Meaning |
    | ------------ | ------- |
    | `AWS_DEFAULT_REGION` | The AWS Region where the build is running (for example, us-east-1). This environment variable is used primarily by the AWS CLI. |
    | `AWS_REGION` | The AWS Region where the build is running (for example, us-east-1). This environment variable is used primarily by the AWS SDKs. |
    | `CODEBUILD_BATCH_BUILD_IDENTIFIER` | The identifier of the build in a batch build. This is specified in the batch buildspec. For more information, see Batch build buildspec reference. |
    | `CODEBUILD_BUILD_ARN` | The Amazon Resource Name (ARN) of the build (for example, arn:aws:codebuild:region-ID:account-ID:build/codebuild-demo-project:b1e6661e-e4f2-4156-9ab9-82a19EXAMPLE). |
    | `CODEBUILD_BUILD_ID` | The CodeBuild ID of the build (for example, codebuild-demo-project:b1e6661e-e4f2-4156-9ab9-82a19EXAMPLE). |
    | `CODEBUILD_BUILD_IMAGE` | The CodeBuild build image identifier (for example, aws/codebuild/standard:2.0). |
    | `CODEBUILD_BUILD_NUMBER` | The current build number for the project. |
    | `CODEBUILD_BUILD_SUCCEEDING` | Whether the current build is succeeding. Set to 0 if the build is failing, or 1 if the build is succeeding. |
    | `CODEBUILD_INITIATOR` | The entity that started the build. If CodePipeline started the build, this is the pipeline's name (for example, codepipeline/my-demo-pipeline). If an user started the build, this is the user's name (for example, MyUserName). If the Jenkins plugin for CodeBuild started the build, this is the string CodeBuild-Jenkins-Plugin. |
    | `CODEBUILD_KMS_KEY_ID` | The identifier of the AWS KMS key that CodeBuild is using to encrypt the build output artifact (for example, arn:aws:kms:region-ID:account-ID:key/key-ID or alias/key-alias). |
    | `CODEBUILD_LOG_PATH` | The log stream name in CloudWatch Logs for the build. |
    | `CODEBUILD_PUBLIC_BUILD_URL` | The URL of the build results for this build on the public builds website. This variable is only set if the build project has public builds enabled. For more information, see Public build projects in AWS CodeBuild. |
    | `CODEBUILD_RESOLVED_SOURCE_VERSION` | The version identifier of a build's source code. The contents depends on the source code repository: CodeCommit, GitHub, GitHub Enterprise Server, and Bitbucket This variable contains the commit ID. CodePipeline This variable contains the source revision provided by CodePipeline. If CodePipeline is not able to resolve the source revision, such as when the source is an Amazon S3 bucket that does not have versioning enabled, this environment variable is not set. Amazon S3 This variable is not set. When applicable, the `CODEBUILD_RESOLVED_SOURCE_VERSION` variable is only available after the DOWNLOAD_SOURCE phase. |
    | `CODEBUILD_SOURCE_REPO_URL` | The URL to the input artifact or source code repository. For Amazon S3, this is `s3://` followed by the bucket name and path to the input artifact. For CodeCommit and GitHub, this is the repository's clone URL. If a build originates from CodePipeline, this environment variable may be empty. For secondary sources, the environment variable for the secondary source repository URL is `CODEBUILD_SOURCE_REPO_URL``<sourceIdentifier>`, where `<sourceIdentifier>` is the source identifier you create. |
    | `CODEBUILD_SOURCE_VERSION` | The value's format depends on the source repository. For Amazon S3, it is the version ID associated with the input artifact. For CodeCommit, it is the commit ID or branch name associated with the version of the source code to be built. For GitHub, GitHub Enterprise Server, and Bitbucket it is the commit ID, branch name, or tag name associated with the version of the source code to be built. Note For a GitHub or GitHub Enterprise Server build that is triggered by a webhook pull request event, it is pr/pull-request-number. For secondary sources, the environment variable for the secondary source version is `CODEBUILD_SOURCE_VERSION``<sourceIdentifier>`, where `<sourceIdentifier>`is the source identifier you create. For more information, see Multiple input sources and output artifacts sample. |
    |`CODEBUILD_SRC_DIR`| The directory path that CodeBuild uses for the build (for example, /tmp/src123456789/src). For secondary sources, the environment variable for the secondary source directory path is`CODEBUILD_SRC_DIR``<sourceIdentifier>`, where `<sourceIdentifier>` is the source identifier you create. For more information, see Multiple input sources and output artifacts sample. |
    | `CODEBUILD_START_TIME` | The start time of the build specified as a Unix timestamp in milliseconds. |
    | `CODEBUILD_WEBHOOK_ACTOR_ACCOUNT_ID` | The account ID of the user that triggered the webhook event. |
    | `CODEBUILD_WEBHOOK_BASE_REF` | The base reference name of the webhook event that triggers the current build. For a pull request, this is the branch reference. |
    | `CODEBUILD_WEBHOOK_EVENT` | The webhook event that triggers the current build. |
    | `CODEBUILD_WEBHOOK_MERGE_COMMIT` | The identifier of the merge commit used for the build. This variable is set when a Bitbucket pull request is merged with the squash strategy and the pull request branch is closed. In this case, the original pull request commit no longer exists, so this environment variable contains the identifier of the squashed merge commit. |
    | `CODEBUILD_WEBHOOK_PREV_COMMIT` | The ID of the most recent commit before the webhook push event that triggers the current build. |
    | `CODEBUILD_WEBHOOK_HEAD_REF` | The head reference name of the webhook event that triggers the current build. It can be a branch reference or a tag reference. |
    | `CODEBUILD_WEBHOOK_TRIGGER` | Shows the webhook event that triggered the build. This variable is available only for builds triggered by a webhook. The value is parsed from the payload sent to CodeBuild by GitHub, GitHub Enterprise Server, or Bitbucket. The value's format depends on what type of event triggered the build. For builds triggered by a pull request, it is pr/pull-request-number. For builds triggered by creating a new branch or pushing a commit to a branch, it is branch/branch-name. For builds triggered by a pushing a tag to a repository, it is tag/tag-name. |
    | `HOME` | This environment variable is always set to /root. |
  - Build projects may be defined in CodePipeline or CodeBuild
  - Output logs in S3 and/or CloudWatch Logs
  - CloudWatch Metrics for build statistics
    - CloudWatch Alarms for threshold-based alerts
  - EventBridge
    - `codebuild-project-build-{state-{failed,in-progress,succeeded},phase-{failure,success}}`
  - Build stats in CloudWatch Metrics
  - Supplied build containers (supplied Docker Image) -- Java, Ruby, Python, Go, Node.js, Android, .NET Core, PHP
    - Or provide your own Docker container.
    - Note: these containers can also be run locally if you need to understand why a build is failing
  - Can run integration tests
  - Optimization: can optionally store reusable artifacts in an S3 bucket
  - Resulting artifacts in an S3 bucket
  - By default, builds run outside your VPCs, but you can specify a VPC config (VPC ID, Subnet IDs, Security Group IDs) to access resources in your VPC (e.g. RDS, ElastiCache, EC2, ALB, etc)
    - Q: What happens if you want the build itself to be outside your VPC but integration testing within your VPC?
    - Q: How do I manually run integration tests on a dev branch?
  - Security
    - CodeBuild Service Role
      - Download code
      - Fetch from SSM and Secrets Manager
      - Upload to S3
      - Store logs in CloudWatch Logs
  - Build Badges (exposed via public URL, compatible with CodeCommit, GitHub, BitBucket)
    - Dynamically shows status of latest build, at the branch level
  - Triggers
    - EventBridge
    - EventBridge to Lambda
    - GitHub web hook
- CodeBuild Test Reports
  - Unit tests, configuration tests, functional tests
  - 3rd-party `file-format`s -- JUnit XML, NUnit XML, NUnit3 XML, Cucumber JSON, TestNG XML, Visual Studio TRX
  - Add `reports:` to `buildspec.yml`

### AWS CodeDeploy

- Defined by `appspec.yml`
- Gradually deploy new app versions to EC2, Lambda, ECS, on-prem
  - In-place and blue/green
    - `AllAtOnce`
    - `HalfAtATime`
    - `OneAtATime`
    - Custom
  - And automated rollbacks if deployment fails or CloudWatch Alarm fires
- EC2 (and on-premises)
  - Requires CodeDeploy Agent
    - Stops traffic from ALB before update and re-enabled afterwards
    - Hooks: (`Start`), `BeforeBlockTraffic`, (`BlockTraffic`), `AfterBlockTraffic`, `ApplicationStop`, (`DownloadBundle`), `BeforeInstall`, (`Install`), `AfterInstall`, `ApplicationStart`, `ValidateService`, `BeforeAllowTraffic`, (`AllowTraffic`), `AfterAllowTraffic`, (`End`)
      - Specified in `appspec.yml`, with `location`, `timeout`, `runas`
        - `${DEPLOYMENT_GROUP_NAME}`
    - Blue/Green (requires load balancer, of course)
      - Deployment
        - Option: Manual deployment -- identify EC2 instances by tag
        - Option: Automatic deployment -- the "Blue" ASG is duplicated as a "Green" ASG
      - Termination
        - Option: Blue instances terminated after a wait (default 1h, max 2d)
        - Option: Blue instances deregistered from ELB and deployment group but kept alive
      - Hooks
        - `BeforeBlockTraffic` and `AfterBlockTraffic` run on Blue ... all other hooks run on Green
    - Configurations -- `CodeDeployDefault.AllAtOnce`, `CodeDeployDefault.HalfAtATime`, `CodeDeployDefault.OneAtATime`, custom
    - Triggers
      - Deployment and/or EC2 instance events to SNS
        - `DeploymentSuccess`, `DeploymentFailure`, `InstanceFailure`
  - EC2 instances must have perms to the S3 bucket containing deployment bundles
  - Deploy to instances defined by EC2 Tags or ASG
- `AWS::Lambda::Function`
  - Only Blue/Green deployments, of course
  - _Both_ `CurrentVersion` and `TargetVersion` for the `Name` and `Alias` specified in `appspec.yml`
  - Integrated with SAM framework
  - Does not require CodeDeploy Agent, of course
  - CodeDeploy will alter percentages for the version mapping to a prod alias
    - `LambdaLinear10PercentEvery{1Minute,{2,3,10}Minutes}`
    - `LambdaCanary10Percent{5,10,15,30}Minutes`
  - Lambda Hooks
    - (`Start`), `BeforeALlowTraffic`, (`AllowTraffic`), `AfterAllowTraffic`, (`End`)
- ECS
  - Only Blue/Green deployments
  - Only behind an ALB
  - Does not require CodeDeploy Agent, of course
  - `appspec.yml` (in an S3 bucket)
    - `TaskDefinition`
      - ECS Task Definition Revision
      - References new Container Image (in ECR)
    - `LoadBalancerInfo`
  - Strategies -- `ECSLinear10PercentEvery3Minutes`, `ECSLinear10PercentEvery10Minutes`, `ECSCanary10Percent5Minutes`, `ECSCanary10Percent30Minutes`, `AllAtOnce`
    - You can define a 2nd ELB Test Listener against Green to validate new code before shifting traffic
  - Lambda hooks
    - (`Start`), `BeforeInstall`, (`Install`), `AfterInstall`, (`AllowTestTraffic`), `AfterAllowTestTraffic`, `BeforeAllowTraffic`, (`AllowTraffic`), `AfterAllowTraffic`
- Rollbacks
  - Automatic or Manual (or disabled)
  - Implementation: a new deployment to the last known-good revision
- Troubleshooting
  - EC2 instance gotta have accurate time
  - CodeDeploy Agent might not be installed, or its service role might not have permissions
  - Set `:proxy_uri:` parameter if necessary
  - Verify ELB health checks configuration
- Really, AWS???
  - ASG scale-out will be with v1, rather than v2. (But CodeDeploy will create a follow-on deployment to fix this.)

#### AWS Elastic Beanstalk

- AWS Elastic Beanstalk
  - Alternative: CodeDeploy
  - Controlled by `appspec.yml`
    - `files` -- list of source/destination pairs
    - `hooks` -- `BeforeInstall`, `ApplicationStop`, etc
      - List of tuples -- script `location`, `timeout`, `runas`
  - Targets:
    - EC2 instances -- Requires CodeDeploy Agent (which is installed by Systems Manager and dependent upon Ruby)
      - Depends upon each EC2 instance having a tag identifying its environment
      - Q: Why isn't codedeploy-agent installable as a yum package???
      - EC2 instances must have authorization to access deployment bundles in S3
    - EC2 Autoscaling Groups -- Also requires CodeDeploy Agent
      - In-place deployment
      - Blue/Green -- new ASG created
        - ELB mandatory
        - Choose how long to retail old ASG (and EC2 instances thereof)
    - Lambda functions
      - Traffic shift integrated within SAM (Serverless Application Model) framework
      - Simply changes the percentages for the PROD (or whatever) alias.
        - `LambdaLinear10PercentEvery3Minutes`
        - `LambdaLinear10PercentEvery10Minutes`
        - `LambdaCanary10Percent5Minutes` (and then 100%)
        - `LambdaCanary10Percent30Minutes` (and then 100%)
        - `AllAtOnce`
    - ECS Platform
      - Only Blue/Green deployments; switch occurs in ALB
      - `ECSLinear10PercentEvery3Minutes`
      - `ECSLinear10PercentEvery10Minutes`
      - `ECSCanary10Percent5Minutes` (and then 100%)
      - `ECSCanary10Percent30Minutes` (and then 100%)
      - `AllAtOnce`
    - On-prem
  - Gradual deployment -- `AllAtOnce`, `HalfAtATime`, `OneAtATime`, BlueGreen or custom
  - Automated rollback
    - Rollbacks are a _new_ deployment to a last known-good revision

#### CodePipeline

- CodePipeline -- Visual Workflow to Orchestrate the above via S3 artifacts
  - Runs using an attached service role
  - Requires a single "Source Provider" -- Github, S3, Elastic Container Registry (ECR), or CodeCommit
  - Stage -- Optional "Build Provider" -- Jenkins, CodeBuild, CloudBees, TeamCity, etc
  - Stage -- Optional "Test" -- CodeBuild, AWS Device Farm, 3rd-party tools
  - Stage -- Mandatory "Deploy Provider"
    - CloudFormation
      - Action Modes
        - ChangeSets
          - Create or Replace
          - Execute (e.g., after approval)
        - Stacks
          - Create or Update (`CREATE_UPDATE`)
          - Delete (`DELETE_ONLY`) -- e.g. discard testing environment after integration testing
          - Replace Failed
      - Template parameter overrides (static and dynamic)
        - Retrieves a json-formatted parameter from an Input Artifact (`Fn::GetParam`)
    - CloudDeploy
    - Elastic Beanstalk
    - Service Catalog (Q:???)
    - ECS
    - ECS (Blue/Green)
    - S3
  - Stage -- Optional "Invoke" -- Lambda, Step Functions
  - Manual approval can be required before any stage
    - `codepipeline:GetPipeline*` against the pipeline resource
    - `codepipeline:PutApprovalResult` against the pipeline/stage/approvalAction resource
- Events
  - `codepipeline-pipeline-{action-executed-{started,cancelled,failed,succeeded},stage-execution-{started,succeeded,resumed,canceled,failed},pipeline-execution-{started,cancelled,resumed,failed,succeeded,superseded},pipeline-manual-approval-{needed,failed,succeeded}}`
  - Triggered by either:
    - CloudWatch Events (recommended)
      - Can be triggered by a _GitHub App_ -- "CodeStar Source Connection"
    - Webhooks (older)
    - Polling for changes
  - EventBridge events for failed pipelines, cancelled stages, etc.
    - A failed stage stops the pipeline and the console will show that
    - CloudTrail can be useful to audit AWS API calls to understand why a stage failed
- Output artifacts may be `CODEBUILD_CLONE_REF` or `CODE_ZIP` (default and recommended)
- Can add additional stages, each with multiple "action group"s, each of which with multiple actions (Approval, Build, Deploy, etc)
  - Manual-approval steps
    - Optional SNS topic
    - Optional URL giving context to the reviewer
    - Requires `codepipeline:{GetPipelines*,PutApprovalResult}` IAM permission
  - CloudFormation -- deploy infrastructure and app, and later on delete test infrastructure
- Note: Actions are performed in parallel; action groups are performed sequentially
  - Action Types: Source, Build, Test, Approval (owner is "AWS"), Invoke, Deploy
    - Actions have input artifacts (frequently 0, 1, or 1-4) and output artifacts (frequently 1, or 0, or 0-5) -- <https://docs.aws.amazon.com/codepipeline/latest/userguide/reference-pipeline-structure.html#reference-action-artifacts>
- Actions
  | Owner | Type | Provider | # Input Artifacts | # Output Artifacts |
  | ------ | -------- | ------------------------------------------------------------------------ | ----------------- | ------------------ |
  | AWS | Source | S3, CodeCommit, ECR | 0 | 1 |
  | 3rd | Source | GitHub | 0 | 1 |
  | AWS | Build | CodeBuild | 1-5 | 0-5 |
  | Custom | Build | Jenkins | 0-5 | 0-5 |
  | AWS | Test | CodeBuild | 1-5 | 0-5 |
  | AWS | Test | DeviceFarm | 1 | 0 |
  | Custom | Test | Jenkins | 0-5 | 0-5 |
  | AWS | Approval | Manual | 0 | 0 |
  | AWS | Deploy | S3, CodeDeploy, Elastic Beanstalk, OpsWorks Stacks, ECS, Service Catalog | 0 | 1 |
  | AWS | Deploy | CloudFormation (perhaps CDK or SAM) and CloudFormation StackSets | 0-10 | 0-1 |
  | 3rd | Deploy | Alexa Skills Kit | 1-2 | 0 |
  | AWS | Invoke | Lambda | 0-5 | 0-5 |
  | AWS | Invoke | Step Functions | 0-1 | 0-1 |
- Best practices
  - One CodePipeline, one CodeDeploy, parallel deployment groups
  - Note: duplicate `RunOrder` values cause parallel execution
  - Manual approval between PreProd CodeDeploy and Prod CodeDeploy
  - EventBridge to detect and react to stage failures, etc
  - Invoking actions
    - Lambda
    - Step Functions
      - Populate tables
      - Start ECS Tasks for load testing
- Multi-region
  - E.g., deploying Lambda code to multiple regions
  - CodeBuild will need to create one template YAML file per region, of course
  - Requirement: S3 Artifact Store in each region in which you have actions
    - Of course, CodePipeline IAM must have rights thereof
    - But CodePipeline will automatically handle cross-region artifact propagation

#### CodeStar

- CodeStar (management of the above, but EOL is 2024-07 -- to be replaced by CodeCatalyst)
  - Unitary dashboard, but limited customization
  - Lots of templates, e.g. C#, Go, HTML5, Java, Node.js, PHP, Python, Ruby
    - For example, Express.js has Lambda, Elastic Beanstalk, and EC2 templates
  - Issue tracking with JIRA or GitHub
  - Cloud9 web IDE (most regions)

#### CodeArtifact

- Alternative: Nexus OSS
- Artifact management
  - Domains contain Repositories and can span accounts, all within a single region
    - Resource policies are per-repo not per-artifact, and may be defined across an entire domain
      - Each repository can have one upstream repo
      - `codeartifact:{DescribePackageVersion,DescribeRepository,GetPackageVersionReadme,GetRepositoryEndpoint,ListPackages,ListPackageVersions,ListPackageVersionAssets,ListPackageVersionDependencies,ReadFromRepository}`
    - Deduplication within an entire domain (sharing a single KMS key, typically an AWS-managed key)
- Proxies up to ten public artifact repos (e.g. npm, Maven)
  - Network security, caching (but not intermediate repos)
- Package version events to EventBridge
  - Can trigger CodePipeline to redeploy with library security fixes
- Integrates with Maven, Gradle, npm, yarn, twine (Python), pip (Python), NuGet (.NET)
  - `pip` -- `aws codeartifact login --tool pip --repository ${REPO_NAME} --domain ${DOMAIN} --domain-owner ${ACCT_ID} --region ${REGION}` (good for 12h)
    - Or `pip3 config set global.index-url=https://aws:$TOKEN@$DOMAIN-$ACCT.d.codeartifact.$REGION.amazonaws.com/pypi/$REPO/simple/`
- Pricing
  - Free Tier -- 2GiB storage, 100000 req/month
  - Otherwise -- 0.05USD/GiB/Month, 0.05USD/10k requests, 0.09USD/GiB egress

### CodeGuru

- CodeGuru Reviewer (ML-powered static code analysis)
  - Currently Java and Python
  - Integrates with BitBucker, GitHub, and AWS CodeCommit
  - Detects
    - Resource leaks
    - Security vulnerabilities
      - Hardcoded secrets/credentials/keys (including in config and documentation)
    - Input validation
- CodeGuru Profiler
  - Requires Agent
    - `MaxStackDepth`, `MemoryUsageLimitPercent`, `MinimumTimeForReportingInMilliseconds`, `ReportingIntervalInMilliseconds`, `SamplingIntervalInMilliseconds`
  - Application performance recommendations
  - AWS or on-prem
    - Manual
      - Lambda code can include `@with_lambda_profiler(profiling_group_name="MyGroup")` 
      - Requires `codeguru_profiler_agent` in .zip or Lambda Layer
    - Automatic -- just enable profiling in the function config
  - Minimum overhead

### EC2 Image Builder

- Invoked from AWS CloudFormation, which can then do rolling updates
- Starts with a bare-bones "Builder EC2" instance, to which build components are applied
- New AMI is tested and then distributed (to multiple regions and/or accounts)

### Remote Access Manager (RAM)

- Share images, recipes, and components across AWS accounts (or throughout an AWS organization)
- SSM Parameter Store can know the "latest" ami-ids.
  - Image Builder -> SNS -> Lambda -> SSM 

### Cloud9

- AWS Cloud9 (Cloud-based IDE)
  - Each Cloud9 environment requires an EC2 instance (type can't be changed after creation)

## SAM (Serverless Application Model)

- Simple YAML generating lots of CloudFormation
- Uses CodeDeploy to deploy Lambda functions
- Allows running locally: Lambda, API Gateway, DynamoDB
  - `sam local start-lambda` or `sam local invoke` (with `--profile`)
  - `sam local start-api`
  - `sam local generate-event`
- Recipes
  - `AWSTemplateFormatVersion: '2010-09-09'`
  - `Transform: 'AWS::Serverless-2016-10-31'`
  - Code
    - `AWS::Serverless::Function`
    - `AWS::Serverless::Api`
    - `AWS::Serverless::SimpleTable`
  - Package/deploy
    - `aws sam build`
      - Creates CloudFormation Template and application code
    - `aws sam package` (`aws cloudformation package`)
      - Stores zip into S3 bucket
    - `aws sam deploy` (`aws cloudformation deploy`)
      - Creates/executes CloudFormation ChangeSet
        - Lambda, API Gateway, DynamoDB
- SAM CLI allows running local build/test/debug apps (requires Docker, of course)
  - Supports Cloud9, VSCode, JetBrains, PyCharm, IntelliJ
  - `sam init --runtime` -- `python`, `nodejs`, `dotnetcore`, `dotnet`, `go`, `java` (and versions thereof)
    - Also specify `--location` for the template (perhaps even a `gh` (GitHub) file)
- Layout
  - `template.yaml`
    - Can define Lambda handlers and then one or more Api `Events` which map to it.
    - DynamoDB tables of type `AWS::Serverless::SimpleTable` (can set ProvisionedThroughput and ServerSideEncryption (SSE) as needed)
      - PrimaryKey
      - Lambda function can have a `DynamoDBCrudPolicy` with a TableName of `!Ref TableNameParameter`
    - Environment variables can be `!Ref` to other resources, or stuff like `!Ref AWS::Region`

### SAM -- DynamoDB

- Python: `import boto3, json, os; dynamo = boto3.client('dynamodb', region_name=os.environ['REGION_NAME']); table_name = os.environ['TABLE_NAME']; # outside handler`

### SAM Policy Templates

- <https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-policy-template-list.html>, e.g. `S3ReadPolicy`, `SQSPollerPolicy`, `DynamoDBCrudPolicy`, etc
  - `Policies:[SQSPollerPolicy:QueueName:!GetAtt MyQueue.QueueName]`

### SAM and CodeDeploy (traffic shifting)

- `AutoPublishAlias: live`
- `DeploymentPreference`
  - `Type: Canary10Percent10Minutes`
  - `Alarms: [!Ref AliasErrorMetricGreaterThanZeroAlarm, !Ref LatestVersionErrorMetricGreaterThanZeroAlarm]`
  - `Hooks:PreTraffic: !Ref PreTrafficLambdaFunction`
  - `Hooks:PostTraffic: !Ref PostTrafficLambdaFunction`

### Serverless Application Repository (SAR)

- Share SAM-packaged apps publicly or amongst specified accounts
  - `sam publish`
- `MetaData:"AWS::ServerlessRepo::Application":{Name,Description,Author,SemanticVersion}`

## AWS Cloud Development Kit (CDK)

- Define cloud infrastructure using code -- TypeScript, Python, Java, .NET
- Emits CloudFormation templates (JSON/YAML)
- Can deploy infrastructure and runtime together
  - Lambda
  - Docker (ECS/EKS)
- CDK vs SAM
  - SAM -- Serverless-focused
  - CDK -- All AWS services
- SAM CLI can locally test CDK
  - `csk synth && sam invoke -t MyStack.template.json myFunction`
- `npm install -g aws-cdk-lib; cdk init app --language $CDK_LANGUAGE`
- `cdk ls`
- `cdk bootstrap` (once per account per region) -- Creates a "CDKToolkit" CloudFormation stack
- `cdk synth` -- creates the actual CF template
- `cdk diff` -- Local CDK vs deployed stack
- `cdk deploy` -- Lots of magic
- `cdk destroy` -- Be careful!
- Sample sequence
  - Create app from template
  - Add code to create more resources
  - Build the app
  - Synthesize stacks
  - Deploy stacks

### CDK -- Constructs

- Single AWS resource or group of related resources
- AWS Construct Library (but also Construct Hub from 3rd parties)
  - L1 -- CFN Resources (directly available) `new s3.CfnBucket(this, "MyBucket", {bucketName: "MyBucket"})`
  - L2 -- Higher-level (intent-based API, with convenient defaults and boilerplate) `const bucket=new s3.Bucket(this, 'MyBucket', {versioned:true, encryption: s3.BucketEncryption.KMS}); const url=bucket.urlForObject("MyBucket/MyObject"); bucket.addLifeCycleRule());`
  - L3 -- Patterns
    - `aws-apigateway.LambdaRestApi` is an Lambda function wrapped by an API Gateway

### CDK -- Testing

- CDK Assertions Module
  - Fine-grained assertionsA
  - Snapshot Tests -- compares against a baseline
- Can also compare against a CloudFormation template file itself (`Template.fromString()`)

## Cognito -- User Pools for authentication and Identity Pools for authorization

- Cognito User Pools (CUP)
  - Sign-in functionality for app users
  - Integrates with API Gateway and ALB
  - Serverless user database
    - MFA, email/phone verification
    - Adaptive Authentication -- block sign-in or require MFA
      - Also checks for compromised credentials
      - Integration with CloudWatch Logs
  - Federated Identities -- Amazon, Facebook, Google, SAML, OpenID Connect
  - Block users with compromised credentials
  - Provides a JWT (JSON Web Token)
    - Base64
    - Header
    - Payload
      - `sub` -- UserID in CognitoDB
      - `email` and `email_verified`
      - `cognito:{username,groups,roles,preferred_role}`
      - `iss`
      - `nonce`
      - `jti`, `origin-jti` (token identifier)
      - `aud`
      - `event_id`
      - `token_use` ("id")
      - `auth_time`, `exp`, `iat` (issued-at time)
    - Signature
  - Doesn't appear to support "correct horse battery staple"
  - Email from Cognito itself (<50/day, just for testing)
  - Email from SES (requires verifying identity)
  - Optionally provides UI for sign-up, sign-in, Oath 2.0
    - Domain can be AWS-owned
    - Domain can be custom DNS domain (recommended for production)
      - Defined in "App Integration"
      - ACM certificate in `us-east-1`
  - Lambda triggers (synchronous; up to one per type)
    - Sign-up
      - Pre-sign-up
      - Post-confirmation
      - Migrate user trigger
    - Authentication
      - Pre-authentication (perhaps deny login)
      - Post-authentication (logging/analytics)
      - Pre-token-generation (add/remove token attributes)
    - Custom authentication (e.g. CAPTCHA or security question)
      - Define auth challenge
      - Create auth challenge
      - Verify auth challenge
    - Messaging
      - Custom message trigger

### Cognito Identity Pools (Federated Identity)

- Provides temporary AWS credentials
  - More users than IAM supports
  - IAM policies allow access to API Gateway or AWS services itself
- Allows identities from
  - Social providers
  - Cognito User Pool
    - Allows "guest"
  - OpenIDConnect and SAML providers
  - Custom login server (Developer Authenticated Identities)
- Gets temp credentials from Security Token Service (STS) service
  - 15m-60m duration
  - `AssumeRole`, `AssumeRoleWithSAML`, `AssumeRoleWithWebIdentity` (but use Cognito User Pools instead)
  - `GetSessionToken`, `GetFederationToken`, `GetCallerIdentity`
  - `DecodeAuthorizationMessage` (when denied)
  - TODO -- <https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_common-scenarios_aws-accounts.html>
  - MFA
    - `GetSessionToken` -- returns Access ID, Secret Key, Session Token, Expiration Date
    - check `aws:MultiFactorAuthPresent:true`
- Default IAM role for guests and authenticated users
  - Rules to determine role based on userID
  - Partition access using `policy variables`
    - Resources can be per-user, e.g. `"Resource": ["arn:aws:s3:::myBucket/${cognito-identity.amazonaws.com:sub}/*"]`
      - Ditto for `"dynamodb:LeadingKeys"`
- `sts:TagSession` -- TODO <https://docs.aws.amazon.com/IAM/latest/UserGuide/id_session-tags.html>
- Steps:
  - Import AWS SDK
  - Authenticate
  - Register identity and retrieve AWS credentials
  - Use credentials

### ALB Authentication

- Options
  - Cognito User Pools
    - Social IdPs (Amazon, Facebook, Google, Apple, Twitter)
    - Corporate identities (SAML, LDAP, or Microsoft AD)
  - Identity Provider (IdP) -- must be OpenID Connect (OIDC) compliant
    - Need Authentication Endpoint, Token Endpoint, User Info Endpoint
- Requires HTTPS listener with `authenticate-oidc` or `authenticate-cognito`
- Define `OnUnauthenticatedRequest`
  - "authenticate" (default)
  - "deny"
  - "allow" (e.g. for Login page)
- Cognito User Pools
  - Create User Pool, Client, DOmain
  - Ensure ID tokens are returned (default)
  - Add social/corporate IdP
  - Permission callback URLs (e.g. `https://$DOMAIN_PREFIX.auth.$REGION.amazoncognito.com/saml2/idresponse` or `https://$USER_POOL_DOMAIN/oauth2/idpresonse`)

## AWS Step Functions

- One state machine per workflow
  - Workflows initialized with an input document
  - Order fulfillment, data processing, etc
- State machine defined as a JSON document
- Workflow triggered by SDK call, API Gateway, Event Bridge (CloudWatch Event)
- Tasks
  - Invoke a Lambda function
  - Run an AWS Batch job
  - Synchronously run an ECS task
  - Insert into DynamoDB
  - Publish to SNS or SQS
  - Launch a 2nd Step Function workflow
  - Or we may run a server (EC2, ECS, on-prem) which polls for step-function work and sends results back
    - Poll: `GetActivityTask`
    - Response: `SendTaskSuccess`, `SendTaskFailure`
    - Still working: `SendTaskHeatBeat` (up to 1 year!!!)
- States
  - Choice state (conditional)
  - Terminal (success or failure)
  - Pass data (no-op, but perhaps inject fixed data)
  - Wait (duration or point in time)
  - Map State (iterate along collection)
  - Parallel state (fork)
- Error handling
  - `Retry` -- Optional exponential-backoff retry
    - Separate strategies for different errors
    - Transition to `Catch` once all retries fail
  - `Catch` -- Transition to new state
    - `"ResultPath": "$.error"` adds error info into the JSON passed to the next state
  - Errors include: `States.{Timeout,TaskFailed,Permissions}` (and custom errors).
    - `States.ALL` matches any error
- Side note:
  ```javascript
  exports.handler = async (event, context) => {
    function MyError(msg) {
      this.name = "MyError";
      this.message = msg;
    }
    MyError.prototype = new Error();
    throw new MyError("I don't like this");
  };
  ```
- ```javascript
  {"Resource": "arn:aws:states:::sqs:sendMessage.waitForTaskToken",
   "Parameters": {
    "QueueUrl": "https://sqs.$REGION.amazonaws.com/$ACCT/MyQueue",
    "MessageBody": {
      "Input.$": "$",
      "TaskToken.$": "$$.Task.Token"}}}
  ```
  - And then the resource calls `SendTaskSuccess` (or `SendTaskFailure`) with the taskToken and an output.
- Standard
  - Up to 1y for entire workflow, up to 2k/s, exactly-once, 90d history
    - Priced per state transition
    - Suitable for non-idempotent
- Express
  - Best if steps are idempotent
  - Workflows only up to 5m, but up to 100k/s. Only CloudWatch history
    - Priced per executions, duration, memory
  - Asynchronous -- at least once
    - Fire-and-forget
  - Synchronous -- at most once

## AppSync -- Managed GraphQL

- Can join data from multiple sources
  - NoSQL, RDBMS, HTTP APIs
  - DynamoDB, Aurora, OpenSearch, etc
  - Lambda
- Real-time data with WebSocket or MQTT
  - Supersedes "Cookie to Sync"
- Mobile apps: local data access and data sync
- Driven by a GraphQL schema
- Authorized by
  - API_KEY
  - AWS_IAM
  - OPENID_CONNECT (JWT)
  - AMAZON_COGNITO_USER_POOLS
- Works well behind CloudFront https termination
- Optional caching

## AWS Amplify

- Amplify -- "Elastic beanstalk for mobile and web"
  - Code in CodeCommit, GitHub, Bitbucket, GitLab, or manually uploaded
  - Frontend
    - Amplify Studio
    - Amplify Libraries
      - React.js, Vue, JS, iOS, Android, Flutter, ...
  - Backend
    - Configured using Amplify CLI
    - Amplify Hosting (CDN)
      - CloudFront
    - API Gateway, DynamoDB, Cognito, S3, Lambda
      - SageMaker
      - Lex
    - AppSync
  - Build-and-deploy
    - Amplify Console
    - Can configure each CodeCommit branch to have its own deployment (typically connected to their own Route53 domain)
  - E2E testing in the "test phase"
    - `amplify.yml`
    - <https://www.cypress.io/>

## AWS Security and Encryption

- SSL against MITM
- Client-side vs server-side encryption
- KMS auditable using CloudTrail
  - Integrated into EBS, S3, RDS, SSM
  - Symmetric (AES-256) -- Never exposed to user...instead they just get to call encrypt/decrypt methods
  - Asymmetric -- Client can encrypt or validate signature.
- Ownership (typically single-region)
  - AWS-owned -- SS3-S3, SSE-SQS, SSE-DDB
  - AWS-managed -- aws/rds, aws/ebs, etc
  - Customer-managed -- 1USD/month + $0.03/10k calls
- Rotation -- yearly, except for manually-imported KMS keys, which can only be rotated manually and only if using aliases
- Permissioning -- Similar access policies to S3 policies, but default-DENY
- (CloudHSM is out-of-scope for test)

### Security Token Service (STS)

- Access to AWS resources for a limited period of time
  - At least 15m
  - Up to 1h for AWS account owners
  - Up to 36h for IAM accounts
- Methods
  - `AssumeRole` (intra-account or inter-account)
  - `AssumeRoleWithSAML` (for users with SAML assertions)
  - `AssumeRoleWithWebIdentity` (for users logged-in with an IdP; AWS recommends Cognito Identity Pools instead)
  - `GetSessionToken` (MFA)
    - Returns:
      - `AccessKeyId`
      - `SecretAccessKey` -- for HMAC signatures
      - `SessionToken` -- Probably so AWS doesn't have to store the temporary session
      - Expiration
  - `GetFederationToken` (temporary credentials)
  - `GetCallerIdentity` (IAM user or role used in the call)
  - `DecodeAuthorizationMessage` (when AWS API denies)
- Steps
  - Define an IAM Role
  - Define which principals may access the role
  - Call `AssumeRole`
  - Profit! (For 15m-60m)
- Note: one can have an IAM Policy including a Condition using on `"aws:MultiFactorAuthPresent":"true"`

## KMS

- Type of keys
  - AWS-owned (free) -- SSE-S3, SSE-SQS, SSE-DDB, etc
  - Customer-managed (1 USD/month)
- Usage (0.03USD / 10000 calls)
- KMS Key Policies similar to S3 bucket policies
- Copying snapshots
  - `{"Principal": {"AWS": "arn:aws:iam::$TARGET-ACCOUNT:role/$ROLE"}}`
  - `{"Action": ["kms:Decrypt", "kms:CreateGrant"]}`
  - `{"Condition": {"StringEquals": {"kms:ViaService": "ec2.$REGION.amazonaws.com", "kms:CallerAccount": "$TARGET-ACCOUNT"}}}`
- Encrypt API limited to 4k
  - `GenerateDataKey` returns
    - Plaintext DEK (for symmetric encryption of data)
    - Encrypted DEK (to include in the final file "envelope")
  - Encryption SDK does this for us
    - LocalCryptoMaterialsCache significantly reduces KMS cost
    - S3 Bucket Key for SSE-KMS dramatically reduces KMS cost (automatically rotated)
      - Fewer KMS CloudTrail events
  - `aws-encryption-api` also does this for us
- Quota shared between all cryptographic operations and include both direct and indirect (e.g. S3) usage
  - Decrypt, Encrypt, GenerateDataKey, GenerateDataKeyWithoutPlaintext, GenerateRandom, ReEncrypt, Sign (asymmetric), Verify (asymmetric)
  - Symmetric: 5500/s-30000/s
  - RSA: 500/s
  - ECC: 300/s
  - Consider DEK caching
- Federated Users (`arn:aws:sts::$ACCT:federated_user/$USERNAME`) can have IAM rights to kms operations. (security token service)

### SSM Parameter Store

- Serverless, version-tracking, IAM security, EventBridge notification, CloudFormation integration
- Optional KMS encryption
- Hierarchically-structured key/value pairs, e.g. `/my-department/my-app/dev/db-url`
  - | Parameter                     | Standard | Advanced             |
    | ----------------------------- | -------- | -------------------- |
    | Parameters per account/region | 10k      | 100k                 |
    | Max parameter size            | 4KB      | 8KB                  |
    | Parameter policies (TTL)      | no       | yes                  |
    | Cost                          | free     | nope                 |
    | Storage                       | free     | USD 0.05/param/month |
    - Notifications to EventBridge, of course
- Also, publicly-available parameters
  - `/aws/service/global-infrastructure/{regions,services}`
  - `aws ssm get-parameter --name /aws/service/global-infrastructure/regions/us-west-1/services/s3/endpoint --output json | jq '.Parameter.Value'`

### Secrets Manager

- More expensive than SSM Parameter Store
- Automatic secret-rotation using Lambda
- Mandatory KMS encryption
- Integration with RDS (MySQL, PostgreSQL, Aurora)
- Can keep cross-region read replicas in sync
- Available via SSM Parameter Store API
- Tight integration with CloudFormation
  ```yaml
  Resources:
    MyCluster:
      Type: AWS::RDS::DBCluster
      Properties:
        Engine: aurora-mysql
        MasterUserName: marshall
        ManageMasterUserPassword: true
      Outputs:
        Secret:
          Value: !GetAtt MyCluster.MasterUserSecret.SecretArn
  ```
  ```yaml
  Resources:
    MyRDSDBInstanceRotationSecret:
      Type: AWS::SecretsManager::Secret
      Properties:
        GenerateSecretString:
          SecretStringTemplate: '{"username": "admin"}'
          GenerateStringKey: password
          PasswordLength: 16
          ExcludeCharacters: "\"@\\"
    MyRDSDBInstance:
      Type: AWS::RDS::DBInstance
      Properties:
        DBInstanceClass: db.t2.micro
        Engine: mysql
        MasterUsername: !sub "{{resolve:secretsmanager:${MyRDSDBInstanceRotationSecret}::username}}"
        MasterUserPassword: !sub "{{resolve:secretsmanager:${MyRDSDBInstanceRotationSecret}::password}}"
    SecretRDSDBInstanceAttachment:
      Type: AWS::SecretsManager::SecretTargetAttachment
      Properties:
        TargetType: AWS::RDS::DBInstance
        SecretId: !Ref MyRDSDBInstanceRotationSecret
        TargetId: !Ref MyRDSDBInstance
  ```

## Misc

### Serverless architecture <https://www.youtube.com/watch?v=9IYpGTS7Jy0> (Heitor Lessa)

- Pillars: operations, reliability, security, performance, cost
- Example code
  - <https://github.com/aws-samples/aws-serverless-airline-booking/tree/archive>
  - <https://github.com/amazon-archives/realworld-serverless-application> (and wiki thereof)
- Internal components
  - Resolvers
    - Query
    - Mutation
  - Data Sources
    - DynamoDB
    - Lambda
    - Step Functions
    - RDS
- Best practices
  - Enable access logs, structure logs into consistent json, and instrument your code
  - CloudWatch Embedded Metric Format (EMF) for async metrics from Lambda
    - Just include metrics (up to 100) in the JSON logging
  - Regulate inbound access rate
    - Simplistic -- limit Lambda concurrency
    - Async -- place Kinesis Data Streams (or SQS) between API Gateway and Lambda
      - Optional batching window for efficiency
      - Lambda Destinations rather than DLQ for failures
  - Authorize customers; manage secrets with Secrets Manager
    - If using parameter store, note default QPS is about 1000/s
  - For DynamoDB, on-demand tables work well to 40K r/w request units
  - Regional endpoints support HTTP/2
  - Lambda Power Tuning is cool
- GraphQL -- AWS AppSync (and Cognito) + DynamoDB
  - Apache Velocity templates for simple CRUD
  - Lambda for complex logic
  - Pipeline resolvers for simple transactions
  - State machines (AWS Step Functions) for long transactions
  - Enforce AuthZ at API, data field, and operation level
  - Can mix-and-match DB (e.g. DynamoDB for most stuff, but Elastic Search for specific fields)
  - Enable selective caching
- Fanout
  - Option: API Gateway to SNS
  - Option: API Gateway to SNS to SQS

### Vue.js

- Videos
  - <https://www.youtube.com/watch?v=CvWcKQYldUw>
- Features
  - Declarative rendering
  - Reactivity via virtual DOM (state included within `{{ }}`)
    - Conditional -- `v-if` (truthy) and `v-else`
    - Callbacks -- `v-on:click="callback()"`
      - Can `$emit` other events
    - Models -- `v-model` and `defineModule<string>()`
- APIs -- Options and Composition (newer)
- Single-File Component (SFC) -- `.vue`

  ```html
  // Composition API
  <script setup lang="ts" generic="T">
    import { ref } from "vue";
    const count = ref(0);
  </script>

  <template>
    <button @click="count++">Count is: {{ count }}</button>
  </template>

  <style scoped>
    button {
      font-weight: bold;
    }
  </style>
  ```

- `defineComponent` expects a function which accepts a props record and returns a callback returning JSX
- Vue 3.3
  - Compiler magic: result of `defineProps` can now be destructured (with optional defaults) into reactive vars
  - `defineEmits` now uses "labelled tuples" syntax
  - `defineSlots` (sub-JSX) now has typed slots
  - Imported types
  - Generic Components
  - Be sure to enable `"jsxImportSource":"vue"` in jsconfig.json's "compilerOptions"

### To Learn

- Apache NiFi -- digraphs of data routing/transformation/etc
- Apache Avro -- row-oriented data serialization (and RPC) framework (originally for Hadoop)
  - Schemas written in Avro IDL or JSON
    - `type`
      - Complex: "record", "enum", "array", "map", "union", "fixed" (number of bytes)
      - Primitive: "null", "boolean", "int", "long", "float", "double", "bytes", "string"
    - `name` -- fieldName
    - `default`
    - `fields` -- list of name/type/default tuples
  - Serialized to JSON or binary
  - Not column-oriented like ORC or Parquet
- Apache Spark -- large-scale data processing (especially for real-time processing and iterative analytics)
  - Resilient Distributed Datasets (RDD)
  - Python and Scala
- Elastic MapReduce (EMR)
  - Java, Hive, Pig, Cascading, Ruby, Perl, Python, R, PHP, C++, Node.js
- AWS Amplify
