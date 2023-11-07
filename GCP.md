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
    | `kubernetes.io/tls`                   | TLS client or server    | Base64 PEM certs, keys, etc                      |
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
- <https://tekton.dev/>
- <https://knative.dev/docs/concepts/>
- <https://cloud.google.com/memorystore/docs/redis>
- Parquet vs Avro vs Orc
  |   | Avro | Orc | Parquet |
  | - | ---- | --- | ------- |

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
