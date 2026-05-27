# Adopt React Component-Based Architecture for Public-Facing UI Components: Data Type Definitions

These rules are ALWAYS ACTIVE for all React components serving as user-facing interfaces, including error pages, authentication flows, dashboard analytics, data tables, forms, interactive UI elements, icon components, and layout/navigation components.

### Rules

- **R-REACT-DT-001** SHOULD: Data type definitions for features SHOULD be separated into dedicated type files (e.g., chat-types.ts).

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
- Data type definitions are located in dedicated type files (e.g., `*-types.ts`) separate from component implementations

<enforcement>
Claude Code MUST NOT skip or defer verification. TypeScript type checking and component organization verification are mandatory before accepting changes to React components.
</enforcement>