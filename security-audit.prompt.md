---
description: 'Audit security configuration — authentication, authorization, vulnerabilities, secrets, headers, and compliance'
mode: agent
---

# Security Audit

Perform a comprehensive security audit of this Java application.

## Step 1: Scan Security Configuration

1. **Dependencies**: Check for Spring Security, OAuth2 client/resource-server
2. **Security Config**: Find `SecurityFilterChain`, `WebSecurityConfigurerAdapter`, or `@EnableWebSecurity`
3. **Authentication**: How are users authenticated? (JWT, session, OAuth2, Basic)
4. **Authorization**: How are endpoints protected? (roles, permissions, method-level)
5. **Config Files**: Scan for hardcoded secrets, API keys, passwords
6. **application.yml**: Check for security-related properties
7. **CORS Config**: How is CORS configured?
8. **CSRF Config**: Is CSRF protection enabled?

## Step 2: Audit Checklist

### Authentication & Authorization
- [ ] Is authentication configured for all non-public endpoints?
- [ ] Is the auth mechanism modern (JWT/OAuth2 vs Basic/Session)?
- [ ] Are roles/permissions properly defined and enforced?
- [ ] Is method-level security used where appropriate (`@PreAuthorize`)?
- [ ] Are password hashing algorithms strong (bcrypt, Argon2)?

### Secrets Management
- [ ] Are there hardcoded passwords, API keys, or tokens in source code?
- [ ] Are there secrets in `application.yml` / `application.properties`?
- [ ] Are secrets in environment variables or a vault?
- [ ] Are database credentials externalized?

### HTTP Security Headers
- [ ] Content-Security-Policy header set?
- [ ] X-Content-Type-Options: nosniff?
- [ ] X-Frame-Options: DENY?
- [ ] Strict-Transport-Security (HSTS)?
- [ ] X-XSS-Protection?

### Input Validation
- [ ] Bean Validation (`@Valid`, `@NotNull`, `@Size`) on request DTOs?
- [ ] SQL injection prevention (parameterized queries)?
- [ ] XSS prevention (output encoding)?

### CORS
- [ ] Are allowed origins properly restrictive (not `*` in production)?
- [ ] Are allowed methods limited to what's needed?
- [ ] Are credentials properly handled?

### CSRF
- [ ] Is CSRF protection enabled for browser-facing endpoints?
- [ ] Is it properly disabled only for stateless API endpoints?

### Dependency Vulnerabilities
- [ ] Run CVE scan on all dependencies
- [ ] Check for known vulnerable versions

### Logging & Audit
- [ ] Are authentication events logged (login, logout, failed attempts)?
- [ ] Are authorization failures logged?
- [ ] Is sensitive data excluded from logs (passwords, tokens, PII)?

## Step 3: Present Findings

```
SECURITY AUDIT REPORT
═════════════════════

🔴 CRITICAL
  [finding] — [location] — [recommendation]

🟠 HIGH
  [finding] — [location] — [recommendation]

🟡 MEDIUM
  [finding] — [location] — [recommendation]

🟢 LOW / INFORMATIONAL
  [finding] — [location] — [recommendation]

✅ PASSING
  [what's properly configured]

RECOMMENDED ACTIONS (prioritized):
1. [Most critical fix]
2. [Second priority]
3. ...
```

## Step 4: Fix (if user approves)

For each finding the user wants fixed:
- Show the exact change
- Apply incrementally
- Build and test after each fix
