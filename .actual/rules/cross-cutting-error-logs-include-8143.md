# Adopt console.error for Client-Side Error Logging in React Components: Error Logs Include

These rules are ALWAYS ACTIVE for all React component error handlers, async operation error handling, centralized error handling utilities, application initialization code, and user-triggered operations that may fail.

### Rules

- **R-CONSOLE-001** MUST: Error logs MUST include sufficient context to identify the source component, operation, and error details (error message, stack trace, or error object).

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
- All error handlers in React components contain console.error calls before user-facing error display
- Centralized error handling utilities (handle-server-error.ts and similar) include console.error logging
- Application entry points and initialization code log errors to console before rendering error states

<enforcement>
Clause R-CONSOLE-001 MUST be verified during code review. PRs introducing error handling without console.error logging for critical user operations MUST be rejected. Exceptions are granted only for error handlers delegating to centralized logging utilities that already include console.error, or for third-party library integration code where error logging is handled by the library itself.
</enforcement>