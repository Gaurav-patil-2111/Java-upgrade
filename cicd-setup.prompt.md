---
description: 'Set up CI/CD pipeline — GitHub Actions workflow for build, test, code quality, Docker, and deployment'
mode: agent
---

# CI/CD Setup

Set up or improve the CI/CD pipeline for this Java project.

## Step 1: Scan Current State

1. Check for existing CI/CD:
   - `.github/workflows/*.yml` (GitHub Actions)
   - `Jenkinsfile` (Jenkins)
   - `.gitlab-ci.yml` (GitLab CI)
   - `.circleci/config.yml` (CircleCI)
2. Check build system (Maven/Gradle) and test setup
3. Check for Docker setup
4. Check for code quality tools (SonarQube, Checkstyle)
5. Check for deployment targets (K8s, ECS, Heroku, etc.)

## Step 2: Present Plan

```
CI/CD SETUP PLAN
════════════════

Current: [No CI/CD / Jenkins / GitHub Actions (basic)]
Target:  [GitHub Actions with full pipeline]

Pipeline Stages:
  1. 🔨 Build — Compile the project
  2. 🧪 Test — Run unit + integration tests
  3. 📊 Quality — Code coverage, static analysis
  4. 🔒 Security — Dependency vulnerability scan
  5. 🐳 Docker — Build and push container image
  6. 🚀 Deploy — Deploy to target environment

Generated Workflows:
  [ ] ci.yml — Build + Test + Quality (on every PR and push)
  [ ] release.yml — Docker build + Deploy (on tag/release)
```

**Wait for approval.**

## Step 3: Generate GitHub Actions Workflow

### ci.yml — Continuous Integration

```yaml
name: CI

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  build-and-test:
    runs-on: ubuntu-latest
    
    services:
      postgres:  # Add if project uses PostgreSQL
        image: postgres:15
        env:
          POSTGRES_DB: testdb
          POSTGRES_USER: test
          POSTGRES_PASSWORD: test
        ports:
          - 5432:5432
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5

    steps:
      - uses: actions/checkout@v4
      
      - name: Set up JDK
        uses: actions/setup-java@v4
        with:
          java-version: '21'
          distribution: 'temurin'
          cache: 'maven'  # or 'gradle'
      
      - name: Build
        run: mvn compile -B
        # or: gradle compileJava
      
      - name: Test
        run: mvn verify -B
        # or: gradle test integrationTest
      
      - name: Upload Test Results
        if: always()
        uses: actions/upload-artifact@v4
        with:
          name: test-results
          path: target/surefire-reports/
      
      - name: Code Coverage
        run: mvn jacoco:report -B
        # Uncomment to enforce threshold:
        # run: mvn jacoco:check -B
      
      - name: Upload Coverage Report
        uses: actions/upload-artifact@v4
        with:
          name: coverage-report
          path: target/site/jacoco/

  security-scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Set up JDK
        uses: actions/setup-java@v4
        with:
          java-version: '21'
          distribution: 'temurin'
          cache: 'maven'
      
      - name: Dependency Check
        run: mvn org.owasp:dependency-check-maven:check -B
        continue-on-error: true
      
      - name: Upload Security Report
        uses: actions/upload-artifact@v4
        with:
          name: dependency-check-report
          path: target/dependency-check-report.html
```

Adapt based on what was detected in the project scan:
- Add/remove service containers (PostgreSQL, Redis, Kafka)
- Switch between Maven and Gradle commands
- Add Docker build step if Dockerfile exists
- Add deployment step if deployment target is known
- Add SonarQube step if configured

## Step 4: Execute

Create the workflow files, verify they're syntactically correct, and commit.
Recommend the user pushes and watches the first pipeline run.
