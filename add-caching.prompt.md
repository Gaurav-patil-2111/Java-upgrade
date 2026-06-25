---
description: 'Add caching — Spring Cache abstraction with Redis, Caffeine, or EhCache, including cache configuration and strategy'
mode: agent
---

# Add Caching

Add caching to this Java application.

## Step 1: Scan Current State

1. Check for existing cache dependencies (Redis, Caffeine, EhCache)
2. Check for `@Cacheable`, `@CacheEvict`, `@CachePut` annotations
3. Check for Spring Cache configuration
4. Check application.yml for cache-related properties
5. Identify hot spots that would benefit from caching:
   - Frequently accessed database queries
   - External API calls
   - Expensive computations

## Step 2: Recommend Cache Provider

| Provider | Best For |
|---|---|
| **Caffeine** | Local/single-instance app, fastest local cache |
| **Redis** | Distributed/multi-instance, need shared cache, session store |
| **EhCache 3** | Local with overflow to disk, JCache compatible |
| **Hazelcast** | Distributed, embedded data grid |

Decision factors:
- Single instance or multiple? → Single: Caffeine, Multiple: Redis
- Need persistence across restarts? → Redis
- Need fast local + shared? → Multi-level (Caffeine L1 + Redis L2)

## Step 3: Present Plan

```
CACHING PLAN
════════════

Recommended Provider: [Caffeine / Redis / Both]
Reason: [why this choice]

Phase 1 — Dependencies & Configuration
  [ ] Add spring-boot-starter-cache
  [ ] Add cache provider dependency (caffeine / spring-boot-starter-data-redis)
  [ ] Enable caching: @EnableCaching on config class
  [ ] Configure cache properties in application.yml

Phase 2 — Cache Hot Spots
  [ ] [ServiceClass.method()] — [reason] — TTL: [duration]
  [ ] [ServiceClass.method()] — [reason] — TTL: [duration]
  [ ] [ServiceClass.method()] — [reason] — TTL: [duration]

Phase 3 — Cache Invalidation
  [ ] Add @CacheEvict on write/update methods
  [ ] Add @CachePut where updates should refresh cache
  [ ] Configure TTL for time-based expiration

Phase 4 — Monitoring
  [ ] Add cache hit/miss metrics (Micrometer)
  [ ] Add cache size monitoring
```

**Wait for approval.**

## Step 4: Execute

Add dependencies, configuration, and `@Cacheable` annotations incrementally.
Build and test after each change.

## Example Configurations

### Caffeine (application.yml)
```yaml
spring:
  cache:
    type: caffeine
    caffeine:
      spec: maximumSize=500,expireAfterWrite=10m
    cache-names:
      - users
      - products
```

### Redis (application.yml)
```yaml
spring:
  data:
    redis:
      host: localhost
      port: 6379
  cache:
    type: redis
    redis:
      time-to-live: 600000  # 10 minutes in ms
      cache-null-values: false
```
