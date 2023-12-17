# Jakarta EE

## CDI Lite vs CDI Full

- Session and conversation scope, `@Interceptors` (though `@Intercept` is fine), bean discovery "all" mode, `@AroundInvoke` on entire classes, decorators, specialization, passivation (i.e. serialization)

## Core CDI

```java
module jakarta.cdi {
  exports jakarta.decorator; exports jakarta.enterprise.context; exports jakarta.enterprise.context.control; exports jakarta.enterprise.context.spi; exports jakarta.enterprise.event; exports jakarta.enterprise.inject; exports jakarta.enterprise.inject.build.compatible.spi; exports jakarta.enterprise.inject.literal; exports jakarta.enterprise.inject.se; exports jakarta.enterprise.inject.spi; exports jakarta.enterprise.inject.spi.configurator; exports jakarta.enterprise.util; requires transitive jakarta.annotation; requires transitive jakarta.interceptor; requires transitive jakarta.cdi.lang.model; requires transitive jakarta.inject; requires static jakarta.el;
  requires static java.naming;
  uses jakarta.enterprise.inject.se.SeContainerInitializer; uses jakarta.enterprise.inject.spi.CDIProvider; uses jakarta.enterprise.inject.build.compatible.spi.BuildServices;
}

module jakarta.cdi.lang.model {
  exports jakarta.enterprise.lang.model; exports jakarta.enterprise.lang.model.declarations; exports jakarta.enterprise.lang.model.types;
}
```

- Beans define contextual objects (created by the container) for state and/or behavior
  - One scope
  - One implementation
  - One or more _bean types_
  - One or more _qualifiers_
  - Zero or one _names_
  - Zero or more _interceptor bindings_

## Jakarta EE integration testing

- <https://www.youtube.com/watch?v=mV4BTfBi8XQ>
- Frameworks
  - Weld JUnit
  - Arquillian (inactive)
  - MicroShed Testing (Abandoned)
  - Atbash
    - Testcontainers
    - WireMock!
    - Database containers and DBUnit

## JAX-RS

- `@ApplicationPath("/api")` annotation on a subclass of `Application`
- `@Path("/notifications")` annotation on a class containing endpoints
- `@GET @Path("/ping")` annotation on a method
  - `return Response.ok().entity("I'm doing fine").build()`
- `@GET @Path("/get/{id}") @Produces(MediaType.APPLICATION_JSON) public Response getNotification(@PathParam("id") int id) { return Response.ok() .entity(new Notification(id, "john", "test notification")) .build(); }`
- `@POST @Path("/post/") @Consumes(MediaType.APPLICATION_JSON) @Produces(MediaType.APPLICATION_JSON) public Response postNotification(Notification notification) { return Response.status(201).entity(notification).build(); }`

## Helidon 4.0.0

- Jakarta EE 10 (Core Profile)
  - Context
    - <https://vived.substack.com/p/will-jakarta-ee-compete-with-microprofile?sd=pf>
    - <https://whichjdk.com/>
  - Bean Validation -- 3.0 -- `jakarta.validation`
    - Implemented by Hibernate Validator
    - Domain entity
      - `import jakarta.validation.constraints.*`
      - `@NotNull(message = "Name cannot be null") private String name;`
      - `@AssertTrue private boolean flag;`
      - `@Min(value=18, message="Adults only") private int age;`
      - `@NotEmpty`, `@NotBlank`
      - `@Positive`, `@PositiveOrZero`, `@Negative`, `@NegativeOrZero`
      - `@Past`, `@PastOrPresent`, `@Future`, `@FutureOrPresent`
      - `Optional<@Past LocalDate> getDateOfBirth();`
      - `List<@NotBlank String> preferences;`
    - `@PostMapping("/users") ResponseEntity<String> addUser(@Valid @RequestBody User user){}`
    - `Set<ConstraintViolation<User>> violations = Validation.buildDefaultValidatorFactory().getValidator().validate(user);`
      - `@ExceptionHandler(MethodArgumentNotValidException.class) ...`
  - CDI -- 4.0.1 -- Dependency Injection for Java -- JSR 330
    - `@Inject`, `@Named`, 
    - Scopes
      - `@ApplicationScoped` (proxied), `@Singleton` (pseudoscope)
      - `@RequestScoped`
      - `@SessionScoped`
      - `@Dependent` (default)
    - vs SpringBoot
      - `@AutoWired` --> `@jakarta.inject.Inject`
        - `@ConfigProperty(name="com.test.something", defaultValue="1")`
      - `@Component` --> `@jakarta.annotation.ManagedBean` or `@Named`
      - `@Scope("singleton")` --> `@Singleton`
      - `@Qualifier` --> `@Qualifier` or `@Named`
    - Cross-cutting concerns
      - We could create a "`@Timed`" annotation as an `@InterceptorBinding public @interface`
      - <https://www.baeldung.com/cdi-interceptor-vs-spring-aspectj>
    - Events
      - `@Inject private jakarta.enterprise.event.Event<T> addTEvent;`
  - JSON-P -- 2.1.1 (`Json.createArrayBuilder().add(...)....build()`)
  - JSON-B -- 3.0 (`.toJson()`, `.fromJson()`)
  - JAX-RS -- 3.1.0 (Jakarta RESTful Web Services)
    - `@Consumes`, `@Produces("application/json")`, `@Path("/hello")`
    - `@PathParam`, `@QueryParam`, `@DefaultValue`, `@HeaderParam`, `@MatrixParam`, `@FormParam`, `@Context`
    - `@Provider`, (Jakarta RESTful Web Services)
    - `@GET`, `@POST`, `@PUT`, `@DELETE`, `@HEAD`
  - JPA -- 3.1.0
  - JTA -- 2.0
    - `@Transactional(Transactional.TxType.SUPPORTS)` (class)
      - `MANDATORY`, `REQUIRES_NEW`, `SUPPORTS`, `NOT_SUPPORTED`, `NEVER`, `REQUIRED` (default) 
    - `@Transactional(rollbackOn = IllegalArgumentException.class, dontRollbackOn = EntityExistsException.class)` (method)
    - Distributed transactions
      - 2PC (Two-Phase Commit) 
      - Saga pattern
        - `Long-Running-Action` HTTP header
        - `409 Conflict`
        - Compensating transactions must be idempotent and retryable
        - Choreography frameworks
          - Axon Saga
          - Eclipse MicroProfile LRA (Long Running Actions)
          - Eventuate Tram Saga
          - Seata
        - Orchestration Pattern
          - Camunda (CPMN)
          - Apache Camel
  - WebSocket -- 2.1.0 
- MicroProfile 6.0
  - Config -- 3.0.3 -- `config_ordinal`
    - `final String url = ConfigProvider.getConfig().getValue("acme.something.url", String.class);`
    - `@Inject @ConfigProperty(name="acme.something.url") private String url;`
    - `@Inject @ConfigProperty(name="acme.something.delay", defaultValue="10") private Supplier<Long> delay;`
  - Fault Tolerance -- 4.0.2
    - Interceptors: (All respect `@Priority()`, of course)
      - `@Timeout`
      - `@Retry`
      - `@Fallback`
      - `@CircuitBreaker`
      - `@Bulkhead`
      - `@Asynchronous` (must return `java.util.concurrent.{Future,CompletionStage}`)
    - (Automatic metrics when MP Metrics enabled)
  - GraphQL -- 2.0
  - Health -- 4.0 -- `/health/started`, `/health/ready`, `/health/live`
  - JWT Auth -- 2.1
  - LRA (Long Running Actions) -- 2.0
  - Metrics -- 5.0.1 -- Prometheus-formatted (textual OpenMetrics) `/micrometer` endpoints
    - `@Timed`, `@Counted`
    - Gauges and Counters; histograms
  - Open API -- 3.1
    - Previously "Swagger", now <https://www.openapis.org/> (Linux Foundation)
    - Language-agnostic; declarative specification <https://learn.openapis.org/>
    - OpenAPI Description, e.g. openapi.yaml
  - Open Telemetry 1.0
  - OpenTracing -- 3.0
  - Reactive Messaging - 3.0
    - Beans are `@ApplicationScoped` or `@Dependent`
  - Reactive Streams Operators -- 3.1.1
  - RESTful Web Services Client -- 3.0
    - `jakarta.ws.rs.client.ClientBuilder.newClient()....`

- CORS
- gRPC
- OCI SDK (Oracle Cloud Infrastructure)
- Scheduling
- Security