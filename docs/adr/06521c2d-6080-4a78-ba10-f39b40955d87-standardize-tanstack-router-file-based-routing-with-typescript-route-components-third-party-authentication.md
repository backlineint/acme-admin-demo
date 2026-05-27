# Standardize TanStack Router File-Based Routing with TypeScript Route Components: Third Party Authentication

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The codebase demonstrates consistent use of file-based routing patterns across 30 files with 87.58% confidence, indicating a deliberate architectural choice for route organization
- Routes are organized into logical groupings including authentication flows (sign-in, sign-up, forgot-password), error pages (401, 403, 404), and authenticated sections (settings, help-center), suggesting a need for clear separation of concerns
- The pattern includes integration with Clerk authentication library for auth routes and custom error handling routes, indicating requirements for both third-party integration and custom route handling
- TypeScript/TSX file extensions across all route files indicate type-safe route component development is a priority
- The routing structure uses parenthetical grouping syntax (e.g., '(auth)', '(errors)') and underscore prefixes (e.g., '_authenticated') suggesting layout route organization and route grouping conventions

## Problem Statement

Without a standardized approach to routing architecture, applications risk inconsistent route organization, unclear navigation hierarchies, difficulty maintaining authentication boundaries, and challenges in scaling route structures as the application grows. The codebase needs a clear decision on how routes should be structured, organized, and implemented to ensure maintainability and developer productivity.

## Decision

1. SHOULD: Third-party authentication provider routes (e.g., Clerk) SHOULD be organized in dedicated subdirectories (e.g., 'clerk/(auth)') to isolate provider-specific implementations

## Policy Block

- SHOULD Third-party authentication provider routes (e.g., Clerk) SHOULD be organized in dedicated subdirectories (e.g., 'clerk/(auth)') to isolate provider-specific implementations

In scope:
- All user-facing route components in the src/routes directory
- Authentication and authorization route boundaries
- Error page routes and error handling flows
- Feature-specific route implementations that integrate with data layers
- Third-party authentication provider route integrations

Out of scope:
- API route handlers or backend endpoints
- Static asset routing
- Server-side rendering configuration
- Build-time route generation or dynamic route creation outside the file-based system
- Non-route TypeScript modules and utilities

Exceptions:
- EXC-001: Legacy routes exist that predate this standard and require gradual migration
- EXC-002: Third-party library requires non-standard route structure for integration

## Rationale

- The pattern appears in 30 files with 87.58% confidence, demonstrating strong consistency and intentional architectural design across the codebase
- File-based routing provides intuitive mapping between file structure and URL paths, reducing cognitive load and improving developer experience
- Clear separation of authentication boundaries through directory structure (_authenticated prefix) enables easier enforcement of access control and security policies
- Parenthetical grouping syntax allows logical organization without URL pollution, supporting clean URLs while maintaining organized file structure
- TypeScript integration ensures type-safe route parameters, search params, and navigation, reducing runtime errors and improving refactoring confidence

## Consequences

Positive:
- Developers can quickly locate route components by following the URL structure, improving navigation and reducing time to find relevant code
- Authentication boundaries are explicit and enforced through directory structure, making security reviews more straightforward
- New routes follow predictable patterns, reducing onboarding time for new team members
- Type safety across route definitions prevents common routing errors and improves IDE autocomplete support
- Route grouping enables shared layouts and middleware without affecting URL structure

Negative:
- File-based routing can become complex with deeply nested route structures, potentially leading to long file paths
- Refactoring URL structures requires moving files, which may affect git history and blame information
- Special syntax (parentheses, underscores) requires documentation and team training to understand conventions
- Migration from existing non-file-based routing systems requires significant refactoring effort
- Dynamic route patterns may be less obvious than programmatic route definitions for complex scenarios

## Alternatives

- Programmatic route configuration using a centralized routes configuration file (rejected)
  Rejected because: Centralized configuration creates a single point of change that grows with application size, reduces colocation of route logic with components, and loses the intuitive file-path-to-URL mapping that improves developer experience
  When valid: May be appropriate for very small applications with fewer than 10 routes or when routes are generated dynamically from external data sources
- Component-level route definitions using decorators or inline route configuration (rejected)
  Rejected because: Decorators are not yet stable in TypeScript for this use case, inline configuration scatters route structure across components making it difficult to visualize the complete routing hierarchy, and lacks the clear file system organization benefits
  When valid: Could be considered for micro-frontends where each module owns its complete routing independently
- Hybrid approach with file-based routing for simple routes and programmatic configuration for complex dynamic routes (deferred)
  Rejected because: Not rejected, but deferred for future consideration as application complexity grows
  When valid: Should be reconsidered if the application requires extensive dynamic route generation based on user permissions or data-driven route structures

## Risks

- Team members unfamiliar with file-based routing conventions may create routes in incorrect locations or misuse grouping syntax
  Mitigation: Provide comprehensive documentation with examples, create route scaffolding CLI tools, implement linting rules to validate route structure, and conduct team training sessions
  Owner: Engineering team lead
- Large-scale URL restructuring becomes expensive due to tight coupling between file paths and URLs
  Mitigation: Implement redirect management strategy, use route aliases where appropriate, plan URL structure carefully upfront, and maintain URL stability as a design principle
  Owner: Product and engineering teams
- Third-party routing libraries may change conventions or deprecate file-based routing patterns
  Mitigation: Monitor TanStack Router release notes and community discussions, maintain abstraction layer for route definitions, and evaluate migration paths during major version upgrades
  Owner: Architecture team

## Implementation Notes

- Create route scaffolding templates or CLI commands (e.g., 'npm run create-route') to ensure consistent route file structure and boilerplate
- Document the routing conventions in the project README with visual diagrams showing the directory structure and corresponding URL patterns
- Implement ESLint rules or custom linting to validate route file locations match expected patterns (e.g., auth routes in (auth) directory)
- Set up IDE snippets for common route patterns (authenticated routes, error pages, etc.) to accelerate development
- Create a route map visualization tool or documentation generator that automatically builds a sitemap from the file structure for reference

## Continuation Context


Verify commands:
- find src/routes -name '*.tsx' -type f | grep -E '(\(auth\)|\(errors\)|_authenticated)' | wc -l
- grep -r 'export default' src/routes --include='*.tsx' | wc -l
- find src/routes -name '*.js' -o -name '*.jsx' | wc -l

Accept when:
- All route files in src/routes directory use .tsx extension (verify command 3 returns 0)
- At least 80% of routes follow the grouping conventions for auth, errors, and authenticated sections (verify command 1 shows significant count)
- All route files export a default component (verify command 2 count matches total route files)
- Route structure documentation exists and is up-to-date with current conventions

## Enforcement

- Verified by: Automated CI checks using grep and find commands to validate route file structure and naming conventions
- Verified by: Code review checklist includes verification of route placement in correct directory groups
- Verified by: ESLint custom rules validate route file patterns and TypeScript usage
- Verified by: Pre-commit hooks check for route files outside approved directory structure
- Violation handling: CI pipeline fails if route files are found with .js or .jsx extensions instead of .tsx
- Violation handling: Pull requests with incorrectly placed routes are flagged with automated comments indicating the correct location
- Violation handling: Quarterly audits identify non-compliant routes and create remediation tickets
- Violation handling: New route violations block merge until corrected or exception is approved
- Exception process: Developer creates exception request documenting the specific route, reason for non-compliance, and proposed alternative approach
- Exception process: Tech lead reviews exception request within 2 business days
- Exception process: If approved, exception is documented in route file header comment with EXC-ID reference and expiration date if applicable
- Exception process: All exceptions are logged in architecture decision log and reviewed quarterly for potential pattern updates