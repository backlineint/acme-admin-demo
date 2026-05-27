# Adopt Radix UI Primitives as Standard Component Library: Teams Create Composed

These rules are ALWAYS ACTIVE for all interactive UI components in `src/components/ui/` and any route or feature components that require keyboard navigation, focus management, ARIA attributes, or accessible interaction patterns.

### Rules

- **R-RADIX-001** MAY: Teams MAY create composed components that combine multiple Radix primitives for common use cases (e.g., form fields with labels and error messages).
- **R-RADIX-002** MUST: All interactive components requiring keyboard navigation, focus management, or ARIA attributes SHALL use Radix UI primitives.
- **R-RADIX-003** MUST: Component wrappers in `src/components/ui/` SHALL properly forward refs using `React.forwardRef` to ensure compatibility with Radix primitives.
- **R-RADIX-004** SHOULD: File names in `src/components/ui/` SHOULD match Radix primitive names in kebab-case (e.g., `dropdown-menu.tsx` for `@radix-ui/react-dropdown-menu`).
- **R-RADIX-005** MUST: All Radix-based components SHALL include unit tests for custom logic and integration tests for accessibility features using `@testing-library/react` and `jest-axe`.
- **R-RADIX-006** SHOULD: Component usage SHOULD be documented with Storybook or similar tool, showing both basic usage and accessibility features (keyboard navigation, screen reader announcements).
- **R-RADIX-EXC-001** MAY: Exception allowed when a required interaction pattern is not available in Radix UI and cannot be reasonably composed from existing primitives (requires architecture review board approval).
- **R-RADIX-EXC-002** MAY: Exception allowed when performance profiling demonstrates that Radix primitive overhead is unacceptable for a specific high-frequency interaction (requires architecture review board approval).

### Verify

```bash
# Count Radix UI imports in component library
grep -r "@radix-ui/react-" src/components/ui/ | wc -l

# Find component wrappers missing React.forwardRef
find src/components/ui -name '*.tsx' -exec grep -L 'React.forwardRef' {} \;

# Verify Radix UI dependencies are installed
npm list @radix-ui/react-dropdown-menu @radix-ui/react-dialog @radix-ui/react-select

# Check for accessibility test coverage
grep -r "jest-axe\|@testing-library/react" src/components/ui/ | wc -l
```

**Accept when:**
- All interactive components in `src/components/ui/` import from `@radix-ui` packages
- Component wrappers properly forward refs using `React.forwardRef`
- Radix UI dependencies are present in `package.json` and properly versioned
- Automated accessibility tests pass for all Radix-based components using `jest-axe` or similar tools
- New interactive components in `src/components/ui/` use Radix primitives or have documented exceptions approved by architecture review board
- Bundle size monitoring is configured and tracked in CI

<enforcement>
Claude Code MUST NOT skip or defer verification. CI pipeline MUST fail if new interactive components in `src/components/ui/` do not use Radix primitives without documented exception. Code review MUST block merge if accessibility attributes are overridden or removed without justification. Architecture review is REQUIRED for custom component implementations that bypass Radix primitives. Quarterly audits of the component library MUST identify and refactor non-compliant components.
</enforcement>