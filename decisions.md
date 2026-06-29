# Java Project — Decisions & Inputs Template

This document is a comprehensive template for capturing the full architectural, operational, and dependency profile of any Java project. Fill in the **Current Values** column from the codebase or user input. Use the **Planned Actions / Target State** column to track migration plans, upgrades, or refactoring targets.

---

## 1. Basic Stack

| # | Input / Component | Current Values | Planned Actions / Target State |
| :--- | :--- | :--- | :--- |
| 1.1 | **Java JDK Version** (8, 11, 17, 21, 25) | | |
| 1.2 | **Java Vendor** (Oracle, OpenJDK, Corretto, Azul Zulu, GraalVM, Temurin) | | |
| 1.3 | **Language Level / Source Compatibility** (e.g., `sourceCompatibility = '17'`) | | |
| 1.4 | **Framework** (Spring Boot, Spring MVC, Jakarta EE, Quarkus, Micronaut, Vert.x, Dropwizard, Play, None) | | |
| 1.5 | **Framework Version** (e.g., Spring Boot 3.2.5) | | |
| 1.6 | **Build Tool** (Maven, Gradle, Ant, Bazel, SBT) | | |
| 1.7 | **Build Tool Version** (e.g., Maven 3.9.6, Gradle 8.7) | | |
| 1.8 | **Build Wrapper** (mvnw / gradlew present? Version?) | | |
| 1.9 | **Packaging Type** (JAR, WAR, EAR, Native Image) | | |
| 1.10 | **App Runtime / Server** (Embedded Tomcat, Embedded Jetty, Embedded Undertow, External Tomcat, WildFly, WebLogic, WebSphere, Liberty, Netty) | | |
| 1.11 | **Module Structure** (Single Module, Multi-Module, Monolith, Microservices, Monorepo) | | |
| 1.12 | **JPMS (Java Modules)** (Yes — has `module-info.java`, No) | | |
| 1.13 | **Programming Model** (Imperative, Reactive / WebFlux, Hybrid) | | |
| 1.14 | **Kotlin / Groovy / Scala Co-usage** (Yes — specify language & version, No) | | |

---

## 1B. Multi-Module Project Map (Fill ONLY if Module Structure = Multi-Module)

> If the project is **single-module**, skip this section entirely.

### Module Inventory

List every module in the project with its role, packaging type, and what it depends on internally.

| # | Module Name | Module Path | Role / Purpose | Packaging | Internal Dependencies (depends on which modules?) |
| :--- | :--- | :--- | :--- | :--- | :--- |
| | *(e.g., core)* | `/modules/core` | Shared domain models, utilities | JAR | None (leaf module) |
| | *(e.g., persistence)* | `/modules/persistence` | Database access, repositories | JAR | core |
| | *(e.g., service)* | `/modules/service` | Business logic layer | JAR | core, persistence |
| | *(e.g., api)* | `/modules/api` | REST controllers, API layer | JAR | core, service |
| | *(e.g., app)* | `/modules/app` | Application entry point, main class | JAR / WAR | core, service, api |

### Module Dependency Order (Bottom-Up)

Determine the correct migration order by listing modules from **leaf (no internal deps)** to **root (depends on everything)**:

```
1. core (leaf — no internal dependencies)
2. persistence (depends on: core)
3. service (depends on: core, persistence)
4. api (depends on: core, service)
5. app (depends on: all)
```

### Per-Module Notes

| # | Module Name | Has Own DB Config? | Has Own Dockerfile? | Has Own CI Pipeline? | Special Notes |
| :--- | :--- | :--- | :--- | :--- | :--- |
| | *(e.g., core)* | | | | |
| | *(e.g., persistence)* | | | | |
| | *(e.g., service)* | | | | |
| | *(e.g., api)* | | | | |
| | *(e.g., app)* | | | | |

### Migration Strategy for Multi-Module

| # | Input / Component | Current Values | Planned Actions / Target State |
| :--- | :--- | :--- | :--- |
| M.1 | **Migration Strategy** (Bottom-Up by dependency order, Prepare-Then-Switch, Shared-First, Big-Bang) | | |
| M.2 | **Shared Parent POM / BOM** (What does the parent manage? Spring Boot version, dependency versions?) | | |
| M.3 | **Can modules be deployed independently?** (Yes — each is a separate deployable, No — single deployable) | | |
| M.4 | **Modules that share a database** (List which modules access the same DB) | | |
| M.5 | **Cross-module API contracts** (Do modules call each other via REST/gRPC, or only via compile-time dependencies?) | | |

---

## 2. JVM & Runtime Tuning

| # | Input / Component | Current Values | Planned Actions / Target State |
| :--- | :--- | :--- | :--- |
| 2.1 | **Garbage Collector** (G1GC, ZGC, Shenandoah, CMS, Parallel, Serial, Default) | | |
| 2.2 | **Heap Configuration** (Xms, Xmx, e.g., `-Xms512m -Xmx2g`) | | |
| 2.3 | **JVM Flags / System Properties** (List custom `-D` or `-XX:` flags) | | |
| 2.4 | **Virtual Threads (Loom)** (Enabled? Used in production?) | | |
| 2.5 | **GraalVM Native Image** (Compiled to native? AOT compilation?) | | |
| 2.6 | **Thread Pool Configuration** (Custom `ExecutorService`, `ForkJoinPool` size, `@Async` pool) | | |
| 2.7 | **JVM Agent(s)** (APM agent, profiler agent, Java agent JARs attached) | | |

---

## 3. Persistence & Data

| # | Input / Component | Current Values | Planned Actions / Target State |
| :--- | :--- | :--- | :--- |
| 3.1 | **Primary Database Engine & Version** (PostgreSQL 16, MySQL 8, Oracle 19c, SQL Server, MongoDB 7, DynamoDB, Cassandra, Redis, H2, etc.) | | |
| 3.2 | **Secondary / Cache Database** (Redis, Memcached, Hazelcast, Ehcache, Caffeine) | | |
| 3.3 | **Data Access / ORM** (Raw JDBC, JPA, Hibernate, Spring Data JPA, Spring Data JDBC, MyBatis, jOOQ, Querydsl) | | |
| 3.4 | **ORM / Data Access Version** (e.g., Hibernate 6.4, MyBatis 3.5) | | |
| 3.5 | **Connection Pool** (HikariCP, Tomcat JDBC, DBCP2, C3P0, PgBouncer) | | |
| 3.6 | **Connection Pool Version** | | |
| 3.7 | **Connection Pool Sizing** (Min idle, Max pool size, Timeout values) | | |
| 3.8 | **Schema Migration Tool** (Flyway, Liquibase, Manual DDL, None) | | |
| 3.9 | **Schema Migration Tool Version** | | |
| 3.10 | **Search Engine** (Elasticsearch, OpenSearch, Solr, None) | | |
| 3.11 | **Read Replicas / Sharding** (Read/Write split? DB sharding strategy? None) | | |
| 3.12 | **Transaction Management** (Declarative `@Transactional`, Programmatic, Distributed XA / Saga, None) | | |
| 3.13 | **Database Connection String Pattern** (Direct JDBC URL, JNDI, Service Discovery, Cloud SQL Proxy) | | |

---

## 4. Caching Strategy

| # | Input / Component | Current Values | Planned Actions / Target State |
| :--- | :--- | :--- | :--- |
| 4.1 | **L1 (In-Process) Cache** (Caffeine, Ehcache, Guava Cache, Spring Cache, None) | | |
| 4.2 | **L2 (Distributed) Cache** (Redis, Memcached, Hazelcast, Apache Ignite, None) | | |
| 4.3 | **Cache Abstraction** (Spring `@Cacheable`, JCache / JSR-107, Custom) | | |
| 4.4 | **Cache Eviction / TTL Policy** (Time-based, Size-based, Event-driven, Manual) | | |
| 4.5 | **HTTP Caching** (ETags, Cache-Control headers, CDN caching, None) | | |

---

## 5. API & Integration

| # | Input / Component | Current Values | Planned Actions / Target State |
| :--- | :--- | :--- | :--- |
| 5.1 | **API Protocol Exposed** (REST, GraphQL, gRPC, SOAP, WebSocket, SSE, RSocket) | | |
| 5.2 | **API Documentation** (Springdoc / OpenAPI 3, Swagger 2, RAML, None) | | |
| 5.3 | **API Versioning Strategy** (URI path `/v1/`, Header-based, Query param, None) | | |
| 5.4 | **HTTP Client Library** (RestTemplate, WebClient, RestClient, Feign, Retrofit, Apache HttpClient, OkHttp, Java HttpClient) | | |
| 5.5 | **Messaging / Brokers** (RabbitMQ, Kafka, ActiveMQ, IBM MQ, AWS SQS/SNS, Google Pub/Sub, JMS, None) | | |
| 5.6 | **Messaging Library / Version** (Spring Kafka, Spring AMQP, etc.) | | |
| 5.7 | **Serialization Format** (JSON / Jackson, JSON / Gson, Protobuf, Avro, XML / JAXB, MessagePack) | | |
| 5.8 | **External API Integrations** (List third-party APIs or services consumed) | | |
| 5.9 | **API Gateway** (Spring Cloud Gateway, Kong, AWS API Gateway, Zuul, NGINX, None) | | |
| 5.10 | **Service Discovery** (Eureka, Consul, Kubernetes DNS, AWS Cloud Map, None) | | |
| 5.11 | **Rate Limiting / Throttling** (Bucket4j, Resilience4j RateLimiter, API Gateway-level, Custom, None) | | |

---

## 6. Resilience & Fault Tolerance

| # | Input / Component | Current Values | Planned Actions / Target State |
| :--- | :--- | :--- | :--- |
| 6.1 | **Circuit Breaker** (Resilience4j, Hystrix, Sentinel, Spring Retry, None) | | |
| 6.2 | **Retry Mechanism** (Spring Retry, Resilience4j Retry, Custom, Exponential Backoff) | | |
| 6.3 | **Bulkhead / Thread Isolation** (Resilience4j Bulkhead, Hystrix, None) | | |
| 6.4 | **Timeout Handling** (Global timeouts configured? Per-service? Library used?) | | |
| 6.5 | **Fallback Strategy** (Graceful degradation? Static fallback? Queue for retry?) | | |
| 6.6 | **Dead Letter Queue (DLQ)** (For messaging failures — configured? Where?) | | |

---

## 7. Security & Authentication

| # | Input / Component | Current Values | Planned Actions / Target State |
| :--- | :--- | :--- | :--- |
| 7.1 | **Auth Model** (OAuth2, OpenID Connect, SAML, Basic Auth, API Key, JWT, mTLS, Custom) | | |
| 7.2 | **Auth Provider / IdP** (Keycloak, Okta, Auth0, AWS Cognito, Azure AD, Spring Security built-in) | | |
| 7.3 | **Security Framework & Version** (Spring Security 6.x, Apache Shiro, Custom) | | |
| 7.4 | **Secrets Management** (Vault, AWS Secrets Manager, Azure Key Vault, Environment Variables, Config files) | | |
| 7.5 | **CORS Configuration** (Defined? Restrictive? Open?) | | |
| 7.6 | **CSRF Protection** (Enabled? Disabled for APIs? Token-based?) | | |
| 7.7 | **Input Validation** (Bean Validation / Hibernate Validator, Custom, None) | | |
| 7.8 | **Encryption** (Data at rest: AES, column-level? Data in transit: TLS version?) | | |
| 7.9 | **Audit Logging** (Who did what, when? Spring Data Envers, Custom audit table, Event-sourced) | | |
| 7.10 | **RBAC / ABAC** (Role-based? Attribute-based? Method-level `@PreAuthorize`?) | | |

---

## 8. Scheduling & Background Processing

| # | Input / Component | Current Values | Planned Actions / Target State |
| :--- | :--- | :--- | :--- |
| 8.1 | **Job Scheduler** (Spring `@Scheduled`, Quartz, ShedLock, Spring Batch, None) | | |
| 8.2 | **Async Processing** (`@Async`, `CompletableFuture`, Project Reactor, RxJava, Virtual Threads) | | |
| 8.3 | **Batch Processing** (Spring Batch, Custom, None) | | |
| 8.4 | **Distributed Scheduling** (ShedLock for cluster-safe cron? Quartz JDBC store?) | | |
| 8.5 | **Event-Driven Architecture** (Spring Events, Domain Events, Event Sourcing, CQRS, None) | | |

---

## 9. Observability & Operations

| # | Input / Component | Current Values | Planned Actions / Target State |
| :--- | :--- | :--- | :--- |
| 9.1 | **Logging Framework** (SLF4J + Logback, Log4j2, JUL, Custom) | | |
| 9.2 | **Structured Logging** (JSON log format? MDC context propagation? Correlation IDs?) | | |
| 9.3 | **Log Aggregation** (ELK Stack, Splunk, Datadog, CloudWatch, Loki, Graylog, None) | | |
| 9.4 | **Metrics** (Micrometer, Prometheus, Dropwizard Metrics, Custom, None) | | |
| 9.5 | **Distributed Tracing** (OpenTelemetry, Micrometer Tracing, Zipkin, Jaeger, AWS X-Ray, None) | | |
| 9.6 | **Health Checks / Actuator** (Spring Boot Actuator, MicroProfile Health, Custom, None) | | |
| 9.7 | **APM Tool** (New Relic, Datadog APM, Dynatrace, AppDynamics, Elastic APM, None) | | |
| 9.8 | **Alerting** (PagerDuty, OpsGenie, Grafana Alerts, CloudWatch Alarms, Custom) | | |
| 9.9 | **Dashboard** (Grafana, Kibana, Datadog, CloudWatch Dashboards, None) | | |

---

## 10. Error Handling & Validation

| # | Input / Component | Current Values | Planned Actions / Target State |
| :--- | :--- | :--- | :--- |
| 10.1 | **Global Exception Handler** (`@ControllerAdvice` / `@RestControllerAdvice`, Custom filter, None) | | |
| 10.2 | **Error Response Format** (RFC 7807 Problem Details, Custom JSON, HTML error pages) | | |
| 10.3 | **Error Code System** (Structured error codes? Error catalog? Ad-hoc messages?) | | |
| 10.4 | **Input Validation** (Bean Validation `@Valid`, Custom validators, Manual checks) | | |
| 10.5 | **Null Safety** (Optional usage, `@NonNull` annotations, Kotlin null safety, None) | | |

---

## 11. Quality & Testing

| # | Input / Component | Current Values | Planned Actions / Target State |
| :--- | :--- | :--- | :--- |
| 11.1 | **Unit Test Framework** (JUnit 5, JUnit 4, TestNG) | | |
| 11.2 | **Mocking Library** (Mockito, EasyMock, PowerMock, WireMock) | | |
| 11.3 | **Integration Test Support** (Spring Boot Test, Testcontainers, Arquillian) | | |
| 11.4 | **Contract Testing** (Spring Cloud Contract, Pact, None) | | |
| 11.5 | **End-to-End / API Testing** (REST Assured, Karate, Postman/Newman, None) | | |
| 11.6 | **Performance / Load Testing** (JMeter, Gatling, k6, Locust, None) | | |
| 11.7 | **Code Coverage Tool** (JaCoCo, Cobertura, None) | | |
| 11.8 | **Coverage Threshold** (Minimum required? e.g., 80% line coverage) | | |
| 11.9 | **Static Analysis** (SonarQube, Checkstyle, PMD, SpotBugs, ErrorProne, ArchUnit, None) | | |
| 11.10 | **Code Formatter** (Spotless, google-java-format, Palantir, EditorConfig, IDE defaults) | | |
| 11.11 | **Dependency Vulnerability Scanner** (OWASP Dependency-Check, Snyk, Dependabot, Trivy, None) | | |
| 11.12 | **Mutation Testing** (PITest, None) | | |

---

## 12. CI/CD & DevOps

| # | Input / Component | Current Values | Planned Actions / Target State |
| :--- | :--- | :--- | :--- |
| 12.1 | **CI/CD Platform** (Jenkins, GitHub Actions, GitLab CI, Azure DevOps, CircleCI, Bamboo, TeamCity) | | |
| 12.2 | **Pipeline Stages** (Compile → Test → Analyze → Build Image → Deploy → Smoke Test) | | |
| 12.3 | **Artifact Repository** (Nexus, Artifactory, GitHub Packages, AWS CodeArtifact, Maven Central) | | |
| 12.4 | **Container Registry** (Docker Hub, ECR, GCR, ACR, Harbor, Nexus) | | |
| 12.5 | **Deployment Target** (Kubernetes, ECS, EC2, Azure App Service, GCP Cloud Run, On-prem VM, Lambda) | | |
| 12.6 | **Deployment Strategy** (Rolling, Blue-Green, Canary, Recreate, A/B) | | |
| 12.7 | **IaC Tool** (Terraform, CloudFormation, Pulumi, Ansible, Helm, Kustomize, None) | | |
| 12.8 | **Environment Promotion** (Dev → Staging → Prod? How many environments?) | | |
| 12.9 | **Feature Flags** (LaunchDarkly, Unleash, Togglz, FF4j, Custom, None) | | |
| 12.10 | **Rollback Strategy** (Automated? Manual? DB rollback plan?) | | |

---

## 13. Configuration Management

| # | Input / Component | Current Values | Planned Actions / Target State |
| :--- | :--- | :--- | :--- |
| 13.1 | **Config Style** (`application.yml`, `application.properties`, TOML, XML) | | |
| 13.2 | **Profiles / Environments** (dev, staging, prod, test — how managed?) | | |
| 13.3 | **External Config Server** (Spring Cloud Config, Consul, etcd, AWS AppConfig, None) | | |
| 13.4 | **Environment Variables** (12-factor app compliant? `SPRING_` prefix overrides?) | | |
| 13.5 | **Config Encryption** (Jasypt, Spring Cloud Vault, sealed secrets, None) | | |

---

## 14. Frontend / UI Integration

| # | Input / Component | Current Values | Planned Actions / Target State |
| :--- | :--- | :--- | :--- |
| 14.1 | **Frontend Type** (Embedded SPA, Server-side rendered, Separate frontend repo, API-only / headless) | | |
| 14.2 | **Template Engine** (Thymeleaf, FreeMarker, JSP, Mustache, None) | | |
| 14.3 | **Frontend Framework** (React, Angular, Vue, HTMX, None) | | |
| 14.4 | **Static Asset Serving** (Embedded in JAR, CDN, NGINX, S3 + CloudFront) | | |

---

## 15. File Storage & Media

| # | Input / Component | Current Values | Planned Actions / Target State |
| :--- | :--- | :--- | :--- |
| 15.1 | **File Storage** (AWS S3, Azure Blob, GCS, MinIO, Local filesystem, NFS, None) | | |
| 15.2 | **File Upload Handling** (Multipart, Streaming, Presigned URLs, Chunked) | | |
| 15.3 | **Document / Report Generation** (Apache POI, JasperReports, iText PDF, OpenPDF, None) | | |
| 15.4 | **Email / Notifications** (Spring Mail, SendGrid, AWS SES, Twilio, Custom SMTP, None) | | |

---

## 16. Internationalization & Locale

| # | Input / Component | Current Values | Planned Actions / Target State |
| :--- | :--- | :--- | :--- |
| 16.1 | **i18n Support** (Spring `MessageSource`, Resource bundles, Database-driven, None) | | |
| 16.2 | **Supported Locales** (en-US, fr-FR, etc.) | | |
| 16.3 | **Timezone Handling** (UTC storage? User-local display? `ZonedDateTime`?) | | |
| 16.4 | **Character Encoding** (UTF-8, ISO-8859-1, etc.) | | |
| 16.5 | **Currency / Number Formatting** (Java `NumberFormat`, ICU4J, Custom) | | |

---

## 17. Compliance & Governance

| # | Input / Component | Current Values | Planned Actions / Target State |
| :--- | :--- | :--- | :--- |
| 17.1 | **Data Privacy** (GDPR, CCPA, HIPAA, PCI-DSS, SOX — applicable regulations) | | |
| 17.2 | **Data Retention Policy** (Automatic purge? Archival? TTL on records?) | | |
| 17.3 | **PII Handling** (Masked in logs? Encrypted at rest? Tokenized?) | | |
| 17.4 | **License Compliance** (OSS license scanning? FOSSA, Black Duck, SPDX, None) | | |
| 17.5 | **Accessibility (a11y)** (WCAG compliance level? Automated a11y testing?) | | |

---

## 18. Multi-Tenancy & Scalability

| # | Input / Component | Current Values | Planned Actions / Target State |
| :--- | :--- | :--- | :--- |
| 18.1 | **Multi-Tenancy Model** (Shared DB / Shared Schema, Shared DB / Separate Schema, Separate DB per Tenant, None) | | |
| 18.2 | **Tenant Resolution** (Subdomain, Header, JWT claim, Path, Custom) | | |
| 18.3 | **Horizontal Scaling** (Stateless app? Session affinity? Sticky sessions?) | | |
| 18.4 | **Auto-Scaling** (HPA on K8s, ECS auto-scaling, VM scale sets, None) | | |
| 18.5 | **Session Management** (Stateless JWT, Spring Session + Redis, Servlet session, None) | | |

---

## 19. Manifests & Configuration Files

| # | File / Config | Present? (Y/N) | Path / Notes |
| :--- | :--- | :--- | :--- |
| 19.1 | **Build Manifest** (`pom.xml` / `build.gradle` / `build.gradle.kts`) | | |
| 19.2 | **Parent POM / BOM** (Spring Boot parent, custom BOM) | | |
| 19.3 | **Settings File** (`settings.gradle`, `settings.xml`) | | |
| 19.4 | **App Configuration** (`application.yml` / `application.properties`) | | |
| 19.5 | **Profile-specific Configs** (`application-dev.yml`, `-staging.yml`, `-prod.yml`) | | |
| 19.6 | **Bootstrap Config** (`bootstrap.yml` — for Spring Cloud Config) | | |
| 19.7 | **Logback / Log4j Config** (`logback-spring.xml`, `log4j2.xml`) | | |
| 19.8 | **Dockerfile** | | |
| 19.9 | **Multi-stage Dockerfile** (Separate build & runtime stages?) | | |
| 19.10 | **Docker Compose** (`docker-compose.yml`) | | |
| 19.11 | **Kubernetes Manifests** (Deployment, Service, ConfigMap, Ingress, HPA, PDB) | | |
| 19.12 | **Helm Chart** (`Chart.yaml`, `values.yaml`) | | |
| 19.13 | **Kustomize Overlays** (`kustomization.yaml`) | | |
| 19.14 | **CI/CD Pipeline Config** (`.github/workflows/*.yml`, `Jenkinsfile`, `.gitlab-ci.yml`) | | |
| 19.15 | **Environment / Secrets Files** (`.env`, `vault-config`, `secrets.yaml`) | | |
| 19.16 | **EditorConfig / Linter Config** (`.editorconfig`, `checkstyle.xml`, `spotbugs-exclude.xml`) | | |
| 19.17 | **Git Config** (`.gitignore`, `.gitattributes`, `.gitmessage`) | | |
| 19.18 | **Terraform / IaC Files** (`main.tf`, `variables.tf`) | | |
| 19.19 | **README / Docs** (`README.md`, `CONTRIBUTING.md`, `CHANGELOG.md`, `ADR/`) | | |
| 19.20 | **OpenAPI Spec** (`openapi.yaml`, `openapi.json`) | | |
| 19.21 | **Makefile / Taskfile** (`Makefile`, `Taskfile.yml`) | | |

---

## 20. Dependency Chart

A detailed view of all project dependencies with their current and planned versions. This can be auto-populated by running:
- **Maven**: `./mvnw dependency:tree` or `./mvnw versions:display-dependency-updates`
- **Gradle**: `./gradlew dependencies` or `./gradlew dependencyUpdates`

| # | Dependency (GroupId : ArtifactId) | Scope | Current Version | Latest Available | Planned / Target Version | Notes / Reason for Change |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| | *(e.g., org.springframework.boot : spring-boot-starter-web)* | compile | | | | |
| | *(e.g., org.springframework.boot : spring-boot-starter-data-jpa)* | compile | | | | |
| | *(e.g., org.springframework.boot : spring-boot-starter-security)* | compile | | | | |
| | *(e.g., org.springframework.boot : spring-boot-starter-actuator)* | compile | | | | |
| | *(e.g., org.postgresql : postgresql)* | runtime | | | | |
| | *(e.g., org.hibernate.orm : hibernate-core)* | compile | | | | |
| | *(e.g., com.zaxxer : HikariCP)* | compile | | | | |
| | *(e.g., org.flywaydb : flyway-core)* | compile | | | | |
| | *(e.g., com.fasterxml.jackson.core : jackson-databind)* | compile | | | | |
| | *(e.g., io.micrometer : micrometer-core)* | compile | | | | |
| | *(e.g., org.springdoc : springdoc-openapi-starter-webmvc-ui)* | compile | | | | |
| | *(e.g., org.springframework.boot : spring-boot-starter-test)* | test | | | | |
| | *(e.g., org.junit.jupiter : junit-jupiter)* | test | | | | |
| | *(e.g., org.mockito : mockito-core)* | test | | | | |
| | *(e.g., org.testcontainers : testcontainers)* | test | | | | |

---

## 21. Source Control

| # | Input / Component | Current Values | Planned Actions / Target State |
| :--- | :--- | :--- | :--- |
| 21.1 | **VCS Platform** (GitHub, GitLab, Bitbucket, Azure Repos) | | |
| 21.2 | **Repository URL** | | |
| 21.3 | **Default Branch** (main, master, develop) | | |
| 21.4 | **Branching Strategy** (GitFlow, Trunk-based, Feature Branching, Release Branching) | | |
| 21.5 | **PR / Merge Review Process** (Required reviewers? CI gates? Auto-merge?) | | |
| 21.6 | **Commit Convention** (Conventional Commits, Jira ticket prefix, Free-form) | | |
| 21.7 | **Mono-repo Tooling** (Nx, Lerna, Bazel, None — if applicable) | | |

---

> **How to use this template:**
> 1. Clone/copy this file into your project's documentation.
> 2. Fill the **Current Values** column by inspecting `pom.xml`/`build.gradle`, config files, Dockerfiles, and CI configs — or by running dependency commands.
> 3. Use the **Planned Actions / Target State** column to plan migrations, upgrades, or architectural changes.
> 4. Review and update periodically as the project evolves.
> 5. When evaluating a new project, use this as a checklist to ensure nothing is overlooked.
