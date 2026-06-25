---
description: 'Upgrade Spring Boot version with namespace migration, security changes, and full compatibility alignment'
mode: agent
---

# Spring Boot Upgrade

Upgrade the Spring Boot version for this project.

## Step 1: Scan Current State

1. Read `pom.xml` / `build.gradle` — find current Spring Boot version
2. Identify all Spring Boot starters used
3. Check for Spring Cloud dependencies (need version alignment)
4. Check for Spring Security configuration
5. Check current Java version
6. Read `application.yml` / `application.properties` for all Spring properties

## Step 2: Determine Upgrade Path

Use the compatibility matrix:

```
Spring Boot 2.7.x → Java 8+  → javax.*  → Spring Cloud 2021.0.x → Hibernate 5.6.x
Spring Boot 3.0.x → Java 17+ → jakarta.* → Spring Cloud 2022.0.x → Hibernate 6.1.x
Spring Boot 3.1.x → Java 17+ → jakarta.* → Spring Cloud 2022.0.x → Hibernate 6.2.x
Spring Boot 3.2.x → Java 17+ → jakarta.* → Spring Cloud 2023.0.x → Hibernate 6.4.x
Spring Boot 3.3.x → Java 17+ → jakarta.* → Spring Cloud 2023.0.x → Hibernate 6.5.x
Spring Boot 3.4.x → Java 17+ → jakarta.* → Spring Cloud 2024.0.x → Hibernate 6.6.x
Spring Boot 3.5.x → Java 17+ → jakarta.* → Spring Cloud 2025.0.x → Hibernate 6.7.x
```

**If crossing 2.x → 3.x**: This is a MAJOR migration. Follow the full plan below.
**If within 3.x minor**: Simpler — mainly property renames and dependency bumps.

## Step 3: Plan (2.x → 3.x Full Migration)

```
SPRING BOOT UPGRADE PLAN
═════════════════════════
Current: Spring Boot [X] + Java [Y]
Target:  Spring Boot [Z] + Java [W]
Risk:    HIGH (major version crossing)

PRE-REQUISITE PHASE
  [ ] Upgrade to latest Spring Boot 2.7.x first (if not already)
  [ ] Fix ALL deprecation warnings in current version
  [ ] Ensure Java 17+ is configured (run java-upgrade if needed)
  [ ] Ensure all tests pass on current version

PHASE 1 — Namespace Migration (javax → jakarta)
  [ ] Replace javax.persistence.* → jakarta.persistence.*
  [ ] Replace javax.validation.* → jakarta.validation.*
  [ ] Replace javax.servlet.* → jakarta.servlet.*
  [ ] Replace javax.annotation.* → jakarta.annotation.*
  [ ] Replace javax.transaction.* → jakarta.transaction.*
  [ ] Replace javax.mail.* → jakarta.mail.* (if used)
  [ ] Replace javax.websocket.* → jakarta.websocket.* (if used)
  [ ] ⚠️ DO NOT replace: javax.crypto, javax.net, javax.security.auth,
      javax.sql, javax.naming (these are JDK packages, they stay!)
  [ ] Update dependencies that provide javax APIs to jakarta versions

PHASE 2 — Spring Boot Version Bump
  [ ] Update spring-boot-starter-parent / spring-boot-dependencies BOM
  [ ] Update Spring Cloud BOM version (per compatibility matrix)
  [ ] Update spring-boot-maven-plugin / spring-boot-gradle-plugin

PHASE 3 — Spring Security Migration (if using Spring Security)
  [ ] Remove WebSecurityConfigurerAdapter (deleted in Spring Security 6)
  [ ] Create SecurityFilterChain @Bean instead
  [ ] Update: authorizeRequests() → authorizeHttpRequests()
  [ ] Update: antMatchers() → requestMatchers()
  [ ] Update: .access("hasRole('X')") → .hasRole("X")
  [ ] Review CSRF, CORS, session management config

PHASE 4 — Property & Config Changes
  Common renames in Spring Boot 3.x:
  [ ] spring.redis.* → spring.data.redis.*
  [ ] spring.elasticsearch.* → spring.elasticsearch.uris (restructured)
  [ ] spring.jpa.hibernate.use-new-id-generator-mappings (removed)
  [ ] spring.mvc.throw-exception-if-no-handler-found (removed, now default)
  [ ] management.endpoints changes (review Actuator config)
  [ ] Check for any other renamed/removed properties

PHASE 5 — Third-Party Dependency Updates
  [ ] Update Hibernate (5.x → 6.x — handled by Spring Boot BOM)
  [ ] Update Flyway / Liquibase (if used)
  [ ] Update Swagger/SpringFox → SpringDoc OpenAPI (if used)
  [ ] Update any other deps that need jakarta-compatible versions
  [ ] Remove any javax.* API bridge dependencies no longer needed

PHASE 6 — Test Updates
  [ ] Update test imports (javax → jakarta in test files)
  [ ] Update @AutoConfigureMockMvc, @WebMvcTest changes
  [ ] Update any Spring Security test utilities
  [ ] Run full test suite

PHASE 7 — Infrastructure
  [ ] Update Dockerfile base image (Java 17+)
  [ ] Update CI/CD pipeline
  [ ] Update deployment configs

VERIFY
  [ ] mvn clean verify / gradle build — ALL tests pass
  [ ] Application starts successfully
  [ ] /actuator/health returns UP
  [ ] Smoke test key endpoints
```

**Wait for user approval before executing.**

## Step 4: Execute

Apply ONE phase at a time. Build and test after each phase.

## Step 5: Common Errors & Fixes

If you encounter these errors during the upgrade:

| Error | Fix |
|---|---|
| `ClassNotFoundException: javax.persistence.Entity` | Incomplete javax→jakarta migration, search for remaining javax imports |
| `WebSecurityConfigurerAdapter cannot be resolved` | Migrate to SecurityFilterChain bean-based config |
| `spring.redis.host not recognized` | Rename to `spring.data.redis.host` |
| `No bean named 'entityManagerFactory'` | Check JPA auto-config, ensure jakarta.persistence deps are correct |
| `HttpStatus.BANDWIDTH_LIMIT_EXCEEDED removed` | Replace with appropriate status code |
| `Trailing slash URL matching no longer default` | Add `spring.mvc.pathmatch.trailing-slash=true` or update tests |

## Step 6: Report

Summarize all changes with before/after dependency versions table.
