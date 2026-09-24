# Standardized layout header alignment across application routes: Feature Root Views Authenticated Route Boundaries

These rules are ALWAYS ACTIVE for feature root views and authenticated route boundaries.

### Rules

- **R-LAYOUT-001** MUST: Feature root views and authenticated route boundaries MUST consume shared layout primitives without defining custom margin or header positioning wrappers.

### Verify

```bash
# Discover and execute layout component test suites verifying header and sidebar composition.
npx vitest run src/components/layout/
# Discover and execute static analysis checks on feature entry points to detect disallowed margin wrappers.
npx tsc --noEmit
```

**Accept when:**
- All feature root views render using shared layout primitives without custom margin wrappers.
- AppSidebar encapsulates all sidebar composition logic across authenticated routes.
- All layout and route tests pass without violations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Code review verification and automated static checks targeting style and margin overrides on layout boundaries are mandatory.
</enforcement>