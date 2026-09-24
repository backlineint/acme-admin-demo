# Standardized layout header alignment across application routes: Sidebar Composition Isolated Within Appsidebar Rather

These rules are ALWAYS ACTIVE for all feature root views under src/features/*, authenticated routes and error views under src/routes/*, and shared layout components including TopNav, AppSidebar, AppTitle, and AuthenticatedLayout.

### Rules

- **R-SIDEBAR-001** MUST: Sidebar composition MUST be isolated within AppSidebar rather than composed or modified directly inside feature module views.

### Verify

```bash
# Discover and execute layout component test suites verifying header and sidebar composition
npm test -- layout
# Discover and execute static analysis checks on feature entry points to detect disallowed margin wrappers
npm run lint
```

**Accept when:**
- All feature root views render using shared layout primitives without custom margin wrappers.
- AppSidebar encapsulates all sidebar composition logic across authenticated routes.
- All layout and route tests pass without violations.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>