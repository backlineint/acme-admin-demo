# Adopt TypeScript Interface Contracts for Public API Component Props: Public Interfaces Exported

These rules are ALWAYS ACTIVE for all public-facing component APIs, external integration points, and reusable UI components exposed through the component library.

### Rules

- **R-PUB-001** MUST: Public API interfaces MUST be exported from their module to enable external consumption.

### Verify

```bash
# Count exported interface definitions for props
grep -r "export interface.*Props" src/components src/features --include="*.tsx" --include="*.ts" | wc -l

# Verify TypeScript strict mode compilation passes
npx tsc --noEmit --strict && echo 'Type checking passed'

# Check that all React.FC components have typed props
grep -r "React.FC<" src/components src/features --include="*.tsx" | grep -v "React.FC<.*Props>" && echo 'Found components without typed props' || echo 'All FC components have typed props'
```

**Accept when:**
- All public components in src/components and src/features directories have exported TypeScript interface definitions for their props
- TypeScript strict mode compilation passes without errors for all component files
- Code review checklist includes verification that new components include proper interface contracts

<enforcement>
Clause MUST NOT skip or defer verification. TypeScript compiler in strict mode is mandatory in CI/CD pipeline. ESLint rules enforcing explicit prop types must be configured. Code review checklist must require interface definitions for all new public components. Violations result in CI build failure and blocked PRs until proper interface contracts are added.
</enforcement>