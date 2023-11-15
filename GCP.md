# Google Cloud Platform

## Kubernetes

- Features
  - Imperative (command-line and file-based) and Declarative (directory-based) APIs
    - Declarative approach offers `patch` (respect drift) and `replace` (overwrite drift)
  - Service discovery and load balancing
  - Expose a container using a DNS alias (or own IP)
    - IPv4/IPv6
  - Storage orchestration (local, cloud storage, etc)
  - Automated rollout/rollback
  - Bin-packing
    - Each container specifies its CPU and RAM needs
  - Self-healing
  - Horizontal scaling
  - Configuration management, including secrets
  - Batch and CI workloads
- Does not provide
  - Preferred CI/CD
  - Middleware, Map/Reduce, DB, Caches
  - Logging, Monitoring, Alerting

### K8s objects

- Persistent entities representing the desired state of a cluster
  - What containerized apps are running (and on which nodes)
  - Resources available to apps
  - Application policies
    - Restart
    - Upgrades
    - Fault-tolerance
- CRUD via K8s API (RESTful), e.g. `kubectl` or via a client library
- K8s object fields <https://kubernetes.io/docs/concepts/overview/working-with-objects/_print/>
  - Typically `.yaml` -- <https://kubernetes.io/docs/concepts/configuration/_print/>
  - `apiVersion:`, e.g. "apps/v1"
  - `kind:`, e.g. "Deployment"
  - `metadata:`
    - `name:`, e.g. "nginx-deployment" (normally RFC-1123 compliant DNS subdomain style)
    - `labels: -`, e.g., string key/value pairs (e.g., release, environment, tier, partition, track, etc, etc)
      - Keys (<253char) may have a subdomain prefix (`kubernetes.io/` and `k8s.io/` are reserved)
      - Values (<63char)
      - Selectable by equality-based and set-based
        - `,` conjunction as a logical `&&`
    - `annotations: -` -- less constrained than labels, but cannot be used for selection
      - Build info -- timestamps, branches, tags, registry, etc
      - Support contact info, etc
      - Managed fields
    - Also, every object also has an UUID
  - `spec:` -- desired state, <https://kubernetes.io/docs/reference/kubernetes-api/_print/>
    - `selector:matchLabels:app:`, e.g. "nginx"
    - `replicas:`, e.g. 2
    - `template:`
      - `metadata:labels:app:`, e.g. "nginx"
      - `spec:containers:`
        - `name:`
        - `image:`
        - `ports:containerPort:`
  - `status:` -- current state

### K8s Namespaces

- Initial namespaces (`kubectl get namespace`)
  - `default`
  - `kube-node-lease`
    - kubelet heartbeats to control plane
  - `kube-public`
    - Even available to unauthenticated client
  - `kube-system`
    - Objects created by k8s itself
- Namespace preference
  - `kubectl config set-context --current --namespace=${NAMESPACE_NAME}`
  - `kubectl config view --minify | grep "namespace:"`
- Relation to DNS naming
  - Every K8s Service has a DNS entry of `${SERVICE_NAME}.${NAMESPACE_NAME}.svc.cluster.local` (and usually can just access via `${SERVICE_NAME}`)
  - No namespaces shouldn't collide with public TLDs.
- Resources
  - Within a namespace: pods, services, replication controllers
  - Not within a namespace: namespaces, nodes, persistentVolumes
- Since k8s 1.22, `kubernetes.io/metadata.name` label on all spaces is set to `${NAMESPACE_NAME}`

### K8s ConfigMaps

- Non-confidential info
  - Supplied by the Kubelet when it launches the container
    - Environment variables
      - `valueFrom:configMapKeyRef:{name,key}`
      - Only updated upon pod restart
    - Command-line args
    - Configuration files in a Volume
      - `volumes:configMap:items:`
  - Via an API call
- Must like in the same Namespace as the Pod
- Not available to static pods
- Locally cached
  - `configMapAndSecretChangeDetectionStrategy` -- Watch, TTL, proxy to to API server
  - May be marked as immutable (safer and more efficient)

### K8s Secrets

- Passwords, tokens, keys
- Stored within API server's `etcd`
- Best practices
  - Enable encryption-at-rest
  - RBAC least-privilege access
  - Restrict access to specific containers
  - Consider external secret providers
- Alternatives
  - Service account tokens to authenticate to cloud services
    - Can in turn present a 3rd-party secrets provider
  - CertificateSigningRequests
  - Device Plugin to access TPM
- Dot-files in a secret volume, e.g. `/etc/secret-volume/.secret-file`
- Types
  - | Type                                  | Usage                   | Notes                                        |
    | ------------------------------------- | ----------------------- | -------------------------------------------- |
    | `generic`                             | Arbitrary (opaque) data |                                              |
    | `kubernetes.io/service-account-token` | ServiceAccount          | Deprecated in favor of `TokenRequest`        |
    | `kubernetes.io/dockercfg`             | `~/.dockercfg           | Deprecated in favor of `.docker/config.json` |
    | `kubernetes.io/dockerconfigjson`      | `~/.docker/config.json  |                                              |
    | `kubernetes.io/basic-auth`            | basic authn             | Username and password                        |
    | `kubernetes.io/ssh-auth`              | ssh authn               | Don't forget `known_hosts` as well!          |
    | `kubernetes.io/tls`                   | TLS client or server    | Base64 PEM certs, keys, etc                  |
    | `bootstrap.kubernetes.io/token`       | Bootstrap token         |                                              |

### K8s Pods

- Pod deployed horizontally across nodes
  - Each container within a pod (app and sidecars, perhaps) has specified CPU/RAM requirements and limits (enforced by `kubelet`)
    - Limit enforcement is implementation-specific, but typically uses LInux `cgroups`
    - Resource usage available from Metrics API
    - Linux `hugepages` supported (but no over-committing)
    - Fractional CPU (cores), e.g. $$0.1 == 100m == "100 millicpu" == "100 millicores"$$
    - Ram: prefer `Ki`, `Mi`, `Gi`, `Ti`, `Pi`, `Ei`
  - `kube-scheduler` places pod instances on nodes

### K8s commands

- `kubectl get pods --field-selector status.phase=Running`

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

## Docker

- Dockerfile
  - Respects `./.dockerignore` 
    - Also `./docker/${DOCKER_FILE_NAME}.Dockerfile.dockerignore`
    - Negate with `!` 
    - Last line to match wins
  - Must begin with `FROM` instruction, except comments, parser directives, and global ARGs
    - `#` introduces comments
    - Typical directive is ``# escape=` ``
    - `FROM [--platform=<platform>] <image> [:<tag>|@<digest>] [AS <name>]`
      - Platform defaults to build platform, but `linux/amd64`, `linux/arm64`, `windows/amd64` are common.
      - If neither tag nor digest specified, then docker assumes `latest`; if tag or digest not found, build errors.
      - `[AS <name>]` to allow subsequent `FROM` and `COPY --from=<name>` instructions
    - Clears any state from previous `FROM` (e.g., when a single Dockerfile commits to multiple images)
  - `ARG` -- build-time arguments (visible to `docker history`), perhaps with default value
    - Specify with `--build-arg ${VAR}=${VALUE}`
    - Prefer `RUN --mount=type=secret` for secrets!
    - Predefined: `HTTP_PROXY`, `HTTPS_PROXY`, `FTP_PROXY`, `NO_PROXY`, and `ALL_PROXY` (plus all-lower-case)
      - Omitted from `docker history`
    - BuildKit backend provides `{TARGET,BUILD}{PLATFORM,OS,ARCH,VARIANT}` if corresponding `ARG` instruction
  - `ENV var=value` -- declares environment variables
    - Access via `$var` or `${var}`
      - `${var:-def}` -- Default value
      - `${var:+setVal}` -- If var is set then return setVal else empty string
    - Supported by `ADD`, `COPY`, `ENV`, `EXPOSE`, `FROM`, `LABEL`, `STOPSIGNAL`, `USER`, `VOLUME`, `WORKDIR`, `ONBUILD` (combined with above)
  - `CMD` -- at most one per Dockerfile (otherwise, last wins)
    - Styles
      - Exec style (preferred) -- `CMD ["pathToExecutable", "arg1", "arg2"]` (json syntax, so requires `"`)
        - Does not run within a shell, though `CMD ["sh", "-c", "echo $HOME"]` is of course possible
      - Default parameters to `ENTRYPOINT` -- `CMD ["arg1", "arg2"]`
      - Shell form -- `CMD command arg1 arg2`
        - Even `CMD echo "Hi Mom" | wc -`
    - Overridden by `docker run` arguments
  - `ENTRYPOINT` -- at most one per Dockerfile (otherwise, last wins)
    - Should be defined when using the container as an executable
    - `docker run <image>` command-line args are appended to exec-style ENTRYPOINT args, and are ignored for shell-style ENTRYPOINTs
    - Shell form means that SIGTERM kills a `/bin/sh -c` rather the container when a `docker stop` is invoked. (But see <https://community.linuxmint.com/software/view/gosu>)
  - `RUN` (shell form (`/bin/sh`) or exec form) -- executes in a new layer on top of the current image and commits results.
    - Fortunately, commits and layers are cheap
  - `RUN --mount` -- creates mounts accessible to the build
    - `RUN --mount=type=bind` -- Bind-mount read-only (by default, but written data will be discarded in any case) context directories
    - `RUN --mount=type=cache` -- Temp directories for caching compiler/package-manager data
    - `RUN --mount=type=tmpfs` -- Temp directories for caching compiler/package-manager data
    - `RUN --mount=type=secret` -- Access secure files without inclusion in image
    - `RUN --mount=type=ssh` -- Access ssh keys (via ssh agents), including passphrase support
    - Examples
      - `RUN --mount=type=cache,target=/var/cache/apt,sharing=locked --mount=type=cache,target=/var/lib/apt,sharing=locked apt update && apt-get --no-install-recommends install -y gcc`
      - `RUN --mount=type=secret,id=aws,target=/root/.aws/credentials aws s3 cp s3://... ...`
  - `RUN --network=<type>` -- `default`, `none` (only `/dev/lo`), `host` (protected by `network.host` entitlement)
  - `RUN --security=<type>` -- Not yet available
  - `EXPOSE` -- `EXPOSE 80` or `EXPOSE 80/tcp`, or even `EXPOSE 8000-8080`
    - `docker run -P` or `docker run --publish-all` automatically redirects to higher ports.
    - `docker network` allows communication amongst containers
  - `VOLUME` -- Creates a mount point for externally-mounted volumes
    - Will contain anything which is in the created image at the time of the the `VOLUME` instruction
  - `ENTRYPOINT`
  - `USER` -- Sets the default user for the remainder of the current stage (`RUN`, `ENTRYPOINT`, `CMD`)
    - `USER <user>[:group]` or `USER <uid>[:gid]`
    - `root` group when the specified user doesn't have a primary group.
  - `WORKDIR` -- Working dir from `RUN`, `CMD`, `ENTRYPOINT`, `COPY`, and `ADD`
    - Creates directory if necessary
    - Relative to current WORKDIR unless absolute
  - `LABEL <key1>=<val1> <key2>=<val2> <key3>=<val3>` -- Add metadata to image
    - Multiple `LABEL` instructions are fine
    - View with `docker image inspect --format='{{json .Config.Labels}}' myImage`

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
