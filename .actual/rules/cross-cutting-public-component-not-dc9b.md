# Adopt TypeScript Interface Contracts for Public API Component Props: Public Component Not

These rules are ALWAYS ACTIVE for all public-facing component APIs, external integration points, and reusable UI components exposed through the component library, including React components exported from feature modules, reusable UI components, custom icon components, data table components, error boundary components, authentication UI components, and dashboard visualization components.

### Rules

- **R-TS-001** MUST NOT: Public component APIs MUST NOT use 'any' type for props without explicit justification and documentation.

### Verify

```bash
# Count exported interface definitions for component props
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
- No public component APIs use 'any' type for props without documented exceptions

<enforcement>
Claude Code MUST NOT skip or defer verification. TypeScript compiler checks in strict mode are mandatory as part of CI/CD pipeline. PR reviews must be blocked until proper interface contracts are added. Violations result in CI build failure.
</enforcement>