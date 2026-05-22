# Standardize TanStack Router File-Based Routing with TypeScript Route Components: Error Handling Routes

These rules are ALWAYS ACTIVE for all user-facing route components in the `src/routes` directory, including authentication and authorization route boundaries, error page routes, and feature-specific route implementations that integrate with data layers.

### Rules

- **R-ERR-001** MUST: Error handling routes (401, 403, 404, etc.) MUST be organized under the `(errors)` parenthetical group directory.

### Verify

```bash
# Verify error routes exist in (errors) directory
find src/routes -path '*/(errors)/*' -name '*.tsx' -type f | wc -l

# Verify all route files use .tsx extension
find src/routes -name '*.js' -o -name '*.jsx' | wc -l

# Verify all route files export a default component
grep -r 'export default' src/routes --include='*.tsx' | wc -l

# Verify total route file count
find src/routes -name '*.tsx' -type f | wc -l
```

**Accept when:**
- All route files in `src/routes` directory use `.tsx` extension (verify command 2 returns 0)
- Error handling routes are located under `(errors)` parenthetical group directory (verify command 1 shows non-zero count)
- All route files export a default component (verify command 3 count matches verify command 4 total)
- Route structure documentation exists and is up-to-date with current conventions

<enforcement>
Claude Code MUST NOT skip or defer verification. All error handling routes MUST be placed in the `(errors)` directory group. Violations block merge until corrected or an approved exception is documented.
</enforcement>