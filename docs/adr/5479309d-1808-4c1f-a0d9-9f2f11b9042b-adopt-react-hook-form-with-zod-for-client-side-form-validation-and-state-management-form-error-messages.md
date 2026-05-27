# Adopt React Hook Form with Zod for Client-Side Form Validation and State Management: Form Error Messages

Status: proposed
Date: 2024-01-15
Deciders: Detection Pipeline (automated)

## Context

- The application contains multiple forms across authentication, user management, settings, and task management features requiring consistent validation and state management
- Forms include sign-in, OTP verification, forgot password, user invitations, account settings, appearance preferences, display settings, profile updates, notifications, and task mutations
- A standardized approach to form handling reduces code duplication, improves maintainability, and ensures consistent user experience across all form interactions
- Client-side validation needs to be type-safe and integrated with TypeScript to catch errors at compile time and provide better developer experience
- The pattern appears in 20 files with 91.33% confidence, indicating widespread adoption of a consistent form handling strategy

## Problem Statement

Without a standardized form validation and state management solution, the application faces inconsistent validation logic, duplicated form handling code, poor type safety, and increased maintenance burden across multiple feature areas. A unified approach is needed to ensure forms behave predictably, validate data consistently, and provide a seamless developer experience.

## Decision

1. MUST: Form error messages MUST be displayed to users using React Hook Form's error state and field-level error rendering

## Policy Block

- MUST Form error messages MUST be displayed to users using React Hook Form's error state and field-level error rendering

In scope:
- All user-facing forms including authentication flows (sign-in, sign-up, OTP, password reset)
- Settings and configuration forms (account, profile, appearance, display, notifications)
- Data entry forms (task creation/editing, user invitations, user management)
- Any component that accepts user input requiring validation and submission

Out of scope:
- Simple input components that don't require validation (search boxes, filters)
- Read-only display components or data tables
- Forms in legacy code scheduled for deprecation
- Prototype or experimental features not yet integrated into the main application

Exceptions:
- EXC-001: A third-party library or external integration requires a different form handling approach that cannot be adapted to React Hook Form
- EXC-002: Performance-critical forms with extremely high frequency updates where React Hook Form's overhead is measurably problematic

## Rationale

- React Hook Form provides excellent performance through uncontrolled components and minimal re-renders, making it suitable for complex forms with many fields
- Zod integration ensures type-safe validation schemas that are automatically synchronized with TypeScript types, reducing runtime errors and improving developer experience
- The pattern's 91.33% confidence across 20 files demonstrates proven adoption and effectiveness in the codebase, indicating this is already the de facto standard
- Standardizing on a single form solution reduces cognitive load for developers, simplifies onboarding, and makes code reviews more efficient

## Consequences

Positive:
- Consistent form behavior and validation across all features improves user experience and reduces confusion
- Type-safe form handling catches validation errors at compile time, reducing production bugs
- Reduced code duplication through reusable validation schemas and form patterns
- Better developer experience with excellent TypeScript support and clear API patterns
- Improved form performance through optimized re-rendering and uncontrolled component strategy

Negative:
- Learning curve for developers unfamiliar with React Hook Form or Zod APIs
- Additional bundle size from React Hook Form and Zod dependencies (though both are relatively lightweight)
- Migration effort required for any existing forms using different validation approaches
- Potential over-engineering for very simple forms with minimal validation requirements

## Alternatives

- Use Formik with Yup for form handling and validation (rejected)
  Rejected because: Formik has performance issues with large forms due to controlled component approach and more frequent re-renders. React Hook Form provides better performance and smaller bundle size.
  When valid: May be considered if the team has extensive Formik expertise and performance is not a concern
- Implement custom form handling with native React state and manual validation (rejected)
  Rejected because: Custom solutions lead to code duplication, inconsistent validation patterns, and increased maintenance burden. Lacks type safety and requires significant development effort to match library features.
  When valid: Only appropriate for extremely simple forms with no validation requirements
- Use native HTML5 form validation without JavaScript libraries (rejected)
  Rejected because: HTML5 validation lacks flexibility for complex business rules, provides inconsistent UX across browsers, and doesn't integrate well with React's component model or TypeScript type system.
  When valid: Could be used for basic forms in server-rendered pages outside the React application

## Risks

- React Hook Form API changes in major version updates could require significant refactoring across all forms
  Mitigation: Pin major versions in package.json, thoroughly test upgrades in staging environment, and maintain comprehensive test coverage for form components
  Owner: Frontend Engineering Team
- Complex nested forms or dynamic field arrays may encounter edge cases or performance issues with React Hook Form
  Mitigation: Establish patterns and examples for complex form scenarios, conduct performance testing for large forms, and document workarounds for known limitations
  Owner: Frontend Architecture Team
- Developers may bypass the standard approach for quick fixes, leading to inconsistent form implementations
  Mitigation: Implement linting rules to detect non-standard form patterns, include form standards in code review checklist, and provide clear documentation with examples
  Owner: Engineering Team Leads

## Implementation Notes

- Create a shared form component library with pre-configured React Hook Form wrappers for common input types (text, select, checkbox, etc.)
- Establish a convention for organizing validation schemas: colocate with components for feature-specific forms, or centralize in a schemas directory for shared validation logic
- Document common patterns for form submission handling, including error handling, loading states, and success feedback
- Provide example implementations for complex scenarios: multi-step forms, conditional fields, dynamic arrays, and async validation
- Set up ESLint rules or custom linting to detect forms not using the standard React Hook Form + Zod pattern

## Continuation Context


Verify commands:
- grep -r "useForm" src/ --include="*.tsx" --include="*.ts" | wc -l
- grep -r "@hookform/resolvers" src/ --include="*.tsx" --include="*.ts" | wc -l
- grep -r "z\.object\|z\.string\|z\.number" src/ --include="*.ts" --include="*.tsx" | wc -l
- find src/ -name "*form*.tsx" -o -name "*Form*.tsx" | xargs grep -L "useForm" || echo "All forms use React Hook Form"

Accept when:
- All form components in the codebase import and use useForm from react-hook-form
- Validation schemas are defined using Zod (z.object, z.string, etc.) and integrated via zodResolver
- No form components use alternative form libraries (Formik, Redux Form) or manual state management for validation
- Form components derive TypeScript types from Zod schemas using z.infer

## Enforcement

- Verified by: Code review checklist includes verification of React Hook Form + Zod usage for all new forms
- Verified by: CI pipeline runs grep-based checks to detect forms not following the standard pattern
- Verified by: Automated tests verify form validation behavior matches Zod schema definitions
- Verified by: Pull request templates include a section for confirming form implementation standards
- Violation handling: Pull requests with non-compliant forms are blocked until refactored to use React Hook Form + Zod
- Violation handling: Existing violations are tracked in technical debt backlog with priority based on form complexity and usage frequency
- Violation handling: Monthly architecture reviews assess compliance metrics and identify patterns of non-compliance
- Violation handling: Violations in critical paths (authentication, payment) are escalated for immediate remediation
- Exception process: Developer submits exception request to tech lead with detailed justification and alternative approach
- Exception process: Tech lead reviews request considering performance, maintainability, and integration constraints
- Exception process: Approved exceptions are documented in code comments and tracked in architecture decision log
- Exception process: Exceptions are reviewed quarterly to determine if they can be migrated to the standard approach