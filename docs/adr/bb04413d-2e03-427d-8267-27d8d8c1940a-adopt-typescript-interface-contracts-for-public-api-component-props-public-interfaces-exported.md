# Adopt TypeScript Interface Contracts for Public API Component Props: Public Interfaces Exported

Status: proposed
Date: 2024-01-15
Deciders: Detection Pipeline (automated)

## Activation

This ADR is ACTIVE for all public-facing component APIs, external integration points, and reusable UI components exposed through the component library.

## Context

- The codebase contains 115 files implementing public-facing component APIs with explicit TypeScript interface contracts, indicating a systematic approach to API design
- Components span multiple feature domains including errors, authentication, dashboards, data tables, and chat functionality, requiring consistent contract definitions
- External consumers and internal teams need stable, well-documented interfaces to integrate with shared components without breaking changes
- TypeScript's type system provides compile-time guarantees for API contracts, reducing runtime errors and improving developer experience
- The pattern shows consistent usage across UI components, brand icons, custom assets, and feature modules, suggesting an established architectural standard

## Problem Statement

Without explicit, strongly-typed interface contracts for public component APIs, consumers face unclear expectations about required props, optional parameters, and return types. This leads to integration errors, breaking changes during refactoring, poor IDE support, and increased maintenance burden. A standardized approach to defining and enforcing API contracts is needed to ensure stability, discoverability, and type safety across all public-facing components.

## Decision

1. MUST: Public API interfaces MUST be exported from their module to enable external consumption

## Policy Block

- MUST Public API interfaces MUST be exported from their module to enable external consumption

In scope:
- All React components exported from feature modules
- Reusable UI components in the component library
- Custom icon components and brand assets
- Data table components and their configuration interfaces
- Error boundary and error display components
- Authentication and user management UI components
- Dashboard and analytics visualization components

Out of scope:
- Internal helper functions not exposed as public APIs
- Private component implementations used only within a single file
- Test utilities and mock components
- Build scripts and configuration files
- Third-party library type definitions (unless wrapping/extending them)

Exceptions:
- EXC-001: Legacy components undergoing gradual migration to TypeScript
- EXC-002: Wrapper components that intentionally pass through all props using spread operators

## Rationale

- Pattern detected across 115 files with 89.96% confidence indicates this is an established, successful practice in the codebase
- TypeScript interface contracts provide compile-time type checking, catching integration errors before runtime and reducing production bugs
- Explicit interfaces serve as living documentation, improving IDE autocomplete and reducing onboarding time for new developers
- Strong typing enables safer refactoring with confidence that breaking changes will be caught by the type checker across all consumers

## Consequences

Positive:
- Compile-time type safety prevents prop mismatches and reduces runtime errors in production
- Enhanced developer experience with IDE autocomplete, inline documentation, and type hints
- Safer refactoring with automatic detection of breaking changes across all component consumers
- Self-documenting code reduces need for separate API documentation and improves maintainability
- Easier onboarding for new team members who can discover component APIs through type definitions

Negative:
- Initial development overhead to define and maintain interface contracts for all components
- Learning curve for developers unfamiliar with TypeScript's advanced type features
- Potential for over-engineering with excessively complex type definitions
- Migration effort required for existing JavaScript components to adopt TypeScript interfaces

## Alternatives

- Use PropTypes runtime validation instead of TypeScript interfaces (rejected)
  Rejected because: PropTypes only provide runtime validation without compile-time checking, offer weaker IDE support, and are being deprecated in favor of TypeScript in modern React development
  When valid: May be appropriate for legacy codebases not yet migrated to TypeScript
- Rely on implicit typing and inference without explicit interface declarations (rejected)
  Rejected because: Implicit typing provides no contract guarantees for external consumers, makes refactoring dangerous, and eliminates the documentation benefits of explicit interfaces
  When valid: Only acceptable for private internal components with single-file scope
- Generate interfaces automatically from JSDoc comments using tooling (deferred)
  Rejected because: Not rejected but deferred pending evaluation of tooling maturity and team workflow integration
  When valid: Could complement explicit interfaces for components with complex documentation needs

## Risks

- Type definitions may become stale or inaccurate if not maintained alongside implementation changes
  Mitigation: Enforce strict TypeScript compiler settings (strict: true) and require type checking in CI pipeline to catch mismatches
  Owner: Engineering team
- Overly complex type definitions may reduce code readability and increase maintenance burden
  Mitigation: Establish type complexity guidelines in code review process and prefer simple, clear interfaces over clever type gymnastics
  Owner: Tech leads and code reviewers
- Breaking interface changes may impact multiple consumers across the codebase
  Mitigation: Use semantic versioning for component library releases, provide deprecation warnings, and maintain changelog of interface changes
  Owner: Component library maintainers

## Implementation Notes

- Start by defining interfaces for the most frequently used components to maximize impact
- Use consistent naming conventions: ComponentNameProps for props interfaces (e.g., ButtonProps, DataTableProps)
- Co-locate interface definitions with component implementations in the same file for easier maintenance
- Export interfaces from index files to create a clear public API surface for each module
- Leverage TypeScript utility types (Pick, Omit, Partial) to compose interfaces and reduce duplication
- Consider using interface extension for component variants that share common props

## Continuation Context


Verify commands:
- grep -r "export interface.*Props" src/components src/features --include="*.tsx" --include="*.ts" | wc -l
- npx tsc --noEmit --strict && echo 'Type checking passed'
- grep -r "React.FC<" src/components src/features --include="*.tsx" | grep -v "React.FC<.*Props>" && echo 'Found components without typed props' || echo 'All FC components have typed props'

Accept when:
- All public components in src/components and src/features directories have exported TypeScript interface definitions for their props
- TypeScript strict mode compilation passes without errors for all component files
- Code review checklist includes verification that new components include proper interface contracts

## Enforcement

- Verified by: TypeScript compiler in strict mode as part of CI/CD pipeline
- Verified by: ESLint rules enforcing explicit prop types (e.g., @typescript-eslint/explicit-module-boundary-types)
- Verified by: Code review checklist requiring interface definitions for all new public components
- Verified by: Automated PR checks verifying type coverage metrics
- Violation handling: CI build fails if TypeScript compilation errors are present
- Violation handling: PR reviews blocked until proper interface contracts are added
- Violation handling: Automated comments on PRs identifying components missing type definitions
- Violation handling: Quarterly audits to identify and remediate components with weak typing
- Exception process: Developer documents exception rationale in code comments with EXC-XXX reference
- Exception process: Tech lead reviews and approves exception during PR review
- Exception process: Exception is logged in architectural decision log with timeline for resolution
- Exception process: Exceptions are reviewed quarterly and must be re-justified or resolved