# Adopt TypeScript Interface Contracts for Public API Component Props: Public Facing Components

These rules are ALWAYS ACTIVE for all public-facing component APIs, external integration points, and reusable UI components exposed through the component library.

### Rules

- **R-TSIC-001** MUST: All public-facing components MUST define their props interface using TypeScript interface or type declarations.

### Verify

```bash
# Count exported Props interfaces in component directories
grep -r "export interface.*Props" src/components src/features --include="*.tsx" --include="*.ts" | wc -l

# Verify TypeScript strict mode compilation passes
npx tsc --noEmit --strict && echo 'Type checking passed'

# Check for React.FC components without typed props
grep -r "React.FC<" src/components src/features --include="*.tsx" | grep -v "React.FC<.*Props>" && echo 'Found components without typed props' || echo 'All FC components have typed props'
```

**Accept when:**
- All public components in src/components and src/features directories have exported TypeScript interface definitions for their props
- TypeScript strict mode compilation passes without errors for all component files
- Code review checklist includes verification that new components include proper interface contracts

<enforcement>
Claude Code MUST NOT skip or defer verification. TypeScript compilation in strict mode is mandatory in CI/CD pipeline. PR reviews are blocked until proper interface contracts are added. Automated checks must verify type coverage metrics.
</enforcement>