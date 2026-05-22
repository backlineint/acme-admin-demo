# Standardize TanStack Router File-Based Routing with TypeScript Route Components: Route Groups Use

These rules are ALWAYS ACTIVE for all user-facing route components in the `src/routes` directory, including authentication and authorization route boundaries, error page routes, feature-specific route implementations, and third-party authentication provider route integrations.

### Rules

- **R-ROUTE-001** MAY: Route groups MAY use parenthetical syntax `(groupname)` to organize routes logically without affecting the URL structure.

### Verify

```bash
# Count route files using grouping conventions (auth, errors, authenticated sections)
find src/routes -name '*.tsx' -type f | grep -E '(\(auth\)|\(errors\)|_authenticated)' | wc -l

# Count total route files exporting default components
grep -r 'export default' src/routes --include='*.tsx' | wc -l

# Verify no .js or .jsx files exist in routes directory
find src/routes -name '*.js' -o -name '*.jsx' | wc -l
```

**Accept when:**
- All route files in `src/routes` directory use `.tsx` extension (third verify command returns 0)
- At least 80% of routes follow the grouping conventions for auth, errors, and authenticated sections (first verify command shows significant count relative to total)
- All route files export a default component (second verify command count matches total route files)
- Route structure documentation exists and is up-to-date with current conventions

<enforcement>
Claude Code MUST NOT skip or defer verification. All route files MUST use `.tsx` extension. Route placement violations block merge until corrected or exception is approved by tech lead.
</enforcement>