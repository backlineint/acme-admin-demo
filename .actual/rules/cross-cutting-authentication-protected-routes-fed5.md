# Standardize TanStack Router File-Based Routing with TypeScript Route Components: Authentication Protected Routes

These rules are ALWAYS ACTIVE for all user-facing route components in the `src/routes` directory, authentication and authorization route boundaries, error page routes, feature-specific route implementations, and third-party authentication provider route integrations.

### Rules

- **R-AUTH-001** MUST: Authentication-protected routes MUST be grouped under the `_authenticated` directory prefix to clearly separate public and private route boundaries.
- **R-AUTH-002** MUST: All route files in `src/routes` directory MUST use `.tsx` extension (TypeScript/TSX).
- **R-AUTH-003** MUST: All route files MUST export a default component.
- **R-AUTH-004** SHOULD: Routes SHOULD be organized into logical groupings including authentication flows (sign-in, sign-up, forgot-password), error pages (401, 403, 404), and authenticated sections using parenthetical grouping syntax (e.g., `(auth)`, `(errors)`).
- **R-AUTH-005** SHOULD: Route structure SHOULD use parenthetical grouping syntax for layout route organization and route grouping conventions without affecting URL paths.

### Verify

```bash
# Count routes using grouping conventions (auth, errors, authenticated)
find src/routes -name '*.tsx' -type f | grep -E '(\(auth\)|\(errors\)|_authenticated)' | wc -l

# Count all route files exporting default
grep -r 'export default' src/routes --include='*.tsx' | wc -l

# Count non-TSX route files (should be 0)
find src/routes -name '*.js' -o -name '*.jsx' | wc -l

# Verify all route files are TSX
find src/routes -name '*.tsx' -type f | wc -l
```

**Accept when:**
- All route files in `src/routes` directory use `.tsx` extension (verify command 4 returns a count, verify command 3 returns 0)
- At least 80% of routes follow the grouping conventions for auth, errors, and authenticated sections (verify command 1 shows significant count relative to total)
- All route files export a default component (verify command 2 count matches total route files)
- Route structure documentation exists and is up-to-date with current conventions
- Authentication-protected routes are located under `_authenticated` directory prefix

<enforcement>
Claude Code MUST NOT skip or defer verification. All route files must be validated against these rules during code review and CI pipeline checks. Violations block merge until corrected or an approved exception is documented.
</enforcement>