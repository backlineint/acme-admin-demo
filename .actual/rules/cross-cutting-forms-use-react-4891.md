# Adopt React Hook Form with Zod for Client-Side Form Validation and State Management: Forms Use React

These rules are ALWAYS ACTIVE for all form components and user input handling code in the application.

### Rules

- **R-FORM-001** MUST: All forms MUST use React Hook Form for form state management and submission handling.
- **R-FORM-002** MUST: All form validation schemas MUST be defined using Zod and integrated via zodResolver.
- **R-FORM-003** MUST: Form components MUST derive TypeScript types from Zod schemas using z.infer.
- **R-FORM-004** SHOULD: Validation schemas should be colocated with components for feature-specific forms, or centralized in a schemas directory for shared validation logic.
- **R-FORM-005** SHOULD: Complex form scenarios (multi-step forms, conditional fields, dynamic arrays, async validation) should follow established patterns and examples.

### Verify

```bash
# Count React Hook Form usage
grep -r "useForm" src/ --include="*.tsx" --include="*.ts" | wc -l

# Verify Zod resolver integration
grep -r "@hookform/resolvers" src/ --include="*.tsx" --include="*.ts" | wc -l

# Count Zod schema definitions
grep -r "z\.object\|z\.string\|z\.number" src/ --include="*.ts" --include="*.tsx" | wc -l

# Verify all form files use React Hook Form
find src/ -name "*form*.tsx" -o -name "*Form*.tsx" | xargs grep -L "useForm" || echo "All forms use React Hook Form"
```

**Accept when:**
- All form components in the codebase import and use useForm from react-hook-form
- Validation schemas are defined using Zod (z.object, z.string, etc.) and integrated via zodResolver
- No form components use alternative form libraries (Formik, Redux Form) or manual state management for validation
- Form components derive TypeScript types from Zod schemas using z.infer
- Complex form patterns are documented and consistently applied across the codebase

<enforcement>
Claude Code MUST NOT skip or defer verification of React Hook Form and Zod usage in form components. All new forms and form modifications MUST comply with R-FORM-001 through R-FORM-005. Violations in critical paths (authentication, payment) require immediate escalation.
</enforcement>