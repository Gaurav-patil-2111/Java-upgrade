---
description: 'Upgrade Java version (8→11→17→21→25) with full compatibility checks and incremental migration'
mode: agent
---

# Java Version Upgrade

Upgrade the Java version for this project.

## Step 1: Scan Current State

1. Read `pom.xml` or `build.gradle` to find:
   - Current Java version (`<java.version>`, `sourceCompatibility`)
   - All dependencies and their versions
   - Build plugins (especially `maven-compiler-plugin` or Gradle Java plugin)
2. Check for `.java-version`, `.sdkmanrc`, or `.tool-versions` files
3. Check Dockerfile for base image Java version
4. Check CI/CD pipeline for Java version

## Step 2: Determine Target Version

- If the user specified a version, use that
- If not, recommend the latest LTS (Java 21 as of 2025, Java 25 as of 2025)
- Validate against compatibility matrix:
  - Spring Boot 2.x supports Java 8-21
  - Spring Boot 3.x requires Java 17+

## Step 3: Analyze Impact

Check for issues at each version boundary:

### Java 8 → 11
- `javax.xml.bind` (JAXB) removed — need `jakarta.xml.bind-api` dependency
- `javax.annotation` removed — need `jakarta.annotation-api` dependency
- JavaFX removed from JDK — need separate dependency
- `sun.misc.Unsafe` usage — check dependencies
- Module system introduced (but not enforced)

### Java 11 → 17
- Strong encapsulation of JDK internals (Java 16)
- `--add-opens` flags may be needed for reflection-heavy libraries
- Nashorn JavaScript engine removed (Java 15)
- `SecurityManager` deprecated (Java 17)
- Records (Java 16) — available to use
- Sealed classes (Java 17) — available to use
- Text blocks (Java 15) — available to use

### Java 17 → 21
- `SecurityManager` further restricted
- Virtual Threads available (Java 21)
- Pattern matching for switch (Java 21)
- Record patterns (Java 21)
- Sequenced Collections (Java 21)
- Generally smooth — few breaking changes

### Java 21 → 25
- `SecurityManager` removed (Java 24)
- Check for any deprecated API removals
- String templates evolved
- Structured concurrency and scoped values

## Step 4: Check Dependency Compatibility

For EACH direct dependency, verify it supports the target Java version:
- Search for known issues with `[library-name] java [target-version] compatibility`
- Pay special attention to: Lombok, MapStruct, annotation processors, byte-code manipulation libraries (ByteBuddy, ASM, cglib)

## Step 5: Present Plan

```
JAVA UPGRADE PLAN
═════════════════
Current: Java [X]
Target:  Java [Y]
Risk:    [LOW / MEDIUM / HIGH]

CHANGES:
Phase 1 — Build Configuration
  [ ] Update <java.version> in pom.xml / sourceCompatibility in build.gradle
  [ ] Update maven-compiler-plugin version (if needed)
  [ ] Update Gradle wrapper (if needed)

Phase 2 — Dependency Fixes (if needed)
  [ ] Add jakarta.xml.bind-api (if upgrading from 8/9/10)
  [ ] Update Lombok to version X.Y.Z
  [ ] Update [other deps as needed]
  [ ] Add --add-opens flags (if needed)

Phase 3 — Infrastructure
  [ ] Update Dockerfile base image
  [ ] Update CI/CD pipeline Java version
  [ ] Update .java-version / .sdkmanrc

Phase 4 — Verify
  [ ] Run: mvn clean verify / gradle build
  [ ] All tests pass
  [ ] No illegal-access warnings at runtime

Phase 5 — Optional Modernization
  [ ] Adopt Records for DTOs
  [ ] Adopt text blocks for multi-line strings
  [ ] Adopt pattern matching for instanceof
  [ ] Adopt virtual threads (Java 21+)
  [ ] Adopt switch expressions
```

**Wait for user approval before executing.**

## Step 6: Execute

Apply changes ONE phase at a time. After each phase, suggest running the build.

## Step 7: Report

Summarize what was changed and recommend next steps for adopting new language features.
