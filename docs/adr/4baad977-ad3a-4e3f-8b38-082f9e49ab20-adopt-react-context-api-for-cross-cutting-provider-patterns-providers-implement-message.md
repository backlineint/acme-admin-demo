# Adopt React Context API for Cross-Cutting Provider Patterns: Providers Implement Message

Status: proposed
Date: 2024-01-15
Deciders: Detection Pipeline (automated)

## Context

- The application requires consistent management of cross-cutting concerns like theming, fonts, and data filtering across multiple components
- React Context API provides a mechanism to share state and functionality across component trees without prop drilling
- Pattern detected in 3 files (theme-provider.tsx, faceted-filter.tsx, font-provider.tsx) with 90% confidence, indicating established architectural practice
- Provider pattern enables encapsulation of complex state management logic while exposing clean, type-safe APIs to consuming components
- Message queue facet suggests asynchronous or event-driven communication patterns between providers and consumers

## Problem Statement

Components throughout the application need access to shared configuration and state (themes, fonts, filtering logic) without creating tight coupling or requiring props to be passed through intermediate components that don't use them. A standardized approach is needed to expose these capabilities as public APIs to the component tree.

## Decision

1. MAY: Providers MAY implement message queue or event-driven patterns for asynchronous state updates when appropriate

## Policy Block

- MAY Providers MAY implement message queue or event-driven patterns for asynchronous state updates when appropriate

In scope:
- Theme management and styling configuration
- Font loading and typography settings
- Data table filtering and faceted search state
- Global application configuration accessible to multiple components
- User preferences and settings that span component boundaries

Out of scope:
- Component-local state that doesn't need to be shared
- Server state management (use React Query or similar)
- Form state within a single form component
- Temporary UI state like hover or focus
- State that can be efficiently passed via props through 1-2 levels

Exceptions:
- EXC-001: Performance profiling demonstrates that Context re-renders are causing measurable performance degradation
- EXC-002: Third-party library requires different state management pattern incompatible with Context API

## Rationale

- Pattern detected with 90% confidence across 3 files demonstrates this is an established architectural practice in the codebase
- React Context API is the idiomatic React solution for sharing state across component trees, reducing prop drilling and improving maintainability
- Provider pattern creates clear boundaries between state management logic and presentation components, improving testability and separation of concerns
- Custom hooks provide type-safe, discoverable APIs that make it easy for developers to consume shared functionality consistently

## Consequences

Positive:
- Eliminates prop drilling, reducing boilerplate and making component hierarchies cleaner
- Centralizes cross-cutting concern logic in dedicated providers, improving maintainability and testability
- Custom hooks provide excellent developer experience with TypeScript autocomplete and type safety
- Consistent pattern across the codebase reduces cognitive load for developers working in different areas

Negative:
- Context updates trigger re-renders of all consumers, which can impact performance if not carefully managed
- Overuse of Context can make component dependencies less explicit compared to props
- Testing components that consume context requires additional setup to provide mock context values
- Debugging context-related issues can be more challenging than tracing explicit prop passing

## Alternatives

- Use prop drilling to pass configuration and state through component hierarchy (rejected)
  Rejected because: Creates excessive boilerplate, couples intermediate components to data they don't use, and becomes unmaintainable as component trees grow deeper
  When valid: Only for shallow component hierarchies (1-2 levels) or when state is truly local to a small subtree
- Adopt a global state management library (Redux, Zustand, Jotai) for all shared state (rejected)
  Rejected because: Adds external dependency and complexity for use cases that React Context handles well; overkill for simple configuration sharing
  When valid: Consider for complex application state with intricate update logic, time-travel debugging needs, or when Context performance becomes problematic
- Use module-level singletons or global variables for configuration (rejected)
  Rejected because: Breaks React's component model, makes testing difficult, prevents server-side rendering, and loses reactivity
  When valid: Never valid for React applications; only acceptable for truly static configuration that never changes

## Risks

- Performance degradation from excessive Context re-renders as application scales
  Mitigation: Use React.memo for expensive consumers, split contexts by update frequency, implement context selectors or use useMemo for derived values
  Owner: Engineering team
- Context proliferation leading to deeply nested provider hierarchies
  Mitigation: Regularly review and consolidate related contexts, create composite providers for commonly co-used contexts, document provider hierarchy
  Owner: Engineering team
- Inconsistent error handling when context is used outside provider boundaries
  Mitigation: Enforce error checking in custom hooks via linting rules, provide clear error messages with setup instructions, include provider setup in component documentation
  Owner: Engineering team

## Implementation Notes

- Create a context/ directory at src/context/ to house all provider implementations
- Follow the pattern: create context with createContext, implement [Feature]Provider component, export use[Feature] hook that validates context availability
- For complex providers (like faceted filters), encapsulate business logic within the provider and expose only the minimal necessary API through the hook
- Document provider dependencies and required setup in component README or Storybook documentation
- Consider using TypeScript strict mode to ensure context types are properly defined and null checks are enforced

## Continuation Context


Verify commands:
- grep -r 'createContext' src/context/ | wc -l
- grep -r 'export.*use[A-Z]' src/context/ | grep -v 'React' | wc -l
- find src/context -name '*-provider.tsx' -o -name '*Provider.tsx' | wc -l

Accept when:
- All cross-cutting concerns identified in policy scope are implemented using Context API with dedicated providers
- Each provider exports a custom hook that validates context availability and throws descriptive errors
- Provider components follow consistent naming conventions and are organized in the context directory

## Enforcement

- Verified by: Code review checklist includes verification of Context API usage for cross-cutting concerns
- Verified by: Automated linting rules detect createContext usage outside context directory
- Verified by: Architecture review for new providers to ensure they fit the established pattern
- Violation handling: Code review feedback requests refactoring to use Context API for identified cross-cutting concerns
- Violation handling: Pull requests introducing prop drilling for in-scope concerns are blocked until refactored
- Violation handling: Existing violations are tracked in technical debt backlog and prioritized for refactoring
- Exception process: Developer documents rationale for exception in PR description with reference to policy exceptions
- Exception process: Tech lead or architect reviews exception request against documented exception criteria
- Exception process: Approved exceptions are documented in code comments with ADR reference and expiration date if temporary