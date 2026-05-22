# Adopt TypeScript Interface Contracts for Public API Component Props: Components Extend Base

These rules are ALWAYS ACTIVE for all React components exported from feature modules, reusable UI components in the component library, custom icon components, data table components, error boundary components, authentication UI components, and dashboard visualization components.

### Rules

- **R-TSIC-001** MAY: Components MAY extend base interfaces from shared libraries (e.g., React.HTMLAttributes) to inherit standard props.

### Verify

```bash
# Count exported interface definitions for component props
grep -r "export interface.*Props" src/components src/features --include="*.tsx" --include="*.ts" | wc -l

# Verify TypeScript strict mode compilation passes
npx tsc --noEmit --strict && echo 'Type checking passed'

# Verify all FC components have typed props
grep -r "React.FC<" src/components src/features --include="*.tsx" | grep -v "React.FC<.*Props>" && echo 'Found components without typed props' || echo 'All FC components have typed props'
```

**Accept when:**
- All public components in src/components and src/features directories have exported TypeScript interface definitions for their props
- TypeScript strict mode compilation passes without errors for all component files
- Code review checklist includes verification that new components include proper interface contracts

<enforcement>
Claude Code MUST NOT skip or defer verification. TypeScript compilation in strict mode is mandatory as part of CI/CD pipeline. PR reviews are blocked until proper interface contracts are added to all new public components.
</enforcement>