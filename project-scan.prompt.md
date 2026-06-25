---
description: 'Scan and analyze the current Java project — detect framework, dependencies, architecture, and overall health'
mode: agent
---

# Project Scan

Perform a comprehensive scan of this Java project and report your findings.

## What to Scan

1. **Read the build file** (`pom.xml` or `build.gradle` / `build.gradle.kts`):
   - Build system and version
   - Java version configured
   - All direct dependencies with versions
   - Plugins configured
   - Multi-module structure (if any)

2. **Detect the framework**:
   - Look for Spring Boot starters, `@SpringBootApplication`, Quarkus, Micronaut, or Jakarta EE markers
   - Identify the framework version

3. **Scan application config**:
   - Read `application.yml` / `application.properties`
   - Identify profiles (dev, staging, prod)
   - Detect database config, messaging config, security config

4. **Detect architecture style**:
   - Examine package structure
   - Classify: package-by-layer, package-by-feature, hexagonal, or other

5. **Check testing setup**:
   - JUnit 4 or 5?
   - Mockito, WireMock, Testcontainers?
   - Code coverage tool?

6. **Check DevOps setup**:
   - Dockerfile present?
   - docker-compose.yml?
   - CI/CD pipeline (`.github/workflows/`, `Jenkinsfile`)?
   - Kubernetes manifests?

7. **Identify potential issues**:
   - Outdated dependencies
   - Deprecated API usage
   - Missing best practices (no tests, no migration tool, hardcoded secrets)

## Output Format

Present findings as a structured **Application Profile**:

```
PROJECT PROFILE
═══════════════
Name:           [project name]
Type:           [microservice / monolith / library / batch / CLI]
Java:           [version]
Build:          [Maven / Gradle] [wrapper: yes/no]
Framework:      [Spring Boot X.Y.Z / Quarkus / etc.]

MODULES & DEPENDENCIES
═══════════════════════
[List key dependencies grouped by category]

ARCHITECTURE
════════════
Style:          [package-by-layer / by-feature / hexagonal]
Layers:         [controller, service, repository, etc.]

PERSISTENCE
═══════════
ORM:            [Hibernate / MyBatis / jOOQ / JDBC]
Database:       [PostgreSQL / MySQL / etc.]
Migration Tool: [Flyway / Liquibase / none]

SECURITY
════════
Framework:      [Spring Security / none]
Auth Type:      [JWT / OAuth2 / Session / Basic / none]

TESTING
═══════
Framework:      [JUnit 4 / JUnit 5]
Mocking:        [Mockito / WireMock]
Integration:    [Testcontainers / @SpringBootTest / none]
Coverage Tool:  [JaCoCo / none]

DEVOPS
══════
Container:      [Docker / Jib / none]
Orchestration:  [Kubernetes / Docker Compose / none]
CI/CD:          [GitHub Actions / Jenkins / none]

⚠️ ISSUES FOUND
════════════════
[List any concerns, outdated deps, missing best practices]
```

Then ask: **"Would you like me to address any of these issues?"**
