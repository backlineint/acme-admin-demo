# Adopt console.error for Client-Side Error Logging in React Components: Error Logs Include

Status: proposed
Date: 2024-01-15
Deciders: Detection Pipeline (automated)

## Context

- The application is a React-based frontend that interacts with backend services and requires visibility into runtime errors and failures
- Error handling occurs in multiple contexts including component lifecycle, async operations (mutations/queries), and application initialization
- Development and debugging workflows require immediate visibility into error conditions without external tooling dependencies
- The codebase demonstrates a consistent pattern of using console.error for logging errors across user-facing components, task management components, error handlers, and application entry points
- Pattern detected in 4 files with 91.90% confidence, indicating established architectural practice rather than isolated implementation

## Problem Statement

Frontend applications need a standardized, lightweight approach to log errors during development and production that provides immediate visibility into failures without requiring complex logging infrastructure, while maintaining consistency across component boundaries and error handling contexts.

## Decision

1. MUST: Error logs MUST include sufficient context to identify the source component, operation, and error details (error message, stack trace, or error object)

## Policy Block

- MUST Error logs MUST include sufficient context to identify the source component, operation, and error details (error message, stack trace, or error object)

In scope:
- React component error handlers (componentDidCatch, error boundaries)
- Async operation error handling (mutation onError callbacks, promise catch blocks)
- Centralized error handling utilities and helper functions
- Application initialization and bootstrap error handling
- User-triggered operations that may fail (delete, update, create operations)

Out of scope:
- Server-side logging (Node.js backend services)
- Build-time errors and compilation failures
- Test execution logging (use test framework reporters)
- Third-party library internal error handling
- Non-error informational logging (use console.log, console.info, or console.debug)

## Rationale

- Console.error provides zero-dependency, universally available logging that works in all browser environments and development tools without configuration
- The pattern is already established across 4 critical files (multi-delete dialogs, main entry point, error handlers) with 91.90% confidence, indicating organic adoption and proven utility
- Browser DevTools automatically capture console.error output with stack traces, making debugging significantly faster during development
- This approach maintains simplicity while allowing future enhancement with structured logging services without requiring immediate refactoring

## Consequences

Positive:
- Developers gain immediate visibility into errors during development without configuring external logging services
- Consistent error logging pattern across components reduces cognitive load and improves code maintainability
- Browser DevTools integration provides automatic stack traces, source mapping, and error grouping
- Low barrier to entry for new developers - console.error is universally understood and requires no training

Negative:
- Console.error output is not persisted or aggregated in production environments without additional tooling
- No built-in error categorization, severity levels, or structured metadata without wrapper abstractions
- Production error monitoring requires additional integration with services like Sentry, LogRocket, or Datadog
- Console output can be disabled or stripped in production builds, potentially losing diagnostic information

## Alternatives

- Adopt a structured logging library (winston, pino, loglevel) for frontend error logging (rejected)
  Rejected because: Adds dependency overhead and configuration complexity for frontend applications where console.error provides sufficient immediate value; structured logging can be layered on top later if needed
  When valid: When production error aggregation, log levels, and structured metadata are required from day one
- Integrate error monitoring service (Sentry, Bugsnag) directly in error handlers without console.error (rejected)
  Rejected because: Creates external service dependency for basic development debugging; console.error provides baseline visibility that works offline and in all environments
  When valid: When production error tracking is the primary concern and development debugging is handled through other means
- Use custom error logging abstraction that wraps console.error and allows future enhancement (deferred)
  Rejected because: Not rejected - this is a natural evolution path; current pattern establishes baseline before abstraction
  When valid: When error logging requirements expand to include categorization, filtering, or integration with multiple backends

## Risks

- Console.error calls may be stripped from production builds by bundler optimizations, losing diagnostic information
  Mitigation: Configure build tools (webpack, vite) to preserve console.error in production or replace with production-safe logging service; document build configuration requirements
  Owner: Frontend Engineering Team
- Inconsistent error context across different components makes debugging difficult when errors lack sufficient detail
  Mitigation: Establish error logging conventions in code review guidelines; provide examples of good error context (component name, operation, user action)
  Owner: Engineering Team
- Over-reliance on console.error without production monitoring creates blind spots for user-impacting errors
  Mitigation: Plan integration with error monitoring service (Sentry, LogRocket) as application matures; treat console.error as development baseline, not production solution
  Owner: DevOps/Platform Team

## Implementation Notes

- In React component error handlers (onError callbacks, catch blocks), call console.error with descriptive message and error object: console.error('Failed to delete users:', error)
- For centralized error handling utilities (like handle-server-error.ts), include console.error before returning formatted error messages to callers
- In application entry points (main.tsx), wrap initialization code with try-catch and log errors before rendering error UI
- Include relevant context in error messages: component name, operation type, affected entities, and user action that triggered the error

## Continuation Context


Verify commands:
- grep -r 'console\.error' src/ --include='*.tsx' --include='*.ts' | wc -l
- grep -r 'catch.*{' src/ --include='*.tsx' --include='*.ts' -A 3 | grep -c 'console\.error'
- grep -r 'onError.*:' src/ --include='*.tsx' --include='*.ts' -A 5 | grep -c 'console\.error'

Accept when:
- All error handlers in React components contain console.error calls before user-facing error display
- Centralized error handling utilities (handle-server-error.ts and similar) include console.error logging
- Application entry points and initialization code log errors to console before rendering error states

## Enforcement

- Verified by: Code review checklist requiring console.error in error handlers
- Verified by: Static analysis with ESLint custom rule to detect catch blocks without console.error
- Verified by: Manual verification during PR review that error paths include appropriate logging
- Violation handling: PR comments requesting addition of console.error in error handlers
- Violation handling: Reject PRs that introduce error handling without logging for critical user operations
- Violation handling: Document violations in code review feedback and request updates before merge
- Exception process: Exceptions granted for error handlers that delegate to centralized logging utilities that already include console.error
- Exception process: Exceptions for third-party library integration code where error logging is handled by the library
- Exception process: Document exception rationale in code comments explaining why console.error is omitted