# Standardize TanStack Router File-Based Routing with TypeScript Route Components: Route Files Organized

These rules are ALWAYS ACTIVE for all user-facing route components in the `src/routes` directory, including authentication and authorization route boundaries, error page routes, feature-specific route implementations, and third-party authentication provider route integrations.

### Rules

- **R-ROUTE-001** MUST: Route files MUST be organized using file-based routing conventions where the file path directly corresponds to the URL path structure.
- **R-ROUTE-002** MUST: All route files in `src/routes` directory MUST use `.tsx` extension (TypeScript/TSX).
- **R-ROUTE-003** MUST: All route files MUST export a default component.
- **R-ROUTE-004** SHOULD: Route files SHOULD follow grouping conventions using parenthetical syntax (e.g., `(auth)`, `(errors)`) for logical organization without URL pollution.
- **R-ROUTE-005** SHOULD: Authentication-related routes SHOULD be organized within an `(auth)` directory group.
- **R-ROUTE-006** SHOULD: Error page routes SHOULD be organized within an `(errors)` directory group.
- **R-ROUTE-007** SHOULD: Authenticated feature routes SHOULD use underscore prefix convention (e.g., `_authenticated`) to denote layout route organization and access control boundaries.
- **R-ROUTE-008** MUST: Route files MUST NOT use `.js` or `.jsx` extensions; only `.tsx` is permitted.
- **R-ROUTE-009** SHOULD: Route structure documentation SHOULD be maintained and kept up-to-date with current conventions.

### Verify

```bash
# Count route files using grouping conventions
find src/routes -name '*.tsx' -type f | grep -E '(\(auth\)|\(errors\)|_authenticated)' | wc -l

# Count route files exporting default components
grep -r 'export default' src/routes --include='*.tsx' | wc -l

# Count non-TypeScript route files (should be 0)
find src/routes -name '*.js' -o -name '*.jsx' | wc -l

# List all route files to verify structure
find src/routes -name '*.tsx' -type f | sort
```

**Accept when:**
- All route files in `src/routes` directory use `.tsx` extension (verify command 3 returns 0)
- At least 80% of routes follow the grouping conventions for auth, errors, and authenticated sections (verify command 1 shows significant count relative to total routes)
- All route files export a default component (verify command 2 count matches total route files)
- Route structure documentation exists and is up-to-date with current conventions
- No `.js` or `.jsx` files exist in the `src/routes` directory

<enforcement>
Claude Code MUST NOT skip or defer verification. All route files must be validated against these rules during code review and CI pipeline execution. Violations block merge until corrected or an approved exception is documented.
</enforcement>