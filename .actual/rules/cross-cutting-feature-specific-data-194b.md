# Standardize TanStack Router File-Based Routing with TypeScript Route Components: Feature Specific Data

These rules are ALWAYS ACTIVE for all user-facing route components in the src/routes directory, authentication and authorization route boundaries, error page routes and error handling flows, and feature-specific route implementations that integrate with data layers.

### Rules

- **R-ROUTING-001** SHOULD: Feature-specific data and logic SHOULD be colocated in feature directories (e.g., 'features/apps/data', 'features/users/data') rather than mixed with route components.
- **R-ROUTING-002** MUST: All route files in src/routes directory use .tsx extension (not .js or .jsx).
- **R-ROUTING-003** MUST: All route files export a default component.
- **R-ROUTING-004** SHOULD: Routes SHOULD follow grouping conventions using parenthetical syntax (e.g., '(auth)', '(errors)') and underscore prefixes (e.g., '_authenticated') for layout route organization.
- **R-ROUTING-005** SHOULD: Authentication routes SHOULD be organized in the (auth) directory grouping.
- **R-ROUTING-006** SHOULD: Error page routes SHOULD be organized in the (errors) directory grouping.
- **R-ROUTING-007** SHOULD: Authenticated sections SHOULD use the _authenticated prefix to enforce access control boundaries.

### Verify

```bash
# Count routes using grouping conventions
find src/routes -name '*.tsx' -type f | grep -E '(\(auth\)|\(errors\)|_authenticated)' | wc -l

# Count all route files exporting default
grep -r 'export default' src/routes --include='*.tsx' | wc -l

# Count non-TSX route files (should be 0)
find src/routes -name '*.js' -o -name '*.jsx' | wc -l

# Total route files
find src/routes -name '*.tsx' -type f | wc -l
```

**Accept when:**
- All route files in src/routes directory use .tsx extension (verify command 3 returns 0)
- At least 80% of routes follow the grouping conventions for auth, errors, and authenticated sections (verify command 1 shows significant count relative to total)
- All route files export a default component (verify command 2 count matches total route files from verify command 4)
- Route structure documentation exists and is up-to-date with current conventions
- Feature-specific data is located in feature directories, not colocated with route components

<enforcement>
Claude Code MUST NOT skip or defer verification. All route files MUST be validated against the .tsx extension requirement and grouping conventions. Pull requests with incorrectly placed routes or non-compliant file extensions MUST be flagged before merge.
</enforcement>