# Jakarta EE

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
