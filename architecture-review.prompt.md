---
description: 'Review architecture — analyze current structure, identify issues, recommend improvements (monolith→microservices, hexagonal, CQRS, etc.)'
mode: agent
---

# Architecture Review

Analyze the current architecture of this Java application and provide recommendations.

## Step 1: Scan Architecture

1. **Package Structure**: Map all top-level packages and their contents
2. **Module Structure**: Single module or multi-module?
3. **Layer Analysis**: Identify controllers, services, repositories, models
4. **Dependency Flow**: Do layers depend on each other correctly? (controller→service→repository)
5. **External Integrations**: HTTP clients, message consumers/producers, external APIs
6. **Database Access**: Single DB or multiple? Shared or isolated?
7. **Cross-Cutting Concerns**: How are security, logging, error handling implemented?
8. **Domain Model**: Anemic or rich domain model?

## Step 2: Classify Architecture

| Style | Indicators |
|---|---|
| **Package-by-Layer** | `controller/`, `service/`, `repository/`, `model/` at top level |
| **Package-by-Feature** | `order/`, `payment/`, `user/` at top level with sub-layers |
| **Hexagonal** | `domain/`, `port/`, `adapter/`, `infrastructure/` |
| **Clean Architecture** | `entity/`, `usecase/`, `interface/`, `framework/` |
| **Monolith** | Large codebase, single deployable, multiple features |
| **Microservice** | Small codebase, single bounded context |
| **Modular Monolith** | Single deployable but strict module boundaries |

## Step 3: Identify Issues

Common architecture problems to look for:
- **Circular dependencies** between packages
- **God classes** (services with too many methods/responsibilities)
- **Anemic domain model** (all logic in services, entities are just data)
- **Leaky abstractions** (controllers calling repositories directly)
- **Tight coupling** to external systems (no abstraction layer)
- **Shared database** concerns (if microservices)
- **Missing domain boundaries** (everything interconnected)
- **Configuration sprawl** (config scattered everywhere)

## Step 4: Present Findings & Recommendations

```
ARCHITECTURE REVIEW
═══════════════════

Current Architecture: [Package-by-Layer Monolith]
Modules: [1 / N]
Bounded Contexts Identified: [Order, Payment, User, ...]
Total Controllers: [N]
Total Services: [N]
Total Repositories: [N]

STRENGTHS
  ✅ [what's done well]
  ✅ [what's done well]

ISSUES
  🟠 [issue] — [where] — [impact]
  🟠 [issue] — [where] — [impact]
  🟡 [issue] — [where] — [impact]

RECOMMENDATIONS (prioritized)
  1. [recommendation + rationale]
  2. [recommendation + rationale]
  3. [recommendation + rationale]

MIGRATION PATHS (if applicable)
  Option A: [e.g., Package-by-Layer → Package-by-Feature]
    Effort: [Low/Medium/High]
    Risk: [Low/Medium/High]
    Benefit: [what you gain]
    
  Option B: [e.g., Monolith → Modular Monolith]
    Effort: [Medium/High]
    Risk: [Medium]
    Benefit: [what you gain]

Would you like me to plan or execute any of these changes?
```

## Important Notes

- Architecture changes are HIGH RISK — always plan, never auto-execute
- Recommend incremental migration (strangler fig, branch by abstraction)
- If recommending microservices, warn about operational complexity
- Consider team size and expertise in recommendations
- Modular monolith is often a better first step than microservices
