---
description: 'Add full observability stack — Spring Actuator, Micrometer metrics, Prometheus, distributed tracing, structured logging'
mode: agent
---

# Add Observability

Set up a complete observability stack for this Java application.

## Step 1: Scan Current State

1. Check if Spring Boot Actuator is present
2. Check if Micrometer is configured
3. Check logging setup (Logback, Log4j2, format)
4. Check if distributed tracing is configured
5. Check if health endpoints exist
6. Check if Kubernetes probes are configured

## Step 2: Present Observability Plan

```
OBSERVABILITY SETUP PLAN
═════════════════════════

Current State:
  Actuator:  [yes / no]
  Metrics:   [Micrometer+Prometheus / JMX only / none]
  Tracing:   [Micrometer Tracing / Sleuth / none]
  Logging:   [Logback / Log4j2] [plain text / JSON]
  Health:    [basic / custom indicators / none]

Phase 1 — Spring Boot Actuator
  [ ] Add spring-boot-starter-actuator dependency
  [ ] Configure endpoint exposure in application.yml:
      management.endpoints.web.exposure.include: health,info,metrics,prometheus
  [ ] Configure health endpoint details:
      management.endpoint.health.show-details: when-authorized

Phase 2 — Metrics (Micrometer + Prometheus)
  [ ] Add micrometer-registry-prometheus dependency
  [ ] Verify /actuator/prometheus endpoint is available
  [ ] Add custom business metrics where appropriate:
      - Counter: requests processed, orders created
      - Timer: operation duration
      - Gauge: active connections, queue size

Phase 3 — Distributed Tracing
  [ ] Add micrometer-tracing-bridge-brave (or bridge-otel) dependency
  [ ] Add zipkin-reporter-brave (for Zipkin) or OTLP exporter (for Jaeger/OTEL)
  [ ] Configure sampling rate:
      management.tracing.sampling.probability: 1.0 (dev) / 0.1 (prod)
  [ ] Verify trace ID propagation in HTTP headers

Phase 4 — Structured Logging
  [ ] Configure logback-spring.xml for JSON output:
      - Add logstash-logback-encoder dependency
      - Configure JSON appender for production profile
      - Keep plain text for local development
  [ ] Add MDC filter for correlation/trace IDs
  [ ] Add MDC context: traceId, spanId, userId, requestId
  [ ] Ensure trace ID appears in every log line

Phase 5 — Health Checks
  [ ] Add custom health indicators for:
      - Database connectivity
      - External service availability
      - Message broker connectivity
      - Cache availability
  [ ] Configure Kubernetes probes (if using K8s):
      livenessProbe: /actuator/health/liveness
      readinessProbe: /actuator/health/readiness

Phase 6 — Alerting Recommendations
  Recommend setting up alerts for:
  - Error rate > threshold (5xx responses)
  - Response time > SLA (p99 latency)
  - Health check failures
  - JVM memory pressure
  - Thread pool exhaustion
```

**Wait for approval.**

## Step 3: Execute

Apply one phase at a time, build and test after each.

## Key Dependencies to Add

```xml
<!-- Actuator -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>

<!-- Prometheus Metrics -->
<dependency>
    <groupId>io.micrometer</groupId>
    <artifactId>micrometer-registry-prometheus</artifactId>
</dependency>

<!-- Distributed Tracing -->
<dependency>
    <groupId>io.micrometer</groupId>
    <artifactId>micrometer-tracing-bridge-brave</artifactId>
</dependency>
<dependency>
    <groupId>io.zipkin.reporter2</groupId>
    <artifactId>zipkin-reporter-brave</artifactId>
</dependency>

<!-- Structured Logging -->
<dependency>
    <groupId>net.logstash.logback</groupId>
    <artifactId>logstash-logback-encoder</artifactId>
    <version>7.4</version>
</dependency>
```
