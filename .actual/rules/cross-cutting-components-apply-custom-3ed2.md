# Adopt Radix UI Primitives as Standard Component Library: Components Apply Custom

These rules are ALWAYS ACTIVE for all interactive UI components in `src/components/ui/` and any components requiring keyboard navigation, focus management, ARIA attributes, form controls, overlay components, navigation components, or feedback components.

### Rules

- **R-RADIX-001** MUST: All interactive UI components requiring keyboard navigation, focus management, or ARIA attributes SHALL be built using Radix UI primitives.
- **R-RADIX-002** MUST: Form controls (select, checkbox, radio, switch) SHALL use corresponding Radix UI primitives.
- **R-RADIX-003** MUST: Overlay components (dialog, dropdown, popover, tooltip, context menu) SHALL use corresponding Radix UI primitives.
- **R-RADIX-004** MUST: Navigation components (tabs, accordion, navigation menu) SHALL use corresponding Radix UI primitives.
- **R-RADIX-005** MUST: Feedback components (alert dialog, toast) SHALL use corresponding Radix UI primitives.
- **R-RADIX-006** SHOULD: Components SHOULD apply custom styling using CSS-in-JS or utility classes while preserving all Radix accessibility attributes and behaviors.
- **R-RADIX-007** MUST: All component wrappers in `src/components/ui/` SHALL use `React.forwardRef` to ensure ref forwarding works correctly with Radix primitives.
- **R-RADIX-008** MUST: Component file names SHALL match Radix primitive names in kebab-case (e.g., `dropdown-menu.tsx` for `@radix-ui/react-dropdown-menu`).
- **R-RADIX-009** MUST: Accessibility attributes and ARIA properties from Radix primitives SHALL NOT be overridden or removed without documented justification and accessibility impact analysis.
- **R-RADIX-010** MAY: Static presentational components without interaction (typography, spacing utilities) and layout components (grid, flex containers) without specific accessibility features MAY use alternative implementations.
- **R-RADIX-011** MAY: Third-party widget integrations that provide their own accessibility layer MAY use alternative implementations.
- **R-RADIX-012** MAY: Canvas or WebGL-based interactive elements where DOM-based accessibility is not applicable MAY use alternative implementations.

### Exceptions

- **EXC-001**: A required interaction pattern is not available in Radix UI and cannot be reasonably composed from existing primitives. Requires architecture review board approval and accessibility audit.
- **EXC-002**: Performance profiling demonstrates that Radix primitive overhead is unacceptable for a specific high-frequency interaction. Requires architecture review board approval and accessibility audit.

### Verify

```bash
# Count Radix UI imports in component library
grep -r "@radix-ui/react-" src/components/ui/ | wc -l

# Find components not using React.forwardRef
find src/components/ui -name '*.tsx' -exec grep -L 'React.forwardRef' {} \;

# Verify Radix UI dependencies are installed
npm list @radix-ui/react-dropdown-menu @radix-ui/react-dialog @radix-ui/react-select

# Check for accessibility violations in Radix-based components
npm run test:a11y

# Monitor bundle size impact
npm run build && npm run analyze:bundle
```

**Accept when:**
- All interactive components in `src/components/ui/` import from `@radix-ui` packages
- Component wrappers properly forward refs using `React.forwardRef`
- Radix UI dependencies are present in `package.json` and properly versioned
- Automated accessibility tests pass for all Radix-based components using jest-axe or similar tools
- Bundle size monitoring shows acceptable impact from Radix UI adoption
- No interactive components in `src/components/ui/` exist without Radix primitives unless documented exceptions are approved

<enforcement>
Claude Code MUST NOT skip or defer verification. All new interactive components in `src/components/ui/` MUST be verified against these rules before acceptance. Code review MUST check for proper Radix primitive usage and ref forwarding. CI pipeline MUST fail if interactive components bypass Radix primitives without documented exception. Accessibility testing MUST pass in CI for all Radix-based components.
</enforcement>