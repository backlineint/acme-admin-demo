# Adopt React Component-Based Architecture for Public-Facing UI Components: Icon Components Brand

These rules are ALWAYS ACTIVE for all React components serving as user-facing interfaces, including error pages, authentication flows, dashboard analytics, data tables, icon components, brand assets, layout and navigation components.

### Rules

- **R-ICON-001** MUST: Icon components and brand assets MUST be isolated in the assets/ directory with consistent naming conventions.

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
- Icon components and brand assets are isolated in the assets/ directory with consistent naming conventions

<enforcement>
Claude Code MUST NOT skip or defer verification. All TypeScript type checking and directory organization checks MUST pass before accepting changes.
</enforcement>