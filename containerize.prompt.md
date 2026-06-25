---
description: 'Containerize the Java app — create optimized Dockerfile, docker-compose, or migrate to Jib/Buildpacks'
mode: agent
---

# Containerize

Containerize this Java application or optimize its existing container setup.

## Step 1: Scan Current State

1. Check for existing `Dockerfile`
2. Check for `docker-compose.yml`
3. Check for Jib or Buildpacks plugin in build file
4. Check for `.dockerignore`
5. Determine artifact type: JAR (fat/thin), WAR
6. Check Java version for base image selection

## Step 2: Determine Approach

| Approach | When to Use |
|---|---|
| **Multi-stage Dockerfile** | Most control, custom build steps |
| **Jib** (Maven/Gradle plugin) | No Docker daemon needed, fast layered images |
| **Cloud Native Buildpacks** | Zero config, auto-detect |
| **Spring Boot Layered JAR** | Optimized Docker layer caching |

## Step 3: Generate / Optimize

### New Dockerfile (Multi-Stage)

```dockerfile
# --- Build Stage ---
FROM eclipse-temurin:21-jdk-alpine AS builder
WORKDIR /app
COPY .mvn/ .mvn
COPY mvnw pom.xml ./
RUN ./mvnw dependency:resolve -B
COPY src ./src
RUN ./mvnw package -DskipTests -B

# --- Runtime Stage ---
FROM eclipse-temurin:21-jre-alpine
WORKDIR /app

# Create non-root user
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

# Copy artifact
COPY --from=builder /app/target/*.jar app.jar

# Set ownership
RUN chown -R appuser:appgroup /app
USER appuser

# Health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=30s \
  CMD wget -qO- http://localhost:8080/actuator/health || exit 1

EXPOSE 8080

ENTRYPOINT ["java", \
  "-XX:+UseContainerSupport", \
  "-XX:MaxRAMPercentage=75.0", \
  "-jar", "app.jar"]
```

### docker-compose.yml (if needed)

Generate docker-compose with app + dependencies (DB, Redis, Kafka, etc.)
based on what was detected in the project scan.

### .dockerignore

```
.git
.gitignore
.idea
*.iml
target/
!target/*.jar
build/
!build/libs/*.jar
.gradle
node_modules
*.md
LICENSE
```

### Jib Setup (if preferred)

```xml
<plugin>
    <groupId>com.google.cloud.tools</groupId>
    <artifactId>jib-maven-plugin</artifactId>
    <version>3.4.0</version>
    <configuration>
        <from>
            <image>eclipse-temurin:21-jre-alpine</image>
        </from>
        <to>
            <image>${project.artifactId}</image>
            <tags><tag>${project.version}</tag></tags>
        </to>
        <container>
            <jvmFlags>
                <jvmFlag>-XX:+UseContainerSupport</jvmFlag>
                <jvmFlag>-XX:MaxRAMPercentage=75.0</jvmFlag>
            </jvmFlags>
            <ports><port>8080</port></ports>
            <user>1000</user>
        </container>
    </configuration>
</plugin>
```

## Step 4: Optimization Checks

If an existing Dockerfile exists, check for:
- [ ] Multi-stage build (separate build and runtime)
- [ ] Minimal base image (JRE-only, Alpine or distroless)
- [ ] Non-root user
- [ ] Container-aware JVM flags (`-XX:+UseContainerSupport`, `-XX:MaxRAMPercentage`)
- [ ] Layer caching optimization (copy deps before source code)
- [ ] Health check defined
- [ ] `.dockerignore` present
- [ ] No secrets baked into image

## Step 5: Present Plan & Execute

Show the proposed files, get approval, then create/update them.
Verify with `docker build -t app:test .`
