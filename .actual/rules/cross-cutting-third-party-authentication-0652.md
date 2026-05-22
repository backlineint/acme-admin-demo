# Standardize TanStack Router File-Based Routing with TypeScript Route Components: Third Party Authentication

These rules are ALWAYS ACTIVE for all user-facing route components in the src/routes directory, authentication and authorization route boundaries, error page routes and error handling flows, feature-specific route implementations that integrate with data layers, and third-party authentication provider route integrations.

### Rules

- **R-ROUTER-001** SHOULD: Third-party authentication provider routes (e.g., Clerk) SHOULD be organized in dedicated subdirectories (e.g., 'clerk/(auth)') to isolate provider-specific implementations.
- **R-ROUTER-002** MUST: All route files in src/routes directory use .tsx extension.
- **R-ROUTER-003** MUST: All route files export a default component.
- **R-ROUTER-004** SHOULD: Routes follow grouping conventions for auth, errors, and authenticated sections using parenthetical grouping syntax (e.g., '(auth)', '(errors)') and underscore prefixes (e.g., '_authenticated').
- **R-ROUTER-005** SHOULD: Authentication boundaries be explicit and enforced through directory structure with clear separation of concerns.

### Verify

```bash
# Count routes using grouping conventions
find src/routes -name '*.tsx' -type f | grep -E '(\(auth\)|\(errors\)|_authenticated)' | wc -l

# Count all route files with default exports
grep -r 'export default' src/routes --include='*.tsx' | wc -l

# Count non-.tsx route files (should be 0)
find src/routes -name '*.js' -o -name '*.jsx' | wc -l

# Total .tsx route files
find src/routes -name '*.tsx' -type f | wc -l
```

**Accept when:**
- All route files in src/routes directory use .tsx extension (verify command 3 returns 0)
- At least 80% of routes follow the grouping conventions for auth, errors, and authenticated sections (verify command 1 shows significant count relative to total)
- All route files export a default component (verify command 2 count matches total route files from command 4)
- Route structure documentation exists and is up-to-date with current conventions
- Third-party authentication provider routes are organized in dedicated subdirectories

<enforcement>
Claude Code MUST NOT skip or defer verification. All route files must be validated against these rules during code review and CI pipeline checks. Violations block merge until corrected or an approved exception is documented.
</enforcement>