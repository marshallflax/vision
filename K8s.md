# Kubernetes

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

## K8s objects

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

## K8s Namespaces

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

## K8s ConfigMaps

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

## K8s Secrets

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

## K8s Pods

- Pod deployed horizontally across nodes
  - Each container within a pod (app and sidecars, perhaps) has specified CPU/RAM requirements and limits (enforced by `kubelet`)
    - Limit enforcement is implementation-specific, but typically uses LInux `cgroups`
    - Resource usage available from Metrics API
    - Linux `hugepages` supported (but no over-committing)
    - Fractional CPU (cores), e.g. $$0.1 == 100m == "100 millicpu" == "100 millicores"$$
    - Ram: prefer `Ki`, `Mi`, `Gi`, `Ti`, `Pi`, `Ei`
  - `kube-scheduler` places pod instances on nodes

## K8s commands

- `kubectl run` -- starts one or more instances of an image
  - `kubectl expose` -- create a HA proxy for accessing instances from outside the cluster
- `kubectl create` -- creates a resource from JSON or YAML
  - Flags
    - `--allow-missing-template-keys` -- Only for `golang` and `jsonpath` output formats
    - `--dry-run` (`client` or `server`)
    - `--edit`
    - `--filename` (`-f`) -- filename, directory, or URL
    - `--kustomize` (`-k`) -- kustomization directory
    - `--output` (`-o`) -- `json`, `yaml`, `name`, `go-template`, `go-template-file`, `template`, `templatefile`, `jsonpath`, `jsonpath-as-json`, or `jsonpath-fil`e
    - `--raw`
    - `--record`
    - `--recursive` (`-R`)
    - `--save-config`
    - `--selector` (`-I`)
    - `--show-managed-fields`
    - `--template`
    - `--validate`
    - `--windows-line-endings` (only for `--edit=true`)
  - Resources
    - `kubectl create clusterrole NAME --verb=verb --resource=resource.group [--resource-name=resourcename] [--dry-run=server|client|none]`
      - `--non-resource-url`, `--resource`, `--resource-name`, `--verb`
    - `kubectl create clusterrolebinding NAME --clusterrole=NAME [--user=username] [--group=groupname] [--serviceaccount=namespace:serviceaccountname] [--dry-run=server|client|none]`
    - `kubectl create configmap NAME [--from-file=[key=]source] [--from-literal=key1=value1] [--dry-run=server|client|none]`
      - `--append-hash`, `--from-env-file`, `--from-file`, `--from-literal`
      - Packages one or more key/value pairs
    - `kubectl create cronjob NAME --image=image --schedule='1/5 * * * ?' -- [COMMAND] [args...]`
      - `--restart`
    - `kubectl create deployment NAME --image=image -- [COMMAND] [args...]`
      - `--replicas`, `--port`, `--validate`
    - `kubectl create ingress NAME --rule=host/path=service:port[,tls[=secret]]`
      - `--annotation`, `--class`, `--default-backend`
    - `kubectl create job NAME --image=image [--from=cronjob/name] -- [COMMAND] [args...]`
    - `kubectl create namespace NAME [--dry-run=server|client|none]`
    - `kubectl create poddisruptionbudget NAME --selector=SELECTOR --min-available=N [--dry-run=server|client|none]`
      - `--max-unavailable`
    - `kubectl create priorityclass NAME --value=VALUE --global-default=BOOL [--dry-run=server|client|none]`
      - `--preemption-policy`
    - `kubectl create quota NAME [--hard=key1=value1,key2=value2] [--scopes=Scope1,Scope2] [--dry-run=server|client|none]`
      - `--hard` (comma-delimited set of resource=quantity hard-limit pairs)
      - `--scopes` (all must match each tracked object)
    - `kubectl create role NAME --verb=verb --resource=resource.group/subresource [--resource-name=resourcename] [--dry-run=server|client|none]`
    - `kubectl create rolebinding NAME --clusterrole=NAME|--role=NAME [--user=username] [--group=groupname] [--serviceaccount=namespace:serviceaccountname] [--dry-run=server|client|none]`
    - `kubectl create secret docker-registry NAME --docker-username=user --docker-password=password --docker-email=email [--docker-server=string] [--from-file=[key=]source] [--dry-run=server|client|none]`
    - `kubectl create generic NAME [--type=string] [--from-file=[key=]source] [--from-literal=key1=value1] [--dry-run=server|client|none]`
    - `kubectl create secret tls NAME --cert=path/to/cert/file --key=path/to/key/file [--dry-run=server|client|none]`
    - `kubectl create service clusterip NAME [--tcp=<port>:<targetPort>] [--dry-run=server|client|none]`
    - `kubectl create service externalname NAME --external-name external.name [--dry-run=server|client|none]`
      - ExternalName service references to an external DNS address instead of only pods, which will allow application authors to reference services that exist off platform, on other clusters, or locally.
    - `kubectl create service loadbalancer NAME [--tcp=port:targetPort] [--dry-run=server|client|none]`
    - `kubectl create service nodeport NAME [--tcp=port:targetPort] [--dry-run=server|client|none]`
    - `kubectl create serviceaccount NAME [--dry-run=server|client|none]`
- `kubectl get [(-o|--output=)json|yaml|name|go-template|go-template-file|template|templatefile|jsonpath|jsonpath-as-json|jsonpath-file|custom-columns|custom-columns-file|wide] (TYPE[.VERSION][.GROUP] [NAME | -l label] | TYPE[.VERSION][.GROUP]/NAME ...) [flags]`
  - `--all-namespaces`, `--chunk-size`, `--show-kind`, `--show-labels`, `--show-managed-fields`, `--watch` (`-w`)
  - Complete list of resources: `kubectl api-resources`
- `kubectl run NAME --image=image [--env="key=value"] [--port=port] [--dry-run=server|client] [--overrides=inline-json] [--command] -- [COMMAND] [args...]`
  - `--attach`, `--expose`, `--grace-period`, `--restart`, `--wait`
  - `--cascade` -- "background" (default), "orphan", "foreground"
  - `--rm`
  - `--hostport`, `--privileged`
  - `--limits`, `--requests`
  - `--quiet`
  - `--tty` (`-t`)
- `kubectl expose (-f FILENAME | TYPE NAME) [--port=port] [--protocol=TCP|UDP|SCTP] [--target-port=number-or-name] [--name=name] [--external-ip=external-ip-of-service] [--type=type]`
  - TYPE: pod (po), service (svc), replicationcontroller (rc), deployment (deploy), replicaset (rs)
  - `--cluster-ip`
  - `--container-port` (or `--target-port`)
  - `--external-ip`
  - `--generator` (defaults to `service/v2`)
  - `--port` (defaults to the underlying resource)
  - `--session-affinity` (`None`, `ClientIP`)
  - `--type` -- `ClusterIP` (default), `NodePort`, `LoadBalancer`, `ExternalName`
- `kubectl delete ([-f FILENAME] | [-k DIRECTORY] | TYPE [(NAME | -l label | --all)])`
  - `--all`
  - `--all-namespaces` (`-A`)
  - `--cascade`, `--grace-period`, `--ignore-not-found`, `--now`, `--wait`

## K8s examples

- `kubectl get pods --field-selector status.phase=Running`
- `kubectl apply -f https://github.com/verrazzano/verrazzano/releases/download/v1.1.2/operator.yaml`
  - `kubectl -n verrazzano-install rollout status deployment/verrazzano-platform-operator`

## K8s certification

- <https://github.com/kodekloudhub/certified-kubernetes-administrator-course>
- Linux Foundation & Cloud Native Computing Foundation
  - <https://www.cncf.io/certification/cka/>
  - Hands-on, not multiple-choice (2 hours)
    - <https://github.com/cncf/curriculum>
    - <http://training.linuxfoundation.org/go//Important-Tips-CKA-CKAD>

### CKA Certification Overview

- Core Concepts
  - Cluster Architecture
    - Master Node (control plane)
      - ETCD cluster stores key/value pairs. <https://github.com/etc-io/etcd/releases/download>
        - Values are typically "documents"
        - Listens on `tcp/2379`
        - Standard client is `etcdctl` (nb, API v2 and API v3 are incompatible -- `export ETCDCTL_API=3`)
          - `etcdctl {put,get,version,mkdir,rmdir,mk,rm,cluster-health,snapshot save,backup,endpoint health}`
        - RAFT protocol
        - Stores: Nodes, PODs, Configs, Secrets, Accounts, Roles, Bindings, etc.
      - `kube-scheduler` -- capacity, node affinity requirements, etc
      - `kube-apiserver`
      - Controller Manager
        - Node Controller
        - Replication Controller
    - Worker Nodes (hosts applications as containers)
      - Container Runtime Engine -- Docker, containerD (originally part of Docker), Rocket
        - Implements Container Runtime Interface (CRI) -- Open Container Initiative (OCI)
          - `imagespec`, `runtimespec`
        - containerD options -- <https://kubernetes.io/docs/reference/tools/map-crictl-dockercli/>
          - Use it as part of Docker
          - `ctr` command-line interface (solely for debugging -- not very friendly) -- Only containerD
            - Examples
              - `ctr images pull docker.io/library/redis:alpine`
              - `ctr run docker.io/library/redis:alpine redis`
          - `nerdctl` -- preferred wrapper around `ctr` -- Only containerD
            - Docker-like CLI
            - Supports `docker compose`
            - Encrypted container images, lazy pulling, P2P image distribution, image signing, k8s namespaces
            - `nerdctl --namespace k8s.io ps -a`
          - `crictl` -- All CRI-compatible runtimes
            - Separately-installed, works across both containerD and rkt runtimes
            - Better for inspecting and debugging container runtimes than creating containers
            - Examples
              - `crictl pull busybox`
              - `crictl images`
              - `crictl ps -a`
              - `crictl exec -i -t ${IMAGE_SHA256} ls`
              - `crictl logs ${IMAGE_SHA256_PERHAPS_TRUNCATED}`
              - `crictl pods` (note that "pods" is a k8s concept but not a Docker concept)
            - Connects to containerD via unix sockets. Since k8s 1.24:
              - `unix:///run/containerd/containerd.sock`
              - `unix:///run/crio/crio.sock`
              - `unix:///var/run/cri-dockerd.sock`
              - Suggested: `export CONTAINER_RUNTIME_ENDPOINT`
      - `kubelet` -- starts, ends, monitors containers
      - `kube-proxy`
        - Uses `iptables` forwarding for service discover
        - Typically deployed as part of a DaemonSet
  - API Primitives
  - Services and Network Primitives
- Scheduling
- Logging and Monitoring
- Application Lifecycle
- Cluster Maintenance
- Security
- Storage
- Networking
- Installation, Configuration, Validation
- Troubleshooting

### Manual K8s setup

- Install `etcd` (`etcd.service`)
  - Only `kube-apiserver` talks to `etcd`
  - Typically port 2379 for API and 2380 internal
    - `--advertise-client-urls https://{$INTERNAL_IP}:2379`
  - HA environment
    - `--initial-cluster controller-0=https://${CONTROLLER_0}:2380 controller-1=https://${CONTROLLER_1}:2380`
  - Usual `/etc/etcd` (lots of certs) and `/var/lib/etcd`
  - Installed by `kubeadm`
    - `kubectl get pods -n kube-system`
    - `kubectl exec etcd-master -n kube-system etcdctl get / --prefix -keys-only` (within etc-master POD)
      - Registry contains: nodes (née minions), pods, replicasets, deployments, roles, secrets
    - Need to specify certs to `kubectl`, e.g.
      ```sh
      kubectl exec etcd-master -n kube-system -- sh -c \
        "ETCDCTL_API=3 etcdctl get / --prefix --keys-only --limit=10 --cacert /etc/kubernetes/pki/etcd/ca.crt --cert /etc/kubernetes/pki/etcd/server.crt  --key /etc/kubernetes/pki/etcd/server.key"
      ```

## kube-apiserver

- `kubectl` talks to `kube-apiserver`, which internally talks to etcd cluster
  - Alternatively, use API directly, e.g. `curl -X POST /api/v1/namespaces/default/pods ...`
- `kube-scheduler` monitors `kube-apiserver`

## Setup

- `kubeadmin` installation -- `kubeadmin-apiserver` as a pod within `kube-system` namespace on the master node
  - See `/etc/kubernetes/manifests/kube-apiserver.yaml`
- Non-`kubeadmin` install
  - `/etc/systemd/system/kube-apiserver.service`

## `kube-controller-manager`

- Controllers
  - Node-Controller
    - Executes `kubectl get nodes` every 5s
      - Node monitor grade period 40s
      - POD eviction timeout 5m
  - Replication-Controller
  - Deployment-Controller
  - Namespace-Controller
  - Endpoint-Controller
  - Job-Controller
  - Service-Account-Controller
  - PV-Protection-Controller (persistent volumes)
  - PV-Binder-Controller (persistent volumes)
  ```sh
  /usr/local/bin/kube-controller-manager \
    --controllers=* \
    --node-monitor-period=5s \
    --node-monitor-grace-period=40s \
    --pod-eviction-timeout=5m0s \
  ```
- `kubeadm` setup
  - `kubectl get pods -n kube-system` (on master node)
  - `/etc/kubernetes/manifests/kube-controller-manager.yaml`
- non-`kubeadm` setup
  - `/etc/systemd/system/kube-controller-manager.service`

## `kube-scheduler`

- Decides which pods should be on which nodes -- doesn't actually place them there.

## YAML

- `kubectl create -f pod-definition.yml`
  - `apiVersion:`
    | Kind | Version |
    | :--: | :-----: |
    | Pod | v1 |
    | Service | v1 |
    | ReplicaSet | apps/v1 |
    | Deployment | apps/v1 |
  - `kind:`
  - `metadata:`
    - `name:`
    - `labels:`
  - `spec:`
    - `containers:` (Pod)
      - `- name:`
      - `  image:`
