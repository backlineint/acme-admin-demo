# Adopt console.error for Client-Side Error Logging in React Components: Error Conditions React

These rules are ALWAYS ACTIVE for all React component error handlers, async operation error handling, centralized error handling utilities, application initialization code, and user-triggered operations in the frontend codebase.

### Rules

- **R-ERR-001** MUST: All error conditions in React components MUST be logged using console.error before displaying user-facing error messages or triggering error boundaries.
- **R-ERR-002** MUST: React component error handlers (componentDidCatch, error boundaries) MUST call console.error with descriptive message and error object.
- **R-ERR-003** MUST: Async operation error handling (mutation onError callbacks, promise catch blocks) MUST include console.error before returning formatted error messages to callers.
- **R-ERR-004** MUST: Centralized error handling utilities (handle-server-error.ts and similar) MUST include console.error logging before returning error responses.
- **R-ERR-005** MUST: Application entry points and initialization code MUST log errors to console.error before rendering error UI or error states.
- **R-ERR-006** MUST: Error messages logged to console.error MUST include relevant context: component name, operation type, affected entities, and user action that triggered the error.
- **R-ERR-007** MAY: Exceptions are granted for error handlers that delegate to centralized logging utilities that already include console.error, provided the delegation is documented.
- **R-ERR-008** MAY: Exceptions are granted for third-party library integration code where error logging is handled by the library itself, provided the exception rationale is documented in code comments.

### Verify

```bash
# Count total console.error calls in React components
grep -r 'console\.error' src/ --include='*.tsx' --include='*.ts' | wc -l

# Count catch blocks with console.error logging
grep -r 'catch.*{' src/ --include='*.tsx' --include='*.ts' -A 3 | grep -c 'console\.error'

# Count onError callbacks with console.error logging
grep -r 'onError.*:' src/ --include='*.tsx' --include='*.ts' -A 5 | grep -c 'console\.error'
```

**Accept when:**
- All error handlers in React components contain console.error calls before user-facing error display
- Centralized error handling utilities (handle-server-error.ts and similar) include console.error logging
- Application entry points and initialization code log errors to console before rendering error states
- Error messages include sufficient context (component name, operation type, affected entities)
- Exceptions are documented in code comments with clear rationale

<enforcement>
Clause Code MUST NOT skip or defer verification. All error handlers must be reviewed for console.error presence during code review. Static analysis with ESLint custom rules SHOULD detect catch blocks without console.error. PRs introducing error handling without logging for critical user operations MUST be rejected.
</enforcement>