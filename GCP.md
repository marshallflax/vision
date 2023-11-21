# Google Cloud Platform

  
## IAM

- Principals
  - Google account (`@gmail.com`, etc)
  - Service account
  - Kubernetes service account
    - <https://cloud.google.com/kubernetes-engine/docs/how-to/kubernetes-service-accounts>
    - Authn Pods to K8s API server
    - Authn Pods to GC resources via Workload Identity
  - Google group
  - Google Workspace account
  - Cloud Identify domain
  - All authenticated users
  - All users
- Application Default Credentials (ADC) search order:
  - `${GOOGLE_APPLICATION_CREDENTIALS}` points to a credential `.json` file
  - `gcloud auth application-default login` --> `~/.config/gcloud/application_default_credentials.json`
  - `METADATA_KEY= curl "http://metadata.google.internal/computeMetadata/v1/instance/${METADATA_KEY}" -H "Metadata-Flavor: Google"`
    - Q: What's the `METADATA_KEY` for credentials?

## Load Balancers

- Application Load Balancer (HTTP/HTTPS with SSL termination)
  - External
    - Global external ALB (global or classic)
    - Regional external ALB
  - Internal
    - Cross-region internal ALB
    - Regional internal ALB
- Network Load Balancer (TCP, UDP, other IP)

## GCP Databases

- Relational
  - Cloud Spanner (global transactional consistency)
  - BigQuery (data warehouse)
    - Tables may be clustered and partitioned
      - <https://cloud.google.com/spanner/docs/schema-design>
    - Near-real-time with Datastream for BigQuery
    - Pricing
      - Per edition -- Standard (only table-level security), Enterprise, Enterprise Plus (CMEK), on-demand
  - Cloud SQL (MySQL, PostgresSQL, SQL Server)
    - Replication with Datastream
  - AlloyDB for PostgreSQL (and AllowDB Omni)
  - Bare Metal Solution for Oracle
- Key-value
  - BigTable (NoSQL)
    - HBase-compatible
      - Key/value, sparsely-populated
        - Billions of rows, thousands of columns, PiB of data
      - May be queried from BigQuery
    - Pricing
      - Instance Type
      - Number of nodes
      - Storage
      - Network
- Document
  - Firestore
    - Native SDKs for iOS, Android, browser
      - Also Node.js, Java, Python, Unity, C++, Go, REST, RPC SDKs
    - Expressive querying
    - Realtime updates
    - Offline support
  - Firebase Realtime Database - <https://firebase.google.com/products/realtime-database/>
    - Cloud Functions for Firebase - <https://firebase.google.com/docs/functions>
      - Triggered by background events, HTTPS requests, Admin SDK, Cloud Scheduler
      - JS/TypeScript (Node.js), Python code
      - Uses
        - Firebase Cloud Messaging (FCM)
        - Database sanitization
        - Background processing (e.g., create thumbnail)
  - MongoDB Atlas
- In-memory
  - Memorystore
    - Redis
    - Memcached
    - Memorystore for Redis Cluster

## Redis

- GCP Redis cluster now Beta <https://cloud.google.com/blog/products/databases/memorystore-for-redis-cluster-launched>
- Eventual consistency using CRDT (conflict-free replicated data types)
- Server-side scripting in Lua
- Virtual memory now deprecated
- Journaling at least every 2s, with background rewriting
- Client
  - <https://lettuce.io/>
    - Supported by <https://docs.spring.io/spring-data/data-redis/docs/current/reference/html/>
  - Usually use `RedisTemplate` (thread-safe) or `StringRedisTemplate`
    - (Bound)?ValueOperations
    - (Bound)?ListOperations
    - (Bound)?SetOperations
    - (Bound)?ZSetOperations (sorted)
    - (Bound)?GeoOperations
    - (Bound)?HashOperations
    - (Bound)?HyperLogLogOperations

## Apigee (API Proxy)

- Rate limiting
- XLB (External Load Balancer) to Apigee to ILB (Internal Load Balancer) to Anthos Service Mesh
- REST, gRPC, GraphQL?
- ML for threat detection?

## Terraform

- Current version is 1.6.3
- Usage
  - `terraform init`
  - `terraform plan`
  - `terraform apply`
  - `terraform destroy`
- Syntax
  - Arguments and Blocks
    - Arguments assign a value to a name
    - Blocks has a type e.g., `resource`, zero-or-more labels (depending upon the type), and a block body within curly braces
  - Top-level blocks
    - `terraform`
      - `required_version` (of TF)
      - `required_providers`
    - `provider`
      - For `gcp`, set `project`, `region`, `zone`
    - `resource`
      - Specifies actual resources
      - First label is type of resource, second label is the identifier to be used for the resource
      - Meta-arguments
        - `depends_on` 
          - Only necessary when you're not using any of that resource's data
          - Prefer expression references
        - `count` or `for_each` (but not both)
          - `${count.index}` (zero-based)
          - `each.key` and `each.value`
            - keys must be defined purely (no `uuid`, `bcrypt`, `timestamp`, etc)
            - Neither keys nor values may contain sensitive data
          - `self` (refers to instance within `provisioner` and `connection` sub-blocks)
        - `provider`
          - Might specify `region`, for example
          - May have aliases
        - `lifecycle`
          - `create_before_destroy` 
            - Propagates to dependencies
          - `prevent_destroy`
            - **Does not protect against removing the `resource` block entirely**
          - `ignore_changes`
            - Cannot apply to meta-arguments themselves
          - `replace_triggered_by`
          - `precondition` and `postcondition`
    - `variable`
      - One per module parameter, typically in `variables.tf`
      - `type`, `description`, `default`, `sensitive`
      - Precedence, highest to lowest
        - Command-line (`-var`, and `var-file`)
        - `*.auto.tfvars` or `*auto.tfvars.json`
        - `terraform.tfvars.json`
        - `terraform.tfvars`
        - Environment vars
        - Variable defaults
    - `locals`
      - Locally-defined variables
      - Referenced with `local.` prefix
    - `data`
      - Variables populated from other APIs
      - Referenced with `var.` prefix
    - `output`
      - Returned parameter
    - `provisioner`
      - Actions to be performed while creating resources
      - `local-exec` and `remote-exec`
    - `module`
      - Multiple `.tf` and `.tf.json` files in a directory
- TDD <https://www.hashicorp.com/resources/test-driven-development-tdd-for-infrastructure>
  - <https://www.openpolicyagent.org/docs/latest/policy-language/>
- Read file 1GiB into 8Meg.
- EventHub -- highly scalable

## Hashicorp Vault

- Open-source, Managed, and Enterprise editions
- Secrets, K8s secrets (Helm), dynamic secrets
  - Automatically-rotated DB credentials, IAM, Automated X.509 PKI
  - Data encryption and tokenization
- Users/principals ("clients") authentication -- results in token:
  - Azure, GCP, AWS, Alibaba, Oracle Cloud, GitHub, K8s
  - LDAP, Okta, RADIUS, JWT/OIDC, PKI Cert, Custom

## "Done"

- Questions
  - Weekly or biweekly sprint cadence? (Preference: slow at first, then accelerating)
- Project Aristotle -- <https://web.archive.org/web/20230324023515/https://rework.withgoogle.com/print/guides/5721312655835136/>
  - Important
    - Psychological safety
    - Dependability
    - Structure and clarity
    - Meaning
    - Impact
  - Not important
    - Team colocation
    - Consensus decision-making
    - Introversion/extroversion
    - Individual performance
    - Seniority
    - Team size
    - Tenure
- DORA core model -- <https://dora.dev/research/>
  - Principles
    - Shift left on security (and privacy!)
    - Continuous delivery
    - Generative organizational culture
    - Streamlined change approval
  - Indicia
    - Change latency (from initial completion of coding to deployment)
    - Deployment frequency
    - Change fail fraction
    - Time to restore service
  - Reduce
    - Deployment Pain
    - Rework
    - Burnout
  - Technical capabilities
    - Code maintainability
    - Continuous Integration
    - DB change management
    - Deployment automation
    - Loose coupling
    - Monitoring and observability
    - Test automation, Test data management
    - Trunk-based development
    - Version-control
  - Foundation
    - Flexible infrastructure
    - Documentation quality
- Agile Principles
  - Individuals and interactions vs. processes and tools
  - Working software vs. comprehensive documentation
  - Customer collaboration over contract negotiation
  - Responding to change over following a plan

## Jakarta EE

- Fujitsu, IBM, Oracle, Payara, Tomitribe, Microsoft, RedHat, etc
- Java EE from Oracle, Eclipse Foundation for Cloud Native Java
- Based on Java EE 8, but `javax.*` replaced with `jakarta.*` namespace. EPL2.0
- Roadmap
  - Microservice support
  - Cloud Native Java (integrations with Docker and K8s)
- Deployed as `war`
- Annotations
  - `@ApplicationScoped` -- singletons and active while processing requests

## Java VMs

- GraalVM
  - Quarkus (RedHat) -- k8s Native Java (OpenJDK Hotspot and GraalVM)
  - GraalVM Community Edition, GraalVM Enterprise Edition, Mandrel (based on OpenJDK)
  - <https://developers.redhat.com/e-books/quarkus-spring-developers>

## Gradle

- `dependencies`
  - `providedCompile` -- required at compile-time but provided by WAR deployment environment

## Misc

- $(n/2)^2 + 2(n/4)^2 + 4(n/8)^2 + 8(n/16)^2 ... + n/2(2)^2$

## TODO

- GitOps
  - Cloud Source Repositories
  - Whitesource RenovateBot
  - Cloud Build
    - Unit/pre-submit testing
    - Integration testing
    - Deployment
- <https://graalvm.github.io/native-build-tools/latest/gradle-plugin.html>
  - BigQuery, BigTable, Logging, Spanner, Storage, Tasks, Trace, Datastore (now Firestore) (NoSQL), Pub/Sub, Secret Manager
- <https://tekton.dev/> (Linux Foundation)
- <https://knative.dev/docs/concepts/>
- <https://cloud.google.com/memorystore/docs/eedis>
- | Avro                | Orc                           | Parquet                                                 |
  | ------------------- | ----------------------------- | ------------------------------------------------------- |
  | Row-based           | Columnar                      | Columnar                                                |
  | Self-describing     | Zlib or Snappy compression    | Self-describing, per-column compression                 |
  | Preferred for Kafka | Supports ACID for Apache Hive | AWS Athena, GCP BigQuery, Apache Spark, Drill, etc, etc |
  | Schema evolution    |                               | Schema evolution                                        |
- CQRS (Command Query Responsibility Separation)
  - Redis CDC (Change Data Capture) -- eventual consistency
- Event sourcing
  - Store in Redis Streams
- Native Java
- Neo4j
- Datapower
