# Adopt Radix UI Primitives as Standard Component Library: Components Placed Src

These rules are ALWAYS ACTIVE for all interactive UI components, form controls, overlay components, navigation components, and feedback components placed in the `src/components/ui/` directory.

### Rules

- **R-RADIX-001** MUST: Components MUST be placed in the `src/components/ui/` directory following the established naming convention (kebab-case matching the Radix primitive name).
- **R-RADIX-002** MUST: All interactive components in `src/components/ui/` MUST import from `@radix-ui` packages.
- **R-RADIX-003** MUST: Component wrappers MUST properly forward refs using `React.forwardRef`.
- **R-RADIX-004** MUST: Radix UI dependencies MUST be present in `package.json` and properly versioned.
- **R-RADIX-005** MUST: Automated accessibility tests MUST pass for all Radix-based components.
- **R-RADIX-006** SHOULD: New interactive components in `src/components/ui/` SHOULD use Radix primitives; exceptions require documented justification and architecture review board approval.
- **R-RADIX-007** SHOULD: Component development SHOULD follow established guidelines with examples showing both basic usage and accessibility features (keyboard navigation, screen reader announcements).
- **R-RADIX-008** MAY: Custom component logic MAY include unit tests and integration tests for accessibility features using `@testing-library/react` and `jest-axe`.

### Verify

```bash
# Count Radix UI imports in components/ui directory
grep -r "@radix-ui/react-" src/components/ui/ | wc -l

# Find components not using React.forwardRef
find src/components/ui -name '*.tsx' -exec grep -L 'React.forwardRef' {} \;

# Verify Radix UI dependencies are installed
npm list @radix-ui/react-dropdown-menu @radix-ui/react-dialog @radix-ui/react-select
```

**Accept when:**
- All interactive components in `src/components/ui/` import from `@radix-ui` packages
- Component wrappers properly forward refs using `React.forwardRef`
- Radix UI dependencies are present in `package.json` and properly versioned
- Automated accessibility tests pass for all Radix-based components
- CI pipeline verification confirms no new interactive components bypass Radix primitives without documented exceptions

<enforcement>
Claude Code MUST NOT skip or defer verification. CI pipeline MUST fail if new interactive components in `src/components/ui/` do not use Radix primitives without documented exception. Code review MUST block merge if accessibility attributes are overridden or removed without justification. Architecture review is REQUIRED for custom component implementations that bypass Radix primitives.
</enforcement>