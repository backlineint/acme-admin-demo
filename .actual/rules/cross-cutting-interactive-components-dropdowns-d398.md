# Adopt Radix UI Primitives as Standard Component Library: Interactive Components Dropdowns

These rules are ALWAYS ACTIVE for all interactive UI components in `src/components/ui/` and any files implementing dropdowns, dialogs, forms, menus, tabs, and other interactive patterns requiring keyboard navigation, focus management, or ARIA attributes.

### Rules

- **R-RADIX-001** MUST: All interactive UI components (dropdowns, dialogs, forms, menus, tabs) MUST be built using Radix UI primitives as the foundational layer.
- **R-RADIX-002** MUST: All component wrappers in `src/components/ui/` MUST use `React.forwardRef` to ensure ref forwarding works correctly with Radix primitives.
- **R-RADIX-003** MUST: Component file names MUST match Radix primitive names in kebab-case (e.g., `dropdown-menu.tsx` for `@radix-ui/react-dropdown-menu`).
- **R-RADIX-004** MUST: Interactive components requiring keyboard navigation, focus management, or ARIA attributes MUST NOT override or remove accessibility attributes without documented justification.
- **R-RADIX-005** SHOULD: Create wrapper components in `src/components/ui/` that re-export Radix primitives with project-specific styling and TypeScript types.
- **R-RADIX-006** SHOULD: Document component usage with Storybook or similar tool, showing both basic usage and accessibility features (keyboard navigation, screen reader announcements).
- **R-RADIX-007** SHOULD: Include unit tests for custom component logic and integration tests for accessibility features using `@testing-library/react` and `jest-axe`.
- **R-RADIX-EXC-001** MAY: A required interaction pattern is not available in Radix UI and cannot be reasonably composed from existing primitives (requires architecture review board approval).
- **R-RADIX-EXC-002** MAY: Performance profiling demonstrates that Radix primitive overhead is unacceptable for a specific high-frequency interaction (requires architecture review board approval).

### Verify

```bash
# Count Radix UI imports in component wrappers
grep -r "@radix-ui/react-" src/components/ui/ | wc -l

# Find component wrappers missing React.forwardRef
find src/components/ui -name '*.tsx' -exec grep -L 'React.forwardRef' {} \;

# Verify Radix UI dependencies are installed
npm list @radix-ui/react-dropdown-menu @radix-ui/react-dialog @radix-ui/react-select

# Run accessibility tests
npm run test:a11y

# Check bundle size impact
npm run build && npm run analyze:bundle
```

**Accept when:**
- All interactive components in `src/components/ui/` import from `@radix-ui` packages
- Component wrappers properly forward refs using `React.forwardRef`
- Radix UI dependencies are present in `package.json` and properly versioned
- Automated accessibility tests pass for all Radix-based components
- No interactive components bypass Radix primitives without documented exceptions
- Bundle size monitoring shows acceptable impact from Radix UI adoption

<enforcement>
Claude Code MUST NOT skip or defer verification. All new interactive components MUST be verified against these rules before approval. Code review MUST check for Radix primitive usage and proper ref forwarding. CI pipeline MUST fail if interactive components in `src/components/ui/` don't use Radix primitives without documented exception. Architecture review is required for any custom component implementations that bypass Radix primitives.
</enforcement>