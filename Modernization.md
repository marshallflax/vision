# So, you want to modernize from Java 8

## Some Slogans

> Defer architectural changes

- Two competing standards
  - Spring Boot annotations are wonderful if you need the ability to pull in a large variety of frameworks.
  - Jakarta EE annotations (especially the MicroProfile subset) allow a large variety of runtimes, including Helidon.
- The fewer classes which require either, the better. POJOs are easier to test and use.

> Microservices are for separate projects

- Microservices increase latency but increase flexibility.
- Microservices should be sufficiently decoupled so that each may make its own choice as to DCI framework and database technology to use. Multiple microservices should never talk to the same database schema.

> Push testing leftwards

- Unit tests > Component tests > Integration tests > API tests > GUI tests

> Distinguish Strong and Eventual Consistency

- Leave state to the experts
  - Redis
    - ACID -- Atomic, Consistent, Isolated, Durable
    - BASE -- Basically Available, Soft state, Eventually-consistent
    - CAP -- Consistency, Availability, Partition-tolerance
  - Pub/Sub
  - Cloud Spanner
  - Databases