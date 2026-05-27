# Adopt console.error for Client-Side Error Logging in React Components: Error Logging Not

These rules are ALWAYS ACTIVE for all React component error handlers, async operation error handling, centralized error handling utilities, application initialization code, and user-triggered operations that may fail.

### Rules

- **R-CONSOLE-001** MUST NOT: Error logging MUST NOT suppress or swallow errors — logged errors should still propagate to appropriate error boundaries or handlers.

### Verify

```bash
# Count console.error calls in TypeScript and TSX files
grep -r 'console\.error' src/ --include='*.tsx' --include='*.ts' | wc -l

# Count catch blocks with console.error
grep -r 'catch.*{' src/ --include='*.tsx' --include='*.ts' -A 3 | grep -c 'console\.error'

# Count onError callbacks with console.error
grep -r 'onError.*:' src/ --include='*.tsx' --include='*.ts' -A 5 | grep -c 'console\.error'
```

**Accept when:**
- All error handlers in React components contain console.error calls before user-facing error display
- Centralized error handling utilities (handle-server-error.ts and similar) include console.error logging
- Application entry points and initialization code log errors to console before rendering error states
- Errors logged via console.error continue to propagate to error boundaries or handlers rather than being suppressed

<enforcement>
Clause R-CONSOLE-001 MUST be verified during code review. Claude Code MUST NOT skip or defer verification of error handler implementations. PRs introducing error handling without console.error logging for critical user operations MUST be rejected with specific feedback requesting addition of logging before merge.
</enforcement>