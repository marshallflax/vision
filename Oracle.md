# Oracle

## Oracle Fusion Middleware (14c -- 14.1.1)

- Oracle WebLogic Server
- Oracle Coherence

## Oracle WebLogic Server

- Versions
  - 10gR3 -- 2008-08
  - 11gR1 -- 2009-07
  - 12cR1 -- 2011-12
  - 12cR2 -- 2015-10
  - 14cR1 -- 2020-03
    - Java EE 8
      - Java API for JSON Binding (JSON-B) -- <https://javaee.github.io/jsonb-spec/>
      - Java API for JSON Processing (JSON-P) -- <https://javaee.github.io/jsonp/> -- Streaming and object APIs
      - Java API for REST 2.1 (JAX-RS) -- <https://jakarta.ee/specifications/restful-ws/>
        - CORS
      - JavaServer Faces
      - Java Servlet 4.0 -- JSR 369 -- HTTP/2, server push, HTTP trailer, mapping discover
        - HTTP/2 and TLSv1.3
      - Bean Validation 2.0 -- JSR 380
      - CDI 2.0
        - Dependency Injection for Java -- JSR 330
          - `@AutoWired` --> `@jakarta.inject.Inject`
          - `@Component` --> `@jakarta.annotation.ManagedBean` or `@Named`
          - `@Scope("singleton")` --> `@Singleton`
          - `@Qualifier` --> `@Qualifier` or `@Named`
        - Managed Beans -- part of JSR 366
        - Interceptors -- part of JSR 345
      - Java EE Security API -- JSR 375
        - `HttpAuthenticationMechanism`, `IdentityStore`, etc.
      - PKCS12
      - JTA network channels (Java Transaction API)
      - EBR (Edition-Based Redefinition) -- <https://www.oracle.com/docs/tech/ebr-technical-deep-dive-overview.pdf>
        - "Editioning Views" allow new columns to be invisible to prior software version; idempotent cross-edition triggers propagate data changes to new columns/tables.
    - Java SE 8 LTS (2014-03) and 11 LTS (2018-09). Also GraalVM Enterprise Edition
      - 17 LTS -- not until 14.1.2 (2024)
    - K8s and Docker management tools
      - WebLogic Image Tool
      - WebLogic Server Kubernetes Operator
      - WebLogic Monitoring Exporter (Prometheus-compatible)
      - WebLogic Logging Exporter
    - WDT (WebLogic Deploy Tooling) -- YAML -- <https://github.com/oracle/weblogic-deploy-tooling>
  - 14cR2 -- 2024???
    Jakarta EE 8
    - Java SE 17 LTS
- Part of Oracle Fusion Middleware
  - Supports JDBC databases -- Oracle, DB2, SQLServer, MySQL Enterprise, etc
  - Components
    - Enterprise Grid Messaging
    - HotSpot JVM
    - Oracle Coherence -- in-memory cache across multiple servers
      - Key/Value pairs
      - Automatic sharding
      - JCache, JPA, REST
      - Migrate to Redis?
    - Oracle TopLink
    - Oracle WebLogic Server Web Services
    - Tuxedo (X/Open XA two-phase commit)
  - Standards
    - BPEL and BPEL-J (Business Process Execution Language -- XML SOA definition)
    - ebXML (e-business XML)
    - JAAS (Java Authentication and Authorization Service)
    - Java EE
    - JPA
    - JMX and SNMP
    - SOAP
    - UDDI (Universal Description, Discovery, and Integration) -- Dead
    - WSDL (Web Services Description Language)
    - WSRP (Web Services for Remote Portlets) -- for web portals
    - WS-Security
    - XSLT and XQuery
  - Integration
    - .NET interoperability
    - CORBA
    - COM+
    - WebSphere MQ
    - Java EE Connector Architecture
    - Native JMS messaging (Java Message Service or Jakarta Messaging API)
      - Point-to-point -- each message consumed by exactly one consumer. Messages persist until expiration or consumption.
      - Publish/subscribe -- by default non-durable
      - Providers identified by JNDI (Java Naming and Directory Interface) -- specify `Context.INITIAL_CONTEXT_FACTORY` and `Context.PROVIDER_URL`
        - AWS SQS, Apache Active MQ, Apache Qpid, MQSeries, WebSphere SIBus (Service Integration Bus), JBoss Messaging and HornetQ, Oracle OMS, OpenJMS, Oracle AQ, Solace PubSub+, RabbitMQ, TIBCO Cloud Messaging, TIBCO EMS
    - Tuxedo Connector

## Oracle Coherence

- (Originally Tangosol Coherence)
- <https://docs.oracle.com/en/middleware/standalone/coherence/14.1.1.0/develop-applications/developing-applications-oracle-coherence.pdf>
- Basic Concepts
  - Multicast to `224.0.0.0/4`
  - Clustered Data Management
    - Fully-coherent single-system-image (SSI)
    - Storage and processing scalability, transparent failover and failback, redundancy, cluster-wide locking/transactions
    - Database caching, HTTP session management, distributed queries, grid agent invocation
  - Logical Layer API; Physical layer XML configuration
    - Topology choice can be deferred until production deployment
  - Caching Strategies
    - Local Cache
      - On heap within the local JVM
      - By default Replicated, Optimistic, and Near Cache also use this
    - Distributed Cache
      - Dynamic and automatic partitioning
      - Incremental data shifting
    - Near Cache
      - Multiple tradeoffs available between performance and synchronization
    - Replicated Cache
      - Small read-heavy traffic
    - View Cache
      - Small read-heavy traffic
    - Optimistic?
  - Data Storage (non-persistent, but backing maps available via CacheLoader/CacheStore)
    - On-heap (but beware GC times)
    - Journaled (optimized for solid-state drives)
      - Requires serialization
    - File-based
      - Berkeley Database JE
  - Serialization
    - POF (Portable Object Format) -- recommended
    - `java.io.Serializable` -- simple but slow
    - `java.io.Externalizable` -- faster but manual configuration ... also consider `com.tangosol.io.ExternalizableLite` and `com.tangosol.run.xml.XmlBean`
  - Configuration and extension
    - API and XML may be mixed
  - Namespace
    - One Coherence instance per JVM; multiple instances per cluster.
    - Single instance may contain multiple named caches (e.g., multiple named tables)
- API
  - `NamedCache cache = CacheFactory.getCache("myCache");` -- `coherence-cache-config.xml`
    - `java.util.Map`
    - `com.tangosol.net.cache.Cache` -- supports expiration
    - `com.tangosol.util.QueryMap`
    - `com.tangosol.util.InvocableMap` -- supports server-side processing
      - `public void execute(Invocable task, Set setMembers, InvocationObserver observer)`
      - `public Map query(Invocable task, Set setMembers)`
    - `com.tangosol.util.ObservableMap` -- map events
      - Live Events
      - Map Events
    - `com.tangosol.util.ConcurrentMap` -- `lock()` and `unlock()`, etc
  - Uses
    - "side" cache -- no persistent backing
    - "inline" cache -- decouples access to external data source (database or service)
      - "write-through"
      - "write-behind"
  - Transactions
    - Basic concurrency with `ConcurrentMap` and `EntryProcessor`
    - Partition-level transactions using `EntryProcessor`
  - HTTP Session management ("Coherence\*Web")
  - ORM Integration (L2 cache)
- Management via JMX

## JSP

- WAR -- `jar -cvf hello.war *`
  - `/WEB-INF/classes/{example-config.xml,tangosol-coherence-override.xml}`
  - `/WEB-INF/lib/coherence.jar`
  - `/WEB-INF/web.xml` -- `<?xml version='1.0' ?><web-app/>`
  - `/hello.jsp`
    - `<%@ page language=java import="a,b,c" %>`
    - `<% out.println("Hello world"); %>`
- EAR
  - `/META-INF/application.xml`
    - `module` (`ejb`, `web`, `java`)

## Helidon

- <https://helidon.io/> (Oracle)
  - <https://www.oracle.com/a/ocom/docs/technical-brief--helidon-report.pdf>
  - <https://www.youtube.com/watch?v=gd00cu4Bw1I>
- Helidon SE ("Helidon Reactive")
  - Originally on top of netty, Helidon 4.0 is virtual-thread-based (Java SE 21) web server
  - Reactive streams
- Helidon MP ("Helidon MicroProfile")
  - CDI
  - JAX-RS, JSON-B, JSON-P
  - GraphQL, CORS, gRPC
  - Integrates with OpenTelemetry, Prometheus, Jaeger/Zipkin, K8s
  - GraalVM, jlink, .jar
  - Integrates with WebLogic -- <https://blogs.oracle.com/weblogicserver/post/announcing-weblogic-helidon-integration>
    - Single-Sign-on MicroTx transactions
    - Transactions with MicroTx (WebLogic Server 14.1.1 and Helidon MP 2.6.2)
- Helidon 4.0.0
  - Java SE 21, Maven 3.8+, Docker 18.09+, Kubectl 1.16.5+

## MicroTx (Transaction Manager)

## Verrazzano (K8s-based container platform)

- Multi-cloud and hybrid
- Support for WebLogic, Coherence, Helidon
- Monitoring, integrated security, DevOps and GitOps

## Installation

- `choco install openjdk` # 21.0.1, `C:\Program Files\OpenJDK`
- `choco install graalvm-java21` # 21.0.0, `C:\Program Files\GraalVM`
- ~~`choco install kubernetes-cli` # 1.28.2, `C:\ProgramData\chocolatey\lib\kubernetes-cli\tools\kubectl.exe`~~
- ~~`choco install minikube` # 1.32.0, `C:\ProgramData\chocolatey\lib\MiniKube`~~
- ~~`choco install docker-cli` # 24.0.5~~
- ~~`choco install docker-desktop` # v4.25.1~~
- (WSL) `sudo snap install kubectl --classic`
- (WSL) `minikube.exe start`
- (WSL) `ln -s "/mnt/c/users/mgflax/.kube" ~/.kube`
- WSL
  - `sudo apt update && sudo apt upgrade && sudo apt install containerd`
  - `wsl --shutdown` (from PS)
  - `curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && chmod +x ./minikube && sudo mv ./minikube /usr/local/bin`
  - Enable hyperv
  - `minikube config set driver hyperv`
- PowerShell
  - `choco install minikube` # 1.32.1, `C:\ProgramData\chocolatey\lib\MiniKube`
- Git bash
  - `kubectl completion bash > ~/bash_completion.d/kubectl && echo "source ~/bash_completion.d/kubectl" >> ~/.bashrc`
  - `minikube completion bash > ~/bash_completion.d/minikube && echo "source ~/bash_completion.d/kubectl" >> ~/.bashrc`
