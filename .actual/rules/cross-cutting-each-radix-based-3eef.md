# Adopt Radix UI Primitives as Standard Component Library: Each Radix Based

These rules are ALWAYS ACTIVE for all interactive UI components in `src/components/ui/` and any route or feature components that implement keyboard navigation, focus management, or ARIA attributes.

### Rules

- **R-RADIX-001** MUST: Each Radix-based component MUST re-export the primitive's sub-components with appropriate TypeScript types and ref forwarding.
- **R-RADIX-002** MUST: All component wrappers MUST use `React.forwardRef` to ensure ref forwarding works correctly with Radix primitives.
- **R-RADIX-003** MUST: All interactive components in `src/components/ui/` MUST import from `@radix-ui` packages, not implement custom accessibility logic.
- **R-RADIX-004** SHOULD: File names SHOULD match Radix primitive names in kebab-case (e.g., `dropdown-menu.tsx` for `@radix-ui/react-dropdown-menu`).
- **R-RADIX-005** SHOULD: Components SHOULD be documented with Storybook or similar, showing both basic usage and accessibility features (keyboard navigation, screen reader announcements).
- **R-RADIX-006** MAY: Custom interaction patterns not available in Radix UI MAY be implemented only when they cannot be reasonably composed from existing primitives and an exception is approved (EXC-001).
- **R-RADIX-007** MAY: Performance-critical high-frequency interactions MAY bypass Radix primitives only when profiling demonstrates unacceptable overhead and an exception is approved (EXC-002).

### Verify

```bash
# Count Radix UI imports in component library
grep -r "@radix-ui/react-" src/components/ui/ | wc -l

# Find components missing React.forwardRef
find src/components/ui -name '*.tsx' -exec grep -L 'React.forwardRef' {} \;

# Verify Radix UI dependencies are installed
npm list @radix-ui/react-dropdown-menu @radix-ui/react-dialog @radix-ui/react-select

# Check for accessibility test coverage
grep -r "jest-axe\|@testing-library/jest-dom" . --include="*.test.tsx" --include="*.spec.tsx" | wc -l
```

**Accept when:**
- All interactive components in `src/components/ui/` import from `@radix-ui` packages
- Component wrappers properly forward refs using `React.forwardRef`
- Radix UI dependencies are present in `package.json` and properly versioned
- Automated accessibility tests pass for all Radix-based components
- No interactive components bypass Radix primitives without documented exception

<enforcement>
Claude Code MUST NOT skip or defer verification. CI pipeline MUST fail if new interactive components in `src/components/ui/` don't use Radix primitives without documented exception. Code review MUST block merge if accessibility attributes are overridden or removed without justification. Architecture review is REQUIRED for custom component implementations that bypass Radix primitives.
</enforcement>