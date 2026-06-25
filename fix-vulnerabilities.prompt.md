---
description: 'Scan for CVEs and security vulnerabilities in dependencies, prioritize by severity, and fix them'
mode: agent
---

# Fix Security Vulnerabilities

Scan this project for known security vulnerabilities (CVEs) in dependencies and fix them.

## Step 1: Identify Vulnerabilities

1. Read `pom.xml` / `build.gradle` — extract ALL dependencies with versions
2. For each dependency, check if it has known CVEs by searching for:
   `[groupId]:[artifactId]:[version] CVE` or `[library-name] security vulnerability`
3. Pay special attention to commonly vulnerable libraries:
   - Jackson (databind)
   - Log4j / Logback
   - Spring Framework / Spring Security
   - Apache Commons
   - Netty
   - SnakeYAML
   - Tomcat (embedded)
   - Hibernate
   - H2 Database

## Step 2: Classify by Severity

```
VULNERABILITY REPORT
════════════════════

🔴 CRITICAL (Exploit available, immediate action needed)
  [CVE-ID] [library:version] — [brief description]
  Fix: Upgrade to [version]

🟠 HIGH (Serious vulnerability)
  [CVE-ID] [library:version] — [brief description]
  Fix: Upgrade to [version]

🟡 MEDIUM (Moderate risk)
  [CVE-ID] [library:version] — [brief description]
  Fix: Upgrade to [version]

🟢 LOW (Minor risk)
  [CVE-ID] [library:version] — [brief description]
  Fix: Upgrade to [version]
```

## Step 3: Check Fix Compatibility

For each fix:
- Does the patched version work with our Spring Boot version?
- Does it work with our Java version?
- Is it a BOM-managed dependency (will Spring Boot BOM update fix it)?
- Are there breaking API changes in the patched version?

## Step 4: Present Fix Plan

```
VULNERABILITY FIX PLAN
══════════════════════

Priority 1 — Critical/High (Apply immediately)
  [ ] [library] [old-ver] → [new-ver] (fixes CVE-XXXX)
      Impact: [none / minor code changes needed / major changes needed]
  
Priority 2 — Medium (Apply soon)
  [ ] [library] [old-ver] → [new-ver] (fixes CVE-YYYY)
      Impact: [describe]

Priority 3 — Low (Schedule for next sprint)
  [ ] [library] [old-ver] → [new-ver] (fixes CVE-ZZZZ)

⚠️ CANNOT FIX WITHOUT LARGER CHANGES
  [ ] [library CVE] — requires Spring Boot upgrade to 3.x
      Options: (A) Upgrade Spring Boot (B) Apply workaround (C) Accept risk
```

**Wait for approval.**

## Step 5: Execute Fixes

- Apply Critical/High fixes first
- Build and test after each fix
- For fixes requiring code changes, show the diff before applying

## Step 6: Recommend Preventive Measures

After fixing, suggest:
- Add OWASP dependency-check plugin to build
- Add Dependabot / Renovate for automated updates
- Add dependency scanning to CI/CD pipeline
- Regular quarterly dependency review cadence
