# Adopt React Component-Based Architecture for Public-Facing UI Components: Component Props Interfaces

These rules are ALWAYS ACTIVE for all React components serving as user-facing interfaces, including error pages, authentication flows, dashboard analytics, data tables, icon components, layout and navigation components, and all feature-domain UI components.

### Rules

- **R-REACT-PROPS-001** SHOULD: Component props interfaces SHOULD be explicitly defined and exported for external consumption.

### Verify

```bash
# Count feature components
find src/features -name '*.tsx' -type f | wc -l

# Count reusable primitive components
find src/components -name '*.tsx' -type f | wc -l

# Count functional components with explicit exports
grep -r 'export.*function.*Component' src/features src/components | wc -l

# Verify TypeScript type checking passes
npx tsc --noEmit
```

**Accept when:**
- All UI components are organized in either `features/` or `components/` directories according to their domain specificity
- TypeScript compilation passes without errors for all component files
- Component props interfaces are explicitly defined and exported for all public-facing components
- No circular dependencies exist between feature components and shared primitives
- Component naming follows conventions: PascalCase for component files, kebab-case for directories
- JSDoc comments document component props for IDE inline documentation

<enforcement>
Claude Code MUST NOT skip or defer verification. TypeScript type checking and component organization verification are mandatory before accepting any changes to React components in scope.
</enforcement>