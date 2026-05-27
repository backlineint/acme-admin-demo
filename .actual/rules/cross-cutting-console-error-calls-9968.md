# Adopt console.error for Client-Side Error Logging in React Components: Console Error Calls

These rules are ALWAYS ACTIVE for all React component error handlers, async operation error handling, centralized error handling utilities, application initialization code, and user-triggered operations that may fail in client-side TypeScript and TSX files.

### Rules

- **R-CONSOLE-ERROR-001** SHOULD: Console.error calls SHOULD be placed immediately after error detection and before any error recovery or user notification logic.

### Verify

```bash
# Count total console.error calls in source
grep -r 'console\.error' src/ --include='*.tsx' --include='*.ts' | wc -l

# Count console.error calls in catch blocks
grep -r 'catch.*{' src/ --include='*.tsx' --include='*.ts' -A 3 | grep -c 'console\.error'

# Count console.error calls in onError callbacks
grep -r 'onError.*:' src/ --include='*.tsx' --include='*.ts' -A 5 | grep -c 'console\.error'
```

**Accept when:**
- All error handlers in React components contain console.error calls before user-facing error display
- Centralized error handling utilities (handle-server-error.ts and similar) include console.error logging
- Application entry points and initialization code log errors to console before rendering error states

<enforcement>
Clause Code MUST NOT skip or defer verification. All error handlers must include console.error calls immediately after error detection. PRs introducing error handling without logging for critical user operations MUST be rejected until console.error is added.
</enforcement>