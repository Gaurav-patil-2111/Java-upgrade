---
description: 'Overall project health check — outdated deps, CVEs, test coverage, code quality, tech debt, and modernization score'
mode: agent
---

# Health Check

Run a comprehensive health check on this Java project and produce a report card.

## What to Check

Scan the project and evaluate each dimension:

### 1. Java Version Health
- What version is configured?
- Is it an LTS version? (8, 11, 17, 21, 25)
- Is it still supported by the vendor?
- Grade: 🟢 Current LTS | 🟡 Previous LTS | 🟠 Old LTS | 🔴 EOL

### 2. Framework Version Health
- What framework and version?
- Is it the latest major? Latest minor?
- Are there known security issues in this version?
- Grade: 🟢 Latest | 🟡 One minor behind | 🟠 One major behind | 🔴 Multiple majors behind / EOL

### 3. Dependency Health
- How many direct dependencies?
- How many are outdated?
- How many have known CVEs?
- Any EOL / unmaintained libraries?
- Grade: 🟢 <10% outdated | 🟡 10-30% | 🟠 30-50% | 🔴 >50%

### 4. Security Health
- Is authentication configured?
- Are secrets externalized?
- Any hardcoded credentials in source?
- Grade: 🟢 Secure | 🟡 Some gaps | 🟠 Significant gaps | 🔴 No security

### 5. Testing Health
- Test framework version (JUnit 5?)
- Approximate test coverage
- Integration tests present?
- Grade: 🟢 >80% coverage + integration | 🟡 50-80% | 🟠 20-50% | 🔴 <20% or no tests

### 6. Code Quality
- Static analysis configured?
- Code formatting rules?
- Architecture tests?
- Grade: 🟢 Full tooling | 🟡 Some tooling | 🟠 Minimal | 🔴 None

### 7. DevOps Maturity
- CI/CD pipeline present?
- Containerized?
- Automated deployment?
- Grade: 🟢 Full CI/CD + containers | 🟡 CI only | 🟠 Manual + Docker | 🔴 Fully manual

### 8. Observability
- Metrics configured?
- Logging structured?
- Distributed tracing?
- Health endpoints?
- Grade: 🟢 Full stack | 🟡 Basic metrics | 🟠 Just logging | 🔴 None

### 9. Documentation
- README present and useful?
- API docs (OpenAPI/Swagger)?
- Architecture docs?
- Grade: 🟢 Comprehensive | 🟡 Basic README | 🟠 Minimal | 🔴 None

## Output Format

```
PROJECT HEALTH REPORT CARD
══════════════════════════

Project: [name]
Scanned: [date]

DIMENSION              GRADE   STATUS
─────────────────────────────────────
Java Version           [🟢/🟡/🟠/🔴]   [Java 21 LTS ✓]
Framework Version      [🟢/🟡/🟠/🔴]   [Spring Boot 3.2.4]
Dependency Health      [🟢/🟡/🟠/🔴]   [3 CVEs, 12 outdated]
Security               [🟢/🟡/🟠/🔴]   [JWT auth, secrets in env vars]
Testing                [🟢/🟡/🟠/🔴]   [JUnit 5, ~72% coverage]
Code Quality           [🟢/🟡/🟠/🔴]   [No static analysis]
DevOps                 [🟢/🟡/🟠/🔴]   [GitHub Actions + Docker]
Observability          [🟢/🟡/🟠/🔴]   [Actuator only, no tracing]
Documentation          [🟢/🟡/🟠/🔴]   [README + Swagger UI]

OVERALL SCORE: [X/9 green] — [rating]

TOP 3 RECOMMENDED ACTIONS:
1. [Highest impact improvement]
2. [Second highest]
3. [Third highest]

Would you like me to address any of these?
```
