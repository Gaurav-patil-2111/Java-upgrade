---
description: 'Modernize testing — migrate JUnit 4→5, add Testcontainers, improve coverage, add ArchUnit'
mode: agent
---

# Test Modernizer

Modernize the testing setup for this Java project.

## Step 1: Scan Current Testing State

1. Check dependencies: JUnit 4 (`junit:junit`) vs JUnit 5 (`org.junit.jupiter`)
2. Check for Mockito version
3. Check for integration test setup (`@SpringBootTest`, Testcontainers)
4. Check for code coverage tool (JaCoCo)
5. Check for test structure (are tests organized? naming conventions?)
6. Count: total test files, test methods, approximate coverage

## Step 2: Identify Improvements

### JUnit 4 → JUnit 5 Migration
If using JUnit 4, identify all test files and note migration needs:

| JUnit 4 | JUnit 5 |
|---|---|
| `import org.junit.Test` | `import org.junit.jupiter.api.Test` |
| `import org.junit.Before` | `import org.junit.jupiter.api.BeforeEach` |
| `import org.junit.After` | `import org.junit.jupiter.api.AfterEach` |
| `import org.junit.BeforeClass` | `import org.junit.jupiter.api.BeforeAll` |
| `import org.junit.Ignore` | `import org.junit.jupiter.api.Disabled` |
| `import org.junit.Assert.*` | `import org.junit.jupiter.api.Assertions.*` |
| `@RunWith(SpringRunner.class)` | `@ExtendWith(SpringExtension.class)` or just `@SpringBootTest` |
| `@RunWith(MockitoJUnitRunner.class)` | `@ExtendWith(MockitoExtension.class)` |
| `@Rule ExpectedException` | `assertThrows()` |
| `@Rule TemporaryFolder` | `@TempDir` |
| `Assert.assertEquals(expected, actual)` | `Assertions.assertEquals(expected, actual)` |

### Testcontainers Integration
If not using Testcontainers, recommend adding for:
- Database integration tests (PostgreSQL, MySQL, MongoDB)
- Messaging integration tests (Kafka, RabbitMQ)
- Redis integration tests

### Code Coverage
If no coverage tool:
- Add JaCoCo plugin
- Configure minimum coverage threshold
- Generate reports in CI

### ArchUnit
If no architecture tests:
- Add ArchUnit dependency
- Create rules: no cycles, layer dependencies, naming conventions

### AssertJ
If using JUnit Assertions:
- Recommend AssertJ for more fluent, readable assertions

## Step 3: Present Plan

```
TEST MODERNIZATION PLAN
═══════════════════════

Current State:
  Framework: [JUnit 4 / JUnit 5]
  Test Count: [N] test files, [M] test methods
  Coverage:   [X%] / [no coverage tool]
  Integration: [Testcontainers / @SpringBootTest / manual]

Proposed Changes:

Phase 1 — JUnit 5 Migration [if applicable]
  [ ] Add junit-jupiter dependencies
  [ ] Migrate [N] test files (import changes, annotation changes)
  [ ] Remove junit:junit dependency
  [ ] Update Mockito runner → extension

Phase 2 — Improve Integration Tests
  [ ] Add Testcontainers dependency
  [ ] Create reusable test container configs
  [ ] Migrate manual DB setup to Testcontainers

Phase 3 — Code Coverage
  [ ] Add JaCoCo plugin
  [ ] Configure coverage threshold (e.g., 70%)
  [ ] Add coverage report generation

Phase 4 — Architecture Tests [optional]
  [ ] Add ArchUnit dependency
  [ ] Create architecture rules

Phase 5 — Assertion Library [optional]
  [ ] Add AssertJ dependency
  [ ] Migrate key assertions to AssertJ style
```

**Wait for approval. User can select which phases to apply.**

## Step 4: Execute

Apply one phase at a time. Run the full test suite after each phase to confirm everything still passes.
