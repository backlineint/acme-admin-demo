# Standardize TanStack Router File-Based Routing with TypeScript Route Components: Route Components Defined

These rules are ALWAYS ACTIVE for all route component files in the `src/routes` directory and related authentication, error handling, and feature-specific route implementations.

### Rules

- **R-ROUTE-001** MUST: All route components MUST be defined as TypeScript/TSX files using the .tsx extension for type safety and component-based architecture.

### Verify

```bash
# Verify all route files use .tsx extension (should return 0 for .js/.jsx files)
find src/routes -name '*.js' -o -name '*.jsx' | wc -l

# Verify route grouping conventions are in use
find src/routes -name '*.tsx' -type f | grep -E '(\(auth\)|\(errors\)|_authenticated)' | wc -l

# Verify all route files export a default component
grep -r 'export default' src/routes --include='*.tsx' | wc -l
```

**Accept when:**
- All route files in `src/routes` directory use `.tsx` extension (verify command 1 returns 0)
- At least 80% of routes follow the grouping conventions for auth, errors, and authenticated sections (verify command 2 shows significant count)
- All route files export a default component (verify command 3 count matches total route files)
- Route structure documentation exists and is up-to-date with current conventions

<enforcement>
Claude Code MUST NOT skip or defer verification. CI pipeline MUST fail if route files are found with .js or .jsx extensions. Pull requests with incorrectly placed routes MUST be flagged with automated comments. New route violations MUST block merge until corrected or exception is approved.
</enforcement>