# Adopt TypeScript Interface Contracts for Public API Component Props: Complex Prop Types

These rules are ALWAYS ACTIVE for all public-facing component APIs, external integration points, and reusable UI components exposed through the component library, including React components exported from feature modules, UI components in the component library, custom icon components and brand assets, data table components and their configuration interfaces, error boundary and error display components, authentication and user management UI components, and dashboard and analytics visualization components.

### Rules

- **R-TYPESCRIPT-PROPS-001** SHOULD: Complex prop types SHOULD be extracted into separate named interfaces rather than inline type definitions.

### Verify

```bash
# Count exported Props interfaces across component directories
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
- Interface naming follows convention: ComponentNameProps (e.g., ButtonProps, DataTableProps)
- Interfaces are co-located with component implementations in the same file
- Interfaces are exported from index files to create a clear public API surface

<enforcement>
Claude Code MUST NOT skip or defer verification. TypeScript compiler in strict mode MUST pass as part of CI/CD pipeline. Code review MUST verify interface contracts are present for all new public components. Violations result in CI build failure and blocked PR reviews until proper interface contracts are added.
</enforcement>