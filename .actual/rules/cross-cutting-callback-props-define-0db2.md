# Adopt TypeScript Interface Contracts for Public API Component Props: Callback Props Define

These rules are ALWAYS ACTIVE for all React components exported from feature modules, reusable UI components in the component library, custom icon components, data table components, error boundary components, authentication UI components, and dashboard visualization components.

### Rules

- **R-CALLBACK-001** SHOULD: Callback props SHOULD define explicit function signatures including parameter and return types.

### Verify

```bash
# Count exported interface definitions for props
grep -r "export interface.*Props" src/components src/features --include="*.tsx" --include="*.ts" | wc -l

# Verify TypeScript strict mode compilation passes
npx tsc --noEmit --strict && echo 'Type checking passed'

# Check for React.FC components without typed props
grep -r "React.FC<" src/components src/features --include="*.tsx" | grep -v "React.FC<.*Props>" && echo 'Found components without typed props' || echo 'All FC components have typed props'
```

**Accept when:**
- All public components in src/components and src/features directories have exported TypeScript interface definitions for their props
- TypeScript strict mode compilation passes without errors for all component files
- All callback props include explicit function signatures with parameter and return types
- Code review checklist includes verification that new components include proper interface contracts

<enforcement>
Claude Code MUST NOT skip or defer verification. TypeScript compiler in strict mode MUST pass as part of CI/CD pipeline. PR reviews MUST be blocked until proper interface contracts with typed callbacks are added. Automated PR checks MUST verify type coverage metrics.
</enforcement>