# Adopt console.error for Client-Side Error Logging in React Components: Components Supplement Console

These rules are ALWAYS ACTIVE for all React component error handlers, async operation error handling, centralized error handling utilities, application initialization code, and user-triggered operations that may fail in client-side TypeScript and TSX files.

### Rules

- **R-CONSOLE-001** MUST: Call `console.error()` with a descriptive message and error object in all React component error handlers (componentDidCatch, error boundaries, onError callbacks).
- **R-CONSOLE-002** MUST: Include `console.error()` in centralized error handling utilities (such as handle-server-error.ts) before returning formatted error messages to callers.
- **R-CONSOLE-003** MUST: Wrap application entry point initialization code (main.tsx) with try-catch and log errors to `console.error()` before rendering error UI.
- **R-CONSOLE-004** MUST: Include relevant context in error messages: component name, operation type, affected entities, and user action that triggered the error.
- **R-CONSOLE-005** MAY: Components MAY supplement `console.error()` with additional logging mechanisms (external services, analytics) but `console.error()` remains the baseline requirement.

### Verify

```bash
# Count total console.error calls in source
grep -r 'console\.error' src/ --include='*.tsx' --include='*.ts' | wc -l

# Count console.error usage in catch blocks
grep -r 'catch.*{' src/ --include='*.tsx' --include='*.ts' -A 3 | grep -c 'console\.error'

# Count console.error usage in onError callbacks
grep -r 'onError.*:' src/ --include='*.tsx' --include='*.ts' -A 5 | grep -c 'console\.error'
```

**Accept when:**
- All error handlers in React components contain `console.error()` calls before user-facing error display
- Centralized error handling utilities (handle-server-error.ts and similar) include `console.error()` logging
- Application entry points and initialization code log errors to console before rendering error states
- Error messages include sufficient context (component name, operation, affected entities, user action)

<enforcement>
Claude Code MUST NOT skip or defer verification. All error handlers must include console.error calls. PRs introducing error handling without console.error logging for critical user operations MUST be rejected with code review feedback requesting addition of console.error before merge. Exceptions are granted only for error handlers that delegate to centralized logging utilities already containing console.error, third-party library integration code, or cases where exception rationale is documented in code comments.
</enforcement>