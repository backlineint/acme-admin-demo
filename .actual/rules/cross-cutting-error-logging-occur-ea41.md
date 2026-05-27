# Adopt console.error for Client-Side Error Logging in React Components: Error Logging Occur

These rules are ALWAYS ACTIVE for all React component error handlers, async operation catch blocks, centralized error handling utilities, application initialization code, and user-triggered operations that may fail in the frontend codebase.

### Rules

- **R-CONSOLE-001** SHOULD: Error logging SHOULD occur in mutation error handlers, async operation catch blocks, and centralized error handling utilities.
- **R-CONSOLE-002** MUST: React component error handlers (componentDidCatch, error boundaries, onError callbacks) MUST include console.error calls with descriptive messages and error objects.
- **R-CONSOLE-003** MUST: Centralized error handling utilities (such as handle-server-error.ts) MUST include console.error before returning formatted error messages to callers.
- **R-CONSOLE-004** MUST: Application entry points (main.tsx) MUST wrap initialization code with try-catch and log errors to console.error before rendering error UI.
- **R-CONSOLE-005** SHOULD: Error messages SHOULD include relevant context: component name, operation type, affected entities, and user action that triggered the error.
- **R-CONSOLE-006** MUST: Promise catch blocks in async operations MUST include console.error logging before error handling or user-facing error display.

### Verify

```bash
# Count total console.error calls in source
grep -r 'console\.error' src/ --include='*.tsx' --include='*.ts' | wc -l

# Count console.error in catch blocks
grep -r 'catch.*{' src/ --include='*.tsx' --include='*.ts' -A 3 | grep -c 'console\.error'

# Count console.error in onError handlers
grep -r 'onError.*:' src/ --include='*.tsx' --include='*.ts' -A 5 | grep -c 'console\.error'
```

**Accept when:**
- All error handlers in React components contain console.error calls before user-facing error display
- Centralized error handling utilities (handle-server-error.ts and similar) include console.error logging
- Application entry points and initialization code log errors to console before rendering error states
- Promise catch blocks in async operations include console.error with descriptive context
- Error messages include component name, operation type, and user action context

<enforcement>
Claude Code MUST NOT skip or defer verification. All error handlers must be reviewed to ensure console.error is present with appropriate context before approving changes.
</enforcement>