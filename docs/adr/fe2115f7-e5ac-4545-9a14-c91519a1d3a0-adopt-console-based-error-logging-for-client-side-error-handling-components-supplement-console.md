# Adopt Console-Based Error Logging for Client-Side Error Handling: Components Supplement Console

Status: proposed
Date: 2024-01-15
Deciders: Detection Pipeline (automated)

## Activation

This ADR is ALWAYS ACTIVE for all client-side error handling code in the application.

## Context

- The application requires consistent error visibility during development and debugging phases to identify issues quickly
- Client-side errors in React components and utility functions need to be captured and logged for troubleshooting
- Multiple components across the codebase (tasks-multi-delete-dialog, users-multi-delete-dialog, handle-server-error, main.tsx) exhibit a consistent pattern of using console-based logging
- The pattern was detected across 4 files with 91.90% confidence, indicating a deliberate architectural choice rather than ad-hoc implementation
- Browser developer tools provide native support for console-based logging, making it a zero-dependency solution for error visibility

## Problem Statement

Without a standardized approach to logging client-side errors, developers may inconsistently handle error visibility, leading to silent failures, difficult debugging sessions, and reduced operational observability. The application needs a lightweight, consistent mechanism to capture and surface errors during both development and production environments.

## Decision

1. MAY: Components MAY supplement console logging with user-facing error messages or toast notifications

## Policy Block

- MAY Components MAY supplement console logging with user-facing error messages or toast notifications

In scope:
- All React component error boundaries and catch blocks
- Server communication error handlers (API calls, fetch operations)
- Application initialization and bootstrap code (main.tsx)
- Utility functions that handle errors (handle-server-error.ts)
- User interaction flows with error states (multi-delete operations)

Out of scope:
- Server-side logging (Node.js backend services)
- Third-party library internal error handling
- Production error monitoring services (Sentry, LogRocket, etc.) - these are complementary
- Performance logging or analytics tracking

Exceptions:
- EXC-001: Errors contain sensitive user data (passwords, tokens, PII) that should not be logged to console
- EXC-002: High-frequency expected errors (e.g., network timeouts during polling) that would flood console logs

## Rationale

- Console-based logging provides immediate visibility into errors during development without requiring additional infrastructure or dependencies
- The pattern's 91.90% confidence across 4 files indicates this is an established practice in the codebase that should be formalized
- Browser developer tools are universally available and familiar to all frontend developers, reducing onboarding friction
- Console logs persist in production environments and can be captured by browser-based monitoring tools for production debugging

## Consequences

Positive:
- Developers gain immediate visibility into client-side errors during development and testing
- Consistent error logging pattern across the codebase improves maintainability and debugging efficiency
- Zero additional dependencies or infrastructure required for basic error observability
- Browser-based monitoring tools can capture console logs for production error tracking

Negative:
- Console logs may expose sensitive information if not properly sanitized before logging
- High-volume console logging can impact browser performance in production environments
- Console logs are ephemeral and lost on page refresh unless captured by external monitoring
- Requires supplementary tooling (Sentry, LogRocket) for comprehensive production error tracking and alerting

## Alternatives

- Use a structured logging library (e.g., loglevel, winston-browser) for client-side logging (rejected)
  Rejected because: Adds dependency overhead and complexity for a pattern that is already working effectively with native console APIs. The detected pattern shows successful adoption of console-based logging without additional libraries.
  When valid: Consider if advanced features like log levels, filtering, or custom transports become necessary
- Rely solely on production error monitoring services (Sentry, Rollbar) without console logging (rejected)
  Rejected because: Removes immediate visibility during development and creates dependency on external services for basic debugging. Console logging provides instant feedback without network latency.
  When valid: Production monitoring should complement, not replace, console logging
- Implement custom error logging service with configurable transports (deferred)
  Rejected because: Over-engineering for current needs. The simple console-based approach meets requirements without additional abstraction layers.
  When valid: Revisit if requirements emerge for multiple logging destinations, complex filtering, or log aggregation

## Risks

- Sensitive data (tokens, passwords, PII) may be inadvertently logged to console and exposed in production
  Mitigation: Implement code review checklist for error logging. Create utility functions that sanitize error objects before logging. Add linting rules to detect common sensitive data patterns.
  Owner: Security team and frontend engineering team
- Excessive console logging in production may degrade browser performance and user experience
  Mitigation: Implement log level controls that can be configured per environment. Use conditional logging that can be disabled in production builds. Monitor performance metrics.
  Owner: Frontend engineering team
- Console logs are ephemeral and may be lost before developers can investigate production issues
  Mitigation: Integrate browser-based error monitoring (Sentry, LogRocket) to capture console logs. Implement error boundaries that both log and report to monitoring services.
  Owner: DevOps and frontend engineering team

## Implementation Notes

- Create a centralized error logging utility (similar to handle-server-error.ts) that standardizes console.error() calls with consistent formatting and context
- Wrap all async operations and API calls with try-catch blocks that invoke the error logging utility
- Implement React Error Boundaries at key component boundaries (feature modules, route components) that log errors before displaying fallback UI
- Add TypeScript types for error logging functions to ensure consistent error object structure across the codebase

## Continuation Context


Verify commands:
- grep -r 'console\.error' src/ --include='*.tsx' --include='*.ts' | wc -l
- grep -r 'catch.*{' src/ --include='*.tsx' --include='*.ts' -A 3 | grep -c 'console\.error'
- npm run lint -- --rule 'no-console: [error, { allow: ["error", "warn"] }]'

Accept when:
- All error catch blocks in the codebase contain console.error() calls with meaningful context
- The handle-server-error utility (or equivalent) is consistently used across API error handling code
- Linting rules pass with console.error and console.warn allowed but other console methods flagged for review

## Enforcement

- Verified by: Code review checklist requiring verification of error logging in all new error handlers
- Verified by: ESLint rules enforcing console.error() usage in catch blocks
- Verified by: Automated testing that verifies console.error is called during error scenarios
- Violation handling: Pull requests with missing error logging must be revised before merge approval
- Violation handling: ESLint violations block CI/CD pipeline until resolved
- Violation handling: Periodic code audits identify and remediate silent error handling
- Exception process: Developer documents justification for exception in code comments and PR description
- Exception process: Engineering lead or security team reviews exception request based on policy exception criteria
- Exception process: Approved exceptions are tracked in ADR amendments or exception registry