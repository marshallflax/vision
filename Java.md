# Java notes

## Versions

- 7
  - Strings in `switch`
  - try-with-resources
  - Diamond operator type inference
  - Simpler varargs, e.g. `Strings... args`
- 8 LTS
  - Lambda
  - `java.time.`{LocalDate,temporal.ChronoUnit,temporal.TemporalAdjusters}`
- 9
  - Modules (Project Jigsaw)
    - `module-info.java`
    - `module com.foo.bar {requires com.foo.baz; exports com.foo.bar.alpha; exports com.foo.bar.beta;}`
  - Reactive Streams
- 10
  - `var` (local-type inference)
- 11 LTS
  - Remove Java EE and CORBA
  - `java.net.http` (client)
- 12
  - JEP 328 -- Flight Recorder
  - Preview: `switch` expression syntax (`->`, and perhaps `yield`)
- 13
  - Preview: Text blocks `"""`
- 14
  - Preview: Records
  - Preview: Pattern Matching `instanceof`
- 15
  - Preview: Sealed Classes (`public abstract sealed class Shape permits Circle, Rectangle, Square {...}`)
    - A "sum type"
  - Hidden classes (in lieu of `sun.misc.Unsafe::defineAnonymousClass`)
- 16
  - Preview: Vector API
  - Unix Domain Socket Channels
- 17 LTS
  - Preview: `switch` pattern-matching
  - Preview: Vector API
- 18
  - UTF-8 by default
  - `jwebserver`
  - `@snippet`
- 19
  - Preview: record patterns (`if (o instanceof Rect(int x, int y, int w, int h)) {return w*h;}`)
  - Preview: virtual threads
  - Preview: vector API
- 20
  - Preview: Scoped Values
- 21 LTS
  - Preview: String Templates
  - Sequenced Collections -- `interface SequencedCollection<E> extends Collection<E> { SequencedCollection<E> reversed(); void addFirst(E); void addLast(E); E getFirst(); E getLast(); E removeFirst(); E removeLast(); }`
  - Record Patterns
  - `switch` pattern matching
  - Virtual Threads
  - Preview: unnamed patterns, variables, classes, main methods
  - Preview: scoped values
  - Preview: Vector API
  - Preview: Structured Concurrency

## Concurrency

- `Future<V>`
  | Type | Method |
  | ---- | ------ |
  | boolean | cancel(boolean mayInterruptIfRunning) |
  | V | get() |
  | V | get(long timeout, TimeUnit unit) |
  | boolean | isCancelled() |
  | boolean | isDone() |
- `Callable<V>` (`@FunctionalInterface`)
  | Type | Method |
  | ---- | ------ |
  | V throws Exception | call() |
- `CompletionStage`

## Reactive Programming

- Principles
  - Responsive -- consistent low latency
  - Elastic
  - Resilient
  - Message-driven
- Reactive Streams
  - `subscribeOn()`, `publishOn()`, `observeOn()`
