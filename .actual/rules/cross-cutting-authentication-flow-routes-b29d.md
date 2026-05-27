# Standardize TanStack Router File-Based Routing with TypeScript Route Components: Authentication Flow Routes

These rules are ALWAYS ACTIVE for all user-facing route components in the `src/routes` directory, including authentication flows, error pages, and feature-specific route implementations that integrate with data layers.

### Rules

- **R-AUTH-001** MUST: Authentication flow routes (sign-in, sign-up, forgot-password, otp) MUST be organized under the '(auth)' parenthetical group directory.
- **R-AUTH-002** MUST: All route files in `src/routes` directory MUST use `.tsx` extension (TypeScript/TSX).
- **R-AUTH-003** MUST: All route files MUST export a default component.
- **R-AUTH-004** SHOULD: Error page routes SHOULD be organized under the '(errors)' parenthetical group directory.
- **R-AUTH-005** SHOULD: Authenticated section routes SHOULD use the '_authenticated' underscore prefix for layout route organization.
- **R-AUTH-006** SHOULD: Route grouping syntax (parentheses for layout groups, underscores for prefixes) SHOULD be used to organize routes without polluting URLs.

### Verify

```bash
# Count routes using grouping conventions (auth, errors, authenticated)
find src/routes -name '*.tsx' -type f | grep -E '(\(auth\)|\(errors\)|_authenticated)' | wc -l

# Count all route files exporting default components
grep -r 'export default' src/routes --include='*.tsx' | wc -l

# Count non-TypeScript route files (should be 0)
find src/routes -name '*.js' -o -name '*.jsx' | wc -l

# Verify all route files are TypeScript
find src/routes -name '*.tsx' -type f | wc -l
```

**Accept when:**
- All route files in `src/routes` directory use `.tsx` extension (verify command 3 returns 0)
- At least 80% of routes follow the grouping conventions for auth, errors, and authenticated sections (verify command 1 shows significant count relative to total routes)
- All route files export a default component (verify command 2 count matches total route files from verify command 4)
- Route structure documentation exists and is up-to-date with current conventions
- No `.js` or `.jsx` files exist in the routes directory

<enforcement>
Claude Code MUST NOT skip or defer verification. All route files must be validated against these rules during code review and CI pipeline checks. Pull requests with incorrectly placed routes or non-compliant file extensions MUST be flagged and blocked from merge until corrected or an approved exception is documented.
</enforcement>