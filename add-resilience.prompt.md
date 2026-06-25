---
description: 'Add resilience patterns — circuit breakers, retries, bulkheads, rate limiters, and timeouts using Resilience4j'
mode: agent
---

# Add Resilience

Add fault tolerance and resilience patterns to this Java application using Resilience4j.

## Step 1: Scan Current State

1. Check for existing resilience dependencies (Resilience4j, Hystrix)
2. Check for `@CircuitBreaker`, `@Retry`, `@Bulkhead` annotations
3. Identify external service calls (HTTP clients, DB, message brokers)
4. Check for timeout configurations
5. Check for fallback implementations

## Step 2: Identify Resilience Needs

For each external dependency, determine:
- How critical is it? (Can the app function without it?)
- How reliable is it? (Failure rate, latency)
- What should happen on failure? (Fallback, fail-fast, queue)

## Step 3: Present Plan

```
RESILIENCE PLAN
═══════════════

External Dependencies Found:
  1. [Service/DB/Broker] — called from [where] — criticality: [HIGH/MED/LOW]
  2. [Service/DB/Broker] — called from [where] — criticality: [HIGH/MED/LOW]

Phase 1 — Dependencies
  [ ] Add resilience4j-spring-boot3 (or resilience4j-spring-boot2)
  [ ] Add resilience4j-micrometer (for metrics)

Phase 2 — Circuit Breakers
  For each critical external call:
  [ ] Add @CircuitBreaker with fallback method
  [ ] Configure: failure threshold, wait duration, permitted calls in half-open

Phase 3 — Retry
  [ ] Add @Retry for transient failures
  [ ] Configure: max attempts, wait duration, exponential backoff
  [ ] Define which exceptions trigger retry

Phase 4 — Timeouts
  [ ] Add @TimeLimiter for long-running calls
  [ ] Configure: timeout duration per service

Phase 5 — Bulkhead (if needed)
  [ ] Add @Bulkhead to isolate failure domains
  [ ] Configure: max concurrent calls, max wait duration

Phase 6 — Rate Limiter (if needed)
  [ ] Add @RateLimiter for APIs calling rate-limited services
  [ ] Configure: limit for period, refresh period

Phase 7 — Monitoring
  [ ] Expose Resilience4j metrics via Micrometer
  [ ] Add Actuator endpoints for circuit breaker state
  [ ] Configure alerts for circuit breaker open events
```

**Wait for approval.**

## Example Configuration (application.yml)

```yaml
resilience4j:
  circuitbreaker:
    instances:
      paymentService:
        register-health-indicator: true
        sliding-window-size: 10
        failure-rate-threshold: 50
        wait-duration-in-open-state: 10s
        permitted-number-of-calls-in-half-open-state: 3
        
  retry:
    instances:
      paymentService:
        max-attempts: 3
        wait-duration: 1s
        exponential-backoff-multiplier: 2
        retry-exceptions:
          - java.io.IOException
          - java.net.SocketTimeoutException
          
  timelimiter:
    instances:
      paymentService:
        timeout-duration: 3s
```

## Step 4: Execute

Apply one pattern at a time, with tests verifying the behavior:
- Circuit breaker opens on repeated failures
- Retry succeeds on transient failure
- Fallback returns graceful degradation
