# Adopt Radix UI Primitives as Standard Component Library: Component Remain Close

These rules are ALWAYS ACTIVE for all interactive UI components in `src/components/ui/` and any route or feature components that implement form controls, overlay components, navigation components, or feedback components.

### Rules

- **R-RADIX-001** SHOULD: Component APIs SHOULD remain close to the Radix primitive API to maintain consistency and reduce learning curve.
- **R-RADIX-002** MUST: All interactive components in `src/components/ui/` MUST import from `@radix-ui` packages.
- **R-RADIX-003** MUST: Component wrappers MUST properly forward refs using `React.forwardRef`.
- **R-RADIX-004** MUST: All interactive components requiring keyboard navigation, focus management, or ARIA attributes MUST use Radix primitives unless an approved exception exists.
- **R-RADIX-005** SHOULD: New form controls (select, checkbox, radio, switch), overlay components (dialog, dropdown, popover, tooltip, context menu), navigation components (tabs, accordion, navigation menu), and feedback components (alert dialog, toast) SHOULD be built from Radix primitives.
- **R-RADIX-006** MAY: Static presentational components without interaction (typography, spacing utilities) and layout components (grid, flex containers) without specific accessibility features MAY use alternative approaches.
- **R-RADIX-007** MUST: Custom component implementations that bypass Radix primitives MUST have documented exceptions approved by the architecture review board.
- **R-RADIX-008** MUST: Accessibility attributes MUST NOT be overridden or removed from Radix-based components without documented justification.

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
- No interactive components bypass Radix primitives without documented exceptions
- Code review checklist verification of Radix primitive usage for new interactive components is complete
- Bundle size monitoring shows acceptable impact of Radix UI adoption

<enforcement>
Claude Code MUST NOT skip or defer verification. CI pipeline MUST fail if new interactive components in `src/components/ui/` do not use Radix primitives without documented exception. Code review MUST block merge if accessibility attributes are overridden or removed without justification. Architecture review is REQUIRED for custom component implementations that bypass Radix primitives. Quarterly audits MUST be conducted to identify and refactor non-compliant components.
</enforcement>