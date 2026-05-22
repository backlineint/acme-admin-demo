# Adopt React Component-Based Architecture for Public-Facing UI Components: Public Facing Components

These rules are ALWAYS ACTIVE for all React components serving as user-facing interfaces, including error pages, authentication flows, dashboard analytics, data tables, forms, interactive UI elements, icon components, and layout/navigation components.

### Rules

- **R-PUBUI-001** MUST: All public-facing UI components MUST be implemented as React functional components with TypeScript type definitions.

### Verify

```bash
# Count feature components
find src/features -name '*.tsx' -type f | wc -l

# Count reusable primitive components
find src/components -name '*.tsx' -type f | wc -l

# Count functional components
grep -r 'export.*function.*Component' src/features src/components | wc -l

# Verify TypeScript type checking passes
npx tsc --noEmit
```

**Accept when:**
- All UI components are organized in either `features/` or `components/` directories according to their domain specificity
- TypeScript compilation passes without errors for all component files
- Component props interfaces are explicitly defined and exported for all public-facing components
- No circular dependencies exist between feature components and shared primitives

<enforcement>
Claude Code MUST NOT skip or defer verification. TypeScript type checking and component organization verification are mandatory before accepting any changes to public-facing UI components.
</enforcement>