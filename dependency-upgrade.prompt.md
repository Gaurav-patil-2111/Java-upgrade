---
description: 'Upgrade all or specific dependencies — check for outdated versions, compatibility, and breaking changes'
mode: agent
---

# Dependency Upgrade

Upgrade dependencies in this Java project.

## Step 1: Scan All Dependencies

1. Read `pom.xml` / `build.gradle` — list ALL direct dependencies with current versions
2. Identify the Spring Boot BOM version (if managed)
3. Note which dependencies are BOM-managed vs explicitly versioned

## Step 2: Check What's Outdated

For each dependency, determine:
- Current version
- Latest stable version
- Is it a major, minor, or patch update?
- Any known breaking changes?

Organize into categories:

```
DEPENDENCY UPGRADE REPORT
═════════════════════════

🔴 SECURITY PATCHES (upgrade immediately)
  [dependency] [current] → [target] (CVE-XXXX-YYYY)

🟠 MAJOR UPGRADES (may have breaking changes)
  [dependency] [current] → [target]

🟡 MINOR UPGRADES (new features, backward-compatible)
  [dependency] [current] → [target]

🟢 PATCH UPGRADES (bug fixes only)
  [dependency] [current] → [target]

✅ UP-TO-DATE
  [dependency] [version]
```

## Step 3: Check Compatibility

Before upgrading, verify:
- Does the new version work with our Java version?
- Does the new version work with our Spring Boot version?
- Are there any transitive dependency conflicts?
- For BOM-managed deps: does upgrading one break alignment?

## Step 4: Present Plan

Group upgrades by risk and let the user choose:

```
RECOMMENDED UPGRADE PLAN
═════════════════════════

Batch 1 — Safe (Patch upgrades, no breaking changes)
  [ ] [dep1] x.y.z → x.y.w
  [ ] [dep2] a.b.c → a.b.d

Batch 2 — Moderate (Minor upgrades)
  [ ] [dep3] x.y.z → x.w.0
  [ ] [dep4] a.b.c → a.d.0

Batch 3 — Careful (Major upgrades, may need code changes)
  [ ] [dep5] x.y.z → y.0.0 — BREAKING: [describe what changed]
```

**Wait for approval.**

## Step 5: Execute

- Apply one batch at a time
- Build and test after each batch
- If a specific dependency causes test failures, isolate and fix

## Common Library Swaps to Recommend

If you spot these, suggest the modern replacement:

| Old Library | Recommended Replacement | Why |
|---|---|---|
| Swagger/SpringFox | SpringDoc OpenAPI | SpringFox unmaintained |
| Hystrix | Resilience4j | Netflix OSS EOL |
| Ribbon | Spring Cloud LoadBalancer | Netflix OSS EOL |
| Zuul 1 | Spring Cloud Gateway | Netflix OSS EOL |
| RestTemplate | RestClient or WebClient | RestTemplate in maintenance mode |
| Joda-Time | java.time (built-in) | No external dep needed |
| Apache HttpClient | java.net.http.HttpClient | Built-in since Java 11 |
| Log4j 1.x | Logback or Log4j 2 | EOL, security vulnerabilities |
| Gson | Jackson | Spring default, more features |
| ModelMapper | MapStruct | Compile-time, type-safe, faster |
| JUnit 4 | JUnit 5 | Modern, extensible |
| EhCache 2 | Caffeine or EhCache 3 | Better performance |
