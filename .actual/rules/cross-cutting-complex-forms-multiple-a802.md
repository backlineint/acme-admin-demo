# Adopt React Hook Form with Zod for Client-Side Form Validation and State Management: Complex Forms Multiple

These rules are ALWAYS ACTIVE for all user-facing forms including authentication flows, settings and configuration forms, and data entry forms that accept user input requiring validation and submission.

### Rules

- **R-FORM-001** SHOULD: Complex forms with multiple steps or conditional fields SHOULD leverage React Hook Form's watch and control APIs for dynamic behavior.

### Verify

```bash
# Count useForm imports across the codebase
grep -r "useForm" src/ --include="*.tsx" --include="*.ts" | wc -l

# Verify @hookform/resolvers integration
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

<enforcement>
Claude Code MUST NOT skip or defer verification. All new forms and form modifications must comply with React Hook Form + Zod standards before acceptance.
</enforcement>