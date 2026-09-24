# Standardized layout header alignment across application routes: Individual Feature View Components Not Implement

These rules are ALWAYS ACTIVE for all feature root views, authenticated routes, and error views under `src/features/*` and `src/routes/*` using shared layout primitives.

### Rules

- **R-LAYOUT-001** MUST_NOT: Individual feature view components MUST NOT implement custom layout alignment overrides or top navigation margins.

### Verify

```bash
# Discover and execute layout component test suites verifying header and sidebar composition
npm test -- --testPathPattern="layout"

# Discover and execute static analysis checks on feature entry points to detect disallowed margin wrappers
npx eslint src/features/ src/routes/
```

**Accept when:**
- All feature root views render using shared layout primitives without custom margin wrappers.
- AppSidebar encapsulates all sidebar composition logic across authenticated routes.
- All layout and route tests pass without violations.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>