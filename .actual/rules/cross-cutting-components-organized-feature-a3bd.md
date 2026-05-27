# Adopt React Component-Based Architecture for Public-Facing UI Components: Components Organized Feature

These rules are ALWAYS ACTIVE for all React components serving as user-facing interfaces, including error pages, authentication flows, dashboard analytics, data tables, forms, interactive UI elements, icon components, and layout/navigation components.

### Rules

- **R-COMP-001** MUST: Components MUST be organized by feature domain (e.g., features/errors, features/auth, features/dashboard) with clear directory structure.
- **R-COMP-002** MUST: All UI components MUST be organized in either features/ or components/ directories according to their domain specificity.
- **R-COMP-003** MUST: Component props interfaces MUST be explicitly defined and exported for all public-facing components.
- **R-COMP-004** MUST: TypeScript compilation MUST pass without errors for all component files.
- **R-COMP-005** MUST: No circular dependencies MUST exist between feature components and shared primitives.
- **R-COMP-006** SHOULD: Use PascalCase for component files, kebab-case for directories, and descriptive names that reflect component purpose.
- **R-COMP-007** SHOULD: Document component props using JSDoc comments to provide inline documentation in IDEs.
- **R-COMP-008** SHOULD: Follow the rule of three: only extract reusable components after the pattern appears in three places.
- **R-COMP-009** MAY: Use a component template or generator to scaffold new components with the correct directory structure and TypeScript boilerplate.

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
- All UI components are organized in either features/ or components/ directories according to their domain specificity
- TypeScript compilation passes without errors for all component files
- Component props interfaces are explicitly defined and exported for all public-facing components
- No circular dependencies exist between feature components and shared primitives
- ESLint rules enforce import restrictions between features and shared components
- Component naming conventions follow PascalCase for files and kebab-case for directories

<enforcement>
Claude Code MUST NOT skip or defer verification. TypeScript type checking and component organization verification are mandatory before accepting any changes to React components in scope.
</enforcement>