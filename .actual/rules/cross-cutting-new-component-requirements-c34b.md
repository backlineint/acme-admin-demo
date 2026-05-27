# Adopt Radix UI Primitives as Standard Component Library: New Component Requirements

These rules are ALWAYS ACTIVE for all interactive UI components in `src/components/ui/` and any new component requirements that involve keyboard navigation, focus management, ARIA attributes, form controls, overlay components, navigation components, or feedback components.

### Rules

- **R-RADIX-001** SHOULD: New UI component requirements SHOULD first evaluate if a Radix primitive exists before implementing custom solutions.

### Verify

```bash
# Count Radix UI imports in component library
grep -r "@radix-ui/react-" src/components/ui/ | wc -l

# Identify components missing React.forwardRef
find src/components/ui -name '*.tsx' -exec grep -L 'React.forwardRef' {} \;

# Verify Radix UI dependencies are installed
npm list @radix-ui/react-dropdown-menu @radix-ui/react-dialog @radix-ui/react-select
```

**Accept when:**
- All interactive components in `src/components/ui/` import from `@radix-ui` packages
- Component wrappers properly forward refs using `React.forwardRef`
- Radix UI dependencies are present in `package.json` and properly versioned
- Automated accessibility tests pass for all Radix-based components using jest-axe or similar tools
- New interactive components follow the naming convention: file names match Radix primitive names in kebab-case (e.g., `dropdown-menu.tsx`)
- Components are documented with Storybook or similar, showing basic usage and accessibility features

<enforcement>
Claude Code MUST NOT skip or defer verification. CI pipeline MUST fail if new interactive components in `src/components/ui/` don't use Radix primitives without documented exception. Code review MUST block merge if accessibility attributes are overridden or removed without justification. Architecture review is REQUIRED for custom component implementations that bypass Radix primitives.
</enforcement>