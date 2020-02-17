# `Microservices with spring boot and spring cloud`

---

## `Introduction`

---

## `Spring Boot`
* Convention over configuration
* Fat jars
* `@SpringBootApplication` and `@ComponentScan`
* `@Component` and `@Autowired`
* `@Configuration` and `@Bean`

---

## `Spring WebFlux`
* `WebClient`
* Servlet v3.1+
* non-servlet based embeded web servers: Netty
* properties file

---

## `SpringFox`
* Document APIs
* Swagger
* Inspect WebFux and Swagger-based annotations

---

## `Spring Data`
* Relational DB, NoSQL database, key-value database, graph database
* Entities and repositories
* `CrudRepository`, `ReactiveCrudRepository`
* `Mono`, `Flux` objects
* Reactive repository not for JPA

---

## `Spring Cloud Stream`
* Streaming over messaging, publish-and-subscribe
* Apache Kafka, RabbitMQ
* Message, publisher, subscriber, channel and binder
* consumer groups, partitioning, persistence, durability, error handling

---

## `Docker`
* Container and VM
* Linux namespaces and control groups (cgroups)
* Dockerfile
* Docker Compose

---

## `Test script`
* Start up the microservices
* Run tests
* Tear down the microservices

---

## `Prerequisites`
* git: from https://git-scm.com/downloads
* java: from https://git-scm.com/downloads
* curl
* jq: from https://git-scm.com/downloads
* spring CLI: from https://docs.spring.io/spring-boot/docs/current/reference/html/getting-started-installing-spring-boot.html#getting-started-installing-the-cli

---

## `Microservices`
* Product: port 7001
  - product id, name, weight
* Review: port 7002
  - product id, review id, author, subject, content
* Recommendation: port 7003
  - product id, recommendation id, author, rate, content
* Product Composite: port 7000

---

## `Spring Boot Project`
* Gradle
* Java 8
* Actuator and WebFlux modules
* Spring Boot v2.1.0 RC1 and Spring Framework v5.1.1

---

## `Multiple-project builds in Gradle`
* `settings.gradle`
* `./gradlew build`

---

## `API and util project`
* Use Java interfaces to describe RESTfule API
* Use model classes to describe its requests and responses
* Helper classes: ex. `ServiceUtil`
* No construction of a fat jar: build.gradle

---

## `Implement APIs`
* Implement APIs by overriding
* Runtime properties: port and logging level

---

## `Implement Composite Microservices`
* Two parts: Integration component and Composite Service implementation
* Runtime properties for the address info of the core microservices
* Integration component uses `RestTemplate` and `ObjectMapper`

---

## `Composite service implementation`
* Uses `ServiceUtil` and the integration component

---

## `Error handling`
* Protocol-specific error handling (HTTP status code) and business logic
* Map Java exception to HTTP response
* Map HTTP status code to Java exception

---

## `Testing APIs`
* curl
* Spring WebFlux `WebTestClient` and Mockito
* Test script

---

## `Docker`
* Container vs. virtual machine
* Isolation
* cgroup

---

## `Running Java in Docker`

---

## `Running microservices using Docker`
* Dockerfile
* Spring profiles specify environment-specific configuration
* Detached

---

## `Docker Compose`
* Manage the whole system of microservices
* Port mapping to expose outside of the container
* Test script to start up the mciroservices, run all tests, tear down microservices
* Restart a failed microservice:
  `docker-compose up -d --scale product=0`
  `docker-compose up -d --scale product=1`
* Combination:
  `./gradlew clean build && docker-compose build && ./test-all.sh start stop`

---

## `OpenAPI and Swagger`
* Documenting RESTful services
* Using SpringFox

---

## `SpringFox`
* Create Swagger-based API documentation for microservices
* Add the API documentation to the Java interfaces, not the implementation classes
* Parts of documentation in property files

---

## `Persistence`
* Protocol layer, service layer, integration layer and persistence layer

---

## `Persistence layer`
* MapStruct tranforms Spring Data entity objects and API model classes
-- generates the implmentation of the bean mappings at compile time by processing MapStruct annotations
* `id` and `version` fields in entity and model classes
* `version` field implements optimistic locking to verify the updates of an entity in the database do not overwrite a concurrent update
* Respositories and extra query methods

---

## `Test persistence`
* `@DataMongoTest` and `@DataJpaTest`
* `Transactional(propagation = NOT_SUPPORTED)`
* `DuplicateKeyException` and `optimisticLockingFailureException`
* Paging and sorting

---

## `Service layer`
* Inject the repository class from the persistence layer
* Inject Java bean mapper (interface)
* Idempotent delete operation
* Tests

---

## `Intergation layer`
* One of the uderlying microservices temporarily is not available results in partly created or deleted composite products

---

## `Define containers for databases`
* Container health checks
*   `depends_on`
* Database connection configuration: `application.yml`
* Mongo DB and MySQL CLI tools

---

## `Reactive microservices`
* Non-blocking synchronous REST APIs and asynchronous event-driven services
* Elasticity and resilience
* Non-blocking synchronous:
--  Read operations, mobile apps or SPA web apps consuming synchronous APIs, accross organizations with no common messaging systems
--  composite service APIs, read services of the core microservices
* Asynchronous event-driven services:
--  create and delete services of the core microservices

---

## `Non-blocking synchronous REST APIs`
* Spring Reactor
* Non-blocking persistence
* Non-blocking REST APIs

---

## `Spring Reactor`
* Flux and Mono
* Define how the stream will be processed, and an infrastructure component (ex. WebFlux) initiate the procssing
* `onComplete`

---

## `Non-blocking persistence for MongoDB`
* JPA remains blocking
* Use `block()` or `StepVerifier` in tests

---

## `Non-blocking REST APIs`
* Use fluent APIs of Spring Reactor
* Use `block()` or `StepVerifier` in tests

---

## `Run blocking code using Scheduler for JPA`
* Run the blocking code on a thread from a thread pool
* Avoid affecting the non-blocking processing

---

## `Non-blocking APIs in the composite`
* The integration layer uses a non-blocking HTTP client (`WebClient`)
* The service implementation calls the core services in parallel and non-blocking
* `onErrorMap()` and `onErrorResume()`
* `zip()` handles a number of parallel requests and zips them together once requests are complete

---

## `Event-driven asynchronous services`
* Event-driven create and delete services
* Publish events in the composite, consume events in the core services

---

## `Spring Cloud Stream`
* Consumer groups
* Retries and dead-letter queues
* Guarantee orders and partitions
* RabbitMQ, Kafka

---

## `Consumer groups`
* Allow one instance per consumer to process each message

---

## `Retries and dead-letter queues`
* Specify a number of retries until a failing message is removed to another storage (dead-ldetter queue) for fault analysis and correction
* Increasing time bewteen each retry

---

## `Guarantee order and partitions`
* Quarantee orders without losing performance and scalability
* Required only for messages that affect the `same` business entities
* The message system places messages with the same key in a specific partitions (sub-topics)
* The delivery order for messages in one partition is quaranteed.
* One coonsumer instance per partition within a consumer group

---

## `Topics and events`
* An event is a message that describes something that has happened

---

## `Message source and event publishing`
* One `MessageChannel` per topic in an Java interface
* The integration layer publishes messages to `MessageChannel`
* Configure the message system to publish events: content type, output channel/topic binding, connectivity for Kafka and RabbitMQ

---

## `Tests`
* `TestSupportBinder` verifies what messages have been sent without using ant messaging system
* `MessageQueueMatcher` supports utitity methods for the verification of messages in a queue

---

## `Consuming events`
* `SubscribableChannel` binding allows for consuming messages that have been published to a topic
* Each message processor only listens to one topic using `Sink` interface
* `StreamListener` annotation specifies what channel to listen to
* `MessageProcessor` bases on a blocking programming model, need to use `block()` method on `Mono` object for triggering error handling and movement of messages to the dead-letter queue
* Configuration similar to publishers

---

## `Tests`
* Use RabbitMQ without partitions
* Use RabbitMQ with two partions per topic
* Use Kafka with two partitions per topics
* Saving events with `auditGroup` consumer group per topic
* Health API assures that databases and messaging systems are ready to proecss requests and messages

---

## `Health API`
* Use Actuator to extends composite service (integration layer) to include the health of three core services
* Health endpoint configuration

---

## `Spring Cloud`
* Netflix Eureka, discovery server
* Spring Cloud load balancer, client-side load balancer
* Spring Cloud Gateway, edge server
* Resilience4j, circuit breaker
* Spring Cloud Config, Configuration server
* Integrate Spring Security with OAuth 2.0
* Spring Cloud Sleuth and Zipkin, distributed tracing

---

## `Service discovery`
* DNS client caches the resolved IP addresses for the first working IP address it tries out
* Moving parts: new instances, failing instances, recovering instances, instances start-up and network related errors

---

## `Netflix Eureka`
* Client-side service discovery: the clients talk to Netflix Eureka to get info about microservice instances
* Service registration and heartbeat
* `DiscoveryClient` allows a client to interact the discovery service and allows a microservice to register with the discovery service
* `LoadBalancerClient` allows a client to make requests through a load balancer to registered instances

---

## `Netflix Eureka server`

---

## `Connect microservices to Netflix Eureka server`
* Virtual hostnames: `spring.application.name`
* Disable the use of Netflix Eureka in all Spring Boot tests
* Load balancer-aware `WebClient` builder

---

## `Configuration in development`
*  Start default configurations in production and minimize wait time in development
