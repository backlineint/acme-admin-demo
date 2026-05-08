# Adopt React Context Providers for Cross-Cutting External API Integration: External Integrations That

Status: proposed
Date: 2024-01-15
Deciders: Detection Pipeline (automated)

## Activation

This ADR is ACTIVE for all React-based frontend applications that integrate with external APIs or third-party services requiring global state management.

## Context

- The application requires integration with external APIs and third-party services (fonts, themes, data sources) that need to be accessible throughout the component tree
- React Context API provides a mechanism to share values between components without prop drilling, making it ideal for managing external API configurations and state
- Multiple components across different levels of the component hierarchy need access to external API data and configuration (font-provider, theme-provider, data-table components)
- The pattern was detected in 3 files with 90% confidence, indicating consistent architectural approach to external API integration
- Cross-cutting concerns like theming and font loading require centralized management to ensure consistency and avoid redundant API calls

## Problem Statement

Applications integrating with external APIs face challenges in distributing API responses, configurations, and state across deeply nested component trees. Prop drilling becomes unwieldy, and direct API calls from individual components lead to redundant requests, inconsistent state, and poor performance. A standardized approach is needed to centralize external API integration while maintaining clean component interfaces.

## Decision

1. MUST: External API integrations that provide cross-cutting functionality MUST be implemented using React Context Providers

## Policy Block

- MUST External API integrations that provide cross-cutting functionality MUST be implemented using React Context Providers

In scope:
- React-based frontend applications
- External API integrations requiring global or feature-level state
- Third-party service integrations (fonts, themes, analytics, feature flags)
- Cross-cutting concerns that span multiple components
- API configurations that need to be shared across the component tree

Out of scope:
- Component-local API calls that are not shared
- Backend service-to-service API integrations
- One-time API calls that do not require state management
- Non-React frontend frameworks
- Internal API calls to the application's own backend (unless they provide cross-cutting state)

Exceptions:
- EX-001: A component needs to make a one-off external API call that is not used by any other component and does not represent shared state
- EX-002: Performance profiling demonstrates that Context Provider re-renders are causing measurable performance degradation

## Rationale

- Pattern detected across 3 files (font-provider, theme-provider, faceted-filter) with 90% confidence, indicating established architectural practice
- React Context API is the idiomatic React solution for sharing data across component trees without prop drilling
- Centralizing external API integration in Context Providers enables caching, error handling, and loading state management in a single location
- The pattern aligns with React best practices and provides clear separation between API integration logic and presentation components

## Consequences

Positive:
- Eliminates prop drilling for external API data, resulting in cleaner component interfaces
- Reduces redundant external API calls through centralized state management and caching
- Provides consistent error handling and loading states for external API integrations
- Improves testability by allowing easy mocking of external API providers in component tests
- Establishes clear architectural pattern that new developers can follow

Negative:
- Adds abstraction layer that may be unnecessary for simple, one-off API calls
- Can lead to over-use of Context, potentially causing unnecessary re-renders if not optimized
- Requires developers to understand React Context API and custom hooks pattern
- May increase initial development time for simple features that could use direct API calls

## Alternatives

- Use prop drilling to pass external API data through component hierarchy (rejected)
  Rejected because: Prop drilling becomes unmaintainable in deep component trees and couples intermediate components to data they don't use
  When valid: Only valid for shallow component hierarchies (2-3 levels) with stable prop interfaces
- Use global state management library (Redux, Zustand, Jotai) for all external API integration (rejected)
  Rejected because: Adds unnecessary dependency and complexity when React Context API is sufficient for most external API integration use cases
  When valid: Valid when application already uses global state library and external API state needs complex middleware or time-travel debugging
- Allow each component to make direct external API calls as needed (rejected)
  Rejected because: Leads to redundant API calls, inconsistent state, and scattered API integration logic that is difficult to maintain
  When valid: Valid only for truly isolated, one-off API calls that are not shared across components

## Risks

- Overuse of Context Providers may lead to performance issues due to unnecessary re-renders when context values change
  Mitigation: Use React.memo, useMemo, and useCallback to optimize re-renders. Split contexts by update frequency. Implement context selectors if needed.
  Owner: Frontend engineering team
- Developers may create too many granular Context Providers, leading to provider wrapper hell in the component tree
  Mitigation: Establish guidelines for when to create new providers vs. extending existing ones. Review provider architecture in code reviews.
  Owner: Tech leads and architects
- External API failures may not be handled consistently across different Context Providers
  Mitigation: Create a base provider pattern or utility that enforces consistent error handling, retry logic, and fallback behavior
  Owner: Frontend engineering team

## Implementation Notes

- Create a context/ directory at src/context/ to house all Context Provider implementations
- Follow naming convention: [Feature]Provider.tsx for the provider component and use[Feature] for the consumer hook
- Include TypeScript types for context values to ensure type safety across consuming components
- Implement error boundaries around Context Providers to gracefully handle external API failures
- Document each Context Provider's purpose, the external API it integrates with, and usage examples in code comments or README

## Continuation Context


Verify commands:
- grep -r 'createContext\|React.createContext' src/context/ --include='*.tsx' --include='*.ts'
- find src/context -name '*-provider.tsx' -o -name '*Provider.tsx' | wc -l
- grep -r 'export.*use[A-Z]' src/context/ --include='*.tsx' --include='*.ts'

Accept when:
- All external API integrations providing cross-cutting functionality are implemented using React Context Providers in src/context/
- Each Context Provider exposes a corresponding custom hook for consumption
- No direct external API calls exist in components where Context Provider pattern is applicable

## Enforcement

- Verified by: Code review checklist includes verification of Context Provider usage for external API integrations
- Verified by: ESLint custom rules to detect direct external API calls in components when Context Provider exists
- Verified by: Architecture review for new external API integrations
- Violation handling: Code review feedback requesting refactor to use Context Provider pattern
- Violation handling: ESLint warnings/errors for direct API calls when provider exists
- Violation handling: Architecture review required for exceptions to the pattern
- Exception process: Developer documents rationale for exception in code comments or PR description
- Exception process: Tech lead reviews and approves exception based on valid use case (see policy exceptions)
- Exception process: Exception is tracked in architecture decision log if it represents a new pattern