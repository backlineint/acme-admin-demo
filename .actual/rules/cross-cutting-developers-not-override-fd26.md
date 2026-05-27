# Adopt Radix UI Primitives as Standard Component Library: Developers Not Override

These rules are ALWAYS ACTIVE for all interactive UI components in `src/components/ui/` and any route or feature components that implement form controls, overlay components, navigation components, or feedback components.

### Rules

- **R-RADIX-001** MUST_NOT: Developers MUST NOT override or remove accessibility attributes (aria-*, role, data-state) provided by Radix primitives.
- **R-RADIX-002** MUST: All interactive components in `src/components/ui/` MUST import from `@radix-ui` packages rather than implementing custom accessible components.
- **R-RADIX-003** MUST: Component wrappers MUST properly forward refs using `React.forwardRef` to ensure ref forwarding works correctly with Radix primitives.
- **R-RADIX-004** SHOULD: Establish naming convention where file names match Radix primitive names in kebab-case (e.g., `dropdown-menu.tsx` for `@radix-ui/react-dropdown-menu`).
- **R-RADIX-005** SHOULD: Include unit tests for custom component logic and integration tests for accessibility features using `@testing-library/react` and `jest-axe`.

### Verify

```bash
# Count Radix UI imports in component library
grep -r "@radix-ui/react-" src/components/ui/ | wc -l

# Find component wrappers missing React.forwardRef
find src/components/ui -name '*.tsx' -exec grep -L 'React.forwardRef' {} \;

# Verify Radix UI dependencies are installed
npm list @radix-ui/react-dropdown-menu @radix-ui/react-dialog @radix-ui/react-select

# Check for accessibility attribute overrides (manual review required)
grep -r "aria-\|role=\|data-state" src/components/ui/ | grep -i "override\|remove\|delete"
```

**Accept when:**
- All interactive components in `src/components/ui/` import from `@radix-ui` packages
- Component wrappers properly forward refs using `React.forwardRef`
- Radix UI dependencies are present in `package.json` and properly versioned
- Automated accessibility tests pass for all Radix-based components
- No accessibility attributes (aria-*, role, data-state) are overridden or removed without documented exception
- Code review checklist verification confirms Radix primitive usage for new interactive components

<enforcement>
Claude Code MUST NOT skip or defer verification. All interactive components must be scanned for Radix UI imports and accessibility attribute integrity. Code review must block merges that override accessibility attributes without documented exception. Architecture review is required for any custom component implementations that bypass Radix primitives.
</enforcement>