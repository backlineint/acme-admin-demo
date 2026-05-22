# Adopt TypeScript Interface Contracts for Public API Component Props: Component Prop Interfaces

These rules are ALWAYS ACTIVE for all React components exported from feature modules, reusable UI components in the component library, custom icon components and brand assets, data table components and their configuration interfaces, error boundary and error display components, authentication and user management UI components, and dashboard and analytics visualization components.

### Rules

- **R-COMP-001** MUST: Component prop interfaces MUST explicitly mark optional properties using the '?' modifier

### Verify

```bash
# Count exported prop interfaces across component directories
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
- All optional props in component interfaces are explicitly marked with the '?' modifier

<enforcement>
Claude Code MUST NOT skip or defer verification. TypeScript compilation and interface contract verification are mandatory before accepting component implementations.
</enforcement>