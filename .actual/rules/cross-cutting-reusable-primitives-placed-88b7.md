# Adopt React Component-Based Architecture for Public-Facing UI Components: Reusable Primitives Placed

These rules are ALWAYS ACTIVE for all React component files serving as user-facing interfaces, including error pages, authentication flows, dashboard analytics, data tables, forms, icons, layouts, and navigation components.

### Rules

- **R-REACT-001** MUST: Reusable UI primitives MUST be placed in the `components/` directory and remain domain-agnostic.
- **R-REACT-002** MUST: All UI components MUST be organized in either `features/` or `components/` directories according to their domain specificity.
- **R-REACT-003** MUST: Component props interfaces MUST be explicitly defined and exported for all public-facing components.
- **R-REACT-004** MUST: TypeScript compilation MUST pass without errors for all component files.
- **R-REACT-005** MUST: No circular dependencies MUST exist between feature components and shared primitives.
- **R-REACT-006** SHOULD: Use PascalCase for component files, kebab-case for directories, and descriptive names that reflect component purpose.
- **R-REACT-007** SHOULD: Document component props using JSDoc comments to provide inline documentation in IDEs.
- **R-REACT-008** SHOULD: Follow the rule of three: only extract reusable components after the pattern appears in three places.
- **R-REACT-009** MAY: Use Storybook or similar tools to document and showcase reusable component APIs.

### Verify

```bash
# Count feature components
find src/features -name '*.tsx' -type f | wc -l

# Count reusable primitives
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
- ESLint rules enforce import restrictions between features and shared components
- Component naming follows PascalCase for files and kebab-case for directories

<enforcement>
Claude Code MUST NOT skip or defer verification. TypeScript type checking and component organization verification are mandatory before accepting any changes to React component files.
</enforcement>