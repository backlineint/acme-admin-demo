# Standardize Console Logging for Error Handling and Application Events: Console Logging Not

Status: proposed
Date: 2024-01-15
Deciders: Detection Pipeline (automated)

## Context

- The application requires consistent error tracking and debugging capabilities across multiple components and modules
- Frontend applications need visibility into runtime errors, server communication failures, and user-triggered operations
- Multiple dialog components (tasks-multi-delete-dialog, users-multi-delete-dialog) and error handling utilities (handle-server-error) exhibit similar logging patterns
- The main application entry point (main.tsx) establishes logging infrastructure at application bootstrap
- Pattern detected across 4 files with 91.90% confidence indicates an established architectural practice

## Problem Statement

Without standardized logging practices, debugging production issues becomes difficult, error tracking is inconsistent, and operational visibility into application behavior is limited. Teams need a consistent approach to capture errors, user actions, and system events for troubleshooting and monitoring.

## Decision

1. MUST: Console logging MUST NOT expose sensitive user data such as passwords, tokens, or personally identifiable information

## Policy Block

- MUST Console logging MUST NOT expose sensitive user data such as passwords, tokens, or personally identifiable information

In scope:
- Error handling utilities and functions
- Dialog components performing server operations
- Application bootstrap and initialization code
- Server communication error handlers
- Batch operation components (multi-delete, bulk updates)

Out of scope:
- Third-party library internal logging
- Development-only debug statements
- Performance profiling logs
- Analytics and telemetry data collection

Exceptions:
- EXC-001: Production builds may suppress verbose logging to reduce console noise
- EXC-002: High-frequency operations may use sampling or rate-limiting for logs

## Rationale

- Pattern detected across 4 critical files (error handlers, dialog components, main entry point) with 91.90% confidence indicates this is an established architectural practice
- Console logging provides immediate visibility during development and can be captured by browser monitoring tools in production
- Consistent logging patterns across similar components (tasks-multi-delete-dialog, users-multi-delete-dialog) enable predictable debugging workflows
- Centralized error handling (handle-server-error.ts) with logging ensures all server errors are captured uniformly

## Consequences

Positive:
- Developers can quickly identify and diagnose errors during development and testing
- Production monitoring tools can capture console logs for error tracking and alerting
- Consistent logging patterns reduce cognitive load when debugging across different components
- Error context and stack traces provide actionable information for troubleshooting

Negative:
- Console logs may expose implementation details or error messages to end users in production
- High-volume logging can impact browser performance and memory usage
- Console logs alone do not provide structured data for advanced analytics or aggregation
- Sensitive information may accidentally be logged if developers are not careful

## Alternatives

- Use a structured logging library (e.g., Winston, Pino, or Bunyan) for frontend logging (rejected)
  Rejected because: Adds dependency overhead and complexity for a frontend application where console logging is sufficient for current needs
  When valid: Consider if application requires log aggregation, structured data, or multiple log transports
- Implement a custom logging abstraction layer wrapping console methods (deferred)
  Rejected because: Not rejected, but deferred until logging requirements become more complex
  When valid: Adopt when needing environment-specific log levels, filtering, or integration with external monitoring services
- Disable all console logging in production builds (rejected)
  Rejected because: Eliminates valuable debugging information for production issues and prevents browser monitoring tools from capturing errors
  When valid: Only valid for highly sensitive applications where information disclosure is a critical concern

## Risks

- Sensitive data (user credentials, tokens, PII) may be accidentally logged to console
  Mitigation: Implement code review checklist for logging statements; add linting rules to detect common sensitive data patterns; sanitize error objects before logging
  Owner: Engineering team
- Excessive logging in high-frequency operations may degrade browser performance
  Mitigation: Implement log sampling or rate-limiting for high-frequency operations; monitor performance metrics; use conditional logging based on environment
  Owner: Engineering team
- Console logs may be disabled or hidden in some browser configurations, reducing visibility
  Mitigation: Complement console logging with external error tracking service (e.g., Sentry, Rollbar); ensure critical errors are also reported through alternative channels
  Owner: DevOps team

## Implementation Notes

- Use console.error() for errors and exceptions, console.warn() for warnings, and console.log() for informational messages
- Include contextual information in log messages such as component name, operation type, and relevant identifiers
- Ensure error objects are logged with their full stack traces by passing the error object directly to console.error()
- Consider wrapping console methods in a thin abstraction to enable future enhancements (log levels, filtering) without changing call sites

## Continuation Context


Verify commands:
- grep -r 'console\.error' src/ --include='*.ts' --include='*.tsx' | wc -l
- grep -r 'handle.*[Ee]rror' src/ --include='*.ts' --include='*.tsx' -A 5 | grep -c 'console'
- npm run lint -- --rule 'no-console: off'

Accept when:
- All error handling functions contain at least one console.error() call with the error object
- Server error handler (handle-server-error.ts) logs complete error details including stack traces
- No console.log statements contain obvious sensitive data patterns (password, token, apiKey, secret)

## Enforcement

- Verified by: Code review checklist includes verification of appropriate logging in error handlers
- Verified by: Automated grep-based checks in CI pipeline verify presence of console logging in error handling code
- Verified by: Manual testing of error scenarios confirms logs appear in browser console
- Violation handling: Pull requests missing error logging in new error handlers are flagged during code review
- Violation handling: Developers are asked to add appropriate logging before merge approval
- Violation handling: Existing code without logging is addressed opportunistically during maintenance
- Exception process: Request exception through architecture review if logging would expose sensitive data
- Exception process: Document rationale for exception in code comments
- Exception process: Ensure alternative error tracking mechanism is in place