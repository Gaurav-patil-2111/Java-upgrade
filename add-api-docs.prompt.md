---
description: 'Add API documentation — SpringDoc OpenAPI with Swagger UI, API grouping, and schema documentation'
mode: agent
---

# Add API Documentation

Add auto-generated API documentation using SpringDoc OpenAPI and Swagger UI.

## Step 1: Scan Current State

1. Check for existing API docs (SpringFox, SpringDoc, manual OpenAPI spec)
2. Count REST controllers and endpoints
3. Check for existing OpenAPI annotations (`@Operation`, `@ApiResponse`, `@Schema`)
4. Check Spring Boot version (affects SpringDoc version)

## Step 2: Present Plan

```
API DOCUMENTATION PLAN
══════════════════════

Endpoints Found: [N] controllers, [M] endpoints

Phase 1 — Add SpringDoc OpenAPI
  [ ] Add springdoc-openapi-starter-webmvc-ui dependency
      (or springdoc-openapi-starter-webflux-ui for reactive)
  [ ] Configure in application.yml:
      - API title, description, version
      - Server URLs
      - Security scheme (if auth is used)

Phase 2 — Access Swagger UI
  [ ] Swagger UI: http://localhost:8080/swagger-ui.html
  [ ] OpenAPI spec: http://localhost:8080/v3/api-docs

Phase 3 — Enhance Documentation (optional)
  [ ] Add @Operation annotations to key endpoints
  [ ] Add @Schema annotations to DTOs
  [ ] Add @ApiResponse for different status codes
  [ ] Group APIs by tag (@Tag)
  [ ] Add examples to request/response bodies

Phase 4 — Security Documentation (if applicable)
  [ ] Configure OpenAPI security scheme (JWT/OAuth2)
  [ ] Add @SecurityRequirement to protected endpoints
```

**Wait for approval.**

## Key Dependencies

### Spring Boot 3.x (Spring MVC)
```xml
<dependency>
    <groupId>org.springdoc</groupId>
    <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
    <version>2.3.0</version>
</dependency>
```

### Spring Boot 2.x (Spring MVC)
```xml
<dependency>
    <groupId>org.springdoc</groupId>
    <artifactId>springdoc-openapi-ui</artifactId>
    <version>1.8.0</version>
</dependency>
```

## Example Configuration

```yaml
springdoc:
  api-docs:
    path: /v3/api-docs
  swagger-ui:
    path: /swagger-ui.html
    tags-sorter: alpha
    operations-sorter: alpha
  info:
    title: ${spring.application.name} API
    description: API documentation
    version: 1.0.0
```

## SpringFox Migration (if applicable)

If the project uses SpringFox/Swagger 2, migrate:

| SpringFox | SpringDoc |
|---|---|
| `@Api` | `@Tag` |
| `@ApiOperation` | `@Operation` |
| `@ApiParam` | `@Parameter` |
| `@ApiModel` | `@Schema` |
| `@ApiModelProperty` | `@Schema` |
| `@ApiResponse` | `@ApiResponse` (different package) |
| Remove springfox dependencies | Add springdoc dependency |
| Remove `@EnableSwagger2` | No annotation needed (auto-configured) |
| Remove Docket bean | Use application.yml config |
