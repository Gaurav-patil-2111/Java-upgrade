---
description: 'Add database migration tool — set up Flyway or Liquibase with initial migration from existing schema'
mode: agent
---

# Database Migration Setup

Add a database migration tool (Flyway or Liquibase) to this project.

## Step 1: Scan Current State

1. Check for existing migration tool (Flyway, Liquibase)
2. Check ORM (JPA/Hibernate) — is `ddl-auto` set to `update` or `create`?
3. Check database type (PostgreSQL, MySQL, H2, etc.)
4. Check for existing `schema.sql` or `data.sql` files
5. Check Spring Boot version (affects auto-configuration)

## Step 2: Recommend Tool

| Tool | Best For |
|---|---|
| **Flyway** | SQL-first approach, simpler, widely adopted |
| **Liquibase** | XML/YAML/JSON changelogs, rollback support, diff tools |

Default recommendation: **Flyway** (simpler, Spring Boot default auto-config)

## Step 3: Present Plan

```
DATABASE MIGRATION SETUP
═════════════════════════

Tool: [Flyway / Liquibase]
Database: [PostgreSQL / MySQL / etc.]

Phase 1 — Add Dependency
  [ ] Add flyway-core (+ flyway-database-postgresql if PG)
  [ ] Spring Boot auto-configures Flyway automatically

Phase 2 — Create Initial Migration
  [ ] Generate V1__initial_schema.sql from current entity definitions
  [ ] Place in src/main/resources/db/migration/

Phase 3 — Update JPA Configuration
  [ ] Change spring.jpa.hibernate.ddl-auto from 'update' to 'validate'
      (Flyway manages schema, Hibernate only validates)

Phase 4 — Test
  [ ] Run application — Flyway applies migration on startup
  [ ] Verify schema matches entity definitions
  [ ] Integration tests work with migration

Phase 5 — Documentation
  [ ] Add migration naming convention to README:
      V{version}__{description}.sql
      e.g., V1__initial_schema.sql
           V2__add_user_email_index.sql
```

**Wait for approval.**

## Migration Naming Convention (Flyway)

```
src/main/resources/db/migration/
├── V1__initial_schema.sql           # First migration
├── V2__add_orders_table.sql         # Feature addition
├── V3__add_index_on_email.sql       # Performance
├── V4__alter_user_add_phone.sql     # Schema change
└── R__refresh_views.sql             # Repeatable migration
```

## Key Configuration

```yaml
spring:
  jpa:
    hibernate:
      ddl-auto: validate    # Changed from 'update' — Flyway manages schema now
  flyway:
    enabled: true
    baseline-on-migrate: true   # For existing databases
    locations: classpath:db/migration
```

## Step 4: Execute

1. Add dependency
2. Generate initial migration SQL from current entities
3. Update JPA config
4. Test that app starts and schema is correct
