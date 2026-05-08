# Adopt Client-Side Form State Management with React Hook Form and Zod Validation: Forms Implement Optimistic

Status: proposed
Date: 2024-01-15
Deciders: Detection Pipeline (automated)

## Context

- The application contains 21 files implementing form-based user interactions across authentication, settings, and data management features
- Forms require client-side validation, state management, and error handling to provide responsive user experience before server submission
- React applications need a standardized approach to handle form state, validation schemas, and submission workflows consistently
- The pattern appears across critical user flows including sign-in, user settings, profile management, task management, and user invitations
- Configuration and environment-specific form behaviors need to be managed consistently across the application runtime

## Problem Statement

Without a standardized form state management and validation approach, React applications face inconsistent validation logic, duplicated error handling code, poor user experience due to delayed validation feedback, and increased maintenance burden across multiple form implementations. The application needs a declarative, type-safe solution for managing form state and validation that integrates seamlessly with React's component model.

## Decision

1. MAY: Forms MAY implement optimistic UI updates for improved perceived performance while awaiting server responses

## Policy Block

- MAY Forms MAY implement optimistic UI updates for improved perceived performance while awaiting server responses

In scope:
- All user-facing forms in authentication flows (sign-in, sign-up, password reset)
- Settings and configuration forms (appearance, notifications, account, profile, display)
- Data management forms (task creation/editing, user invitations, import dialogs)
- Any component that collects structured user input requiring validation

Out of scope:
- Simple search inputs or filters that don't require validation
- Single-field inline editing without complex validation requirements
- Read-only display components or data tables without mutation capabilities
- Third-party form components with their own state management systems

Exceptions:
- EXC-001: Legacy forms in maintenance mode that are scheduled for deprecation within 6 months
- EXC-002: Third-party library integration requires alternative form management approach

## Rationale

- Pattern detected across 21 files with 91.27% confidence indicates strong architectural consistency and team adoption
- React Hook Form provides performant, uncontrolled component approach reducing re-renders and improving form performance
- Zod integration enables type-safe validation schemas with TypeScript inference, reducing runtime errors and improving developer experience
- Declarative validation approach centralizes validation logic, making it easier to test, maintain, and reuse across components

## Consequences

Positive:
- Consistent form handling patterns across the application reduce cognitive load for developers
- Type-safe validation schemas catch errors at compile time and provide excellent IDE autocomplete support
- Improved user experience through immediate client-side validation feedback before server round-trips
- Reduced boilerplate code for form state management, validation, and error handling
- Better testability through isolated validation schemas and predictable form state management

Negative:
- Additional dependencies (react-hook-form, zod, @hookform/resolvers) increase bundle size
- Learning curve for developers unfamiliar with React Hook Form API and Zod schema syntax
- Complex forms with dynamic fields may require advanced React Hook Form patterns (field arrays, conditional fields)
- Validation logic duplication between client-side (Zod) and server-side validation may occur if not carefully managed

## Alternatives

- Use Formik with Yup validation for form state management (rejected)
  Rejected because: Formik uses controlled components causing more re-renders; React Hook Form provides better performance with uncontrolled approach. Zod offers superior TypeScript integration compared to Yup.
  When valid: May be considered for projects already heavily invested in Formik ecosystem with migration costs outweighing benefits
- Implement custom form state management using React useState and useReducer (rejected)
  Rejected because: Custom implementation requires significant boilerplate, lacks ecosystem tooling, and increases maintenance burden. Does not provide built-in validation integration or error handling patterns.
  When valid: Only for extremely simple forms with 1-2 fields where library overhead is unjustified
- Use native HTML5 form validation without JavaScript framework (rejected)
  Rejected because: HTML5 validation provides limited customization, inconsistent browser behavior, and poor integration with React component lifecycle. Cannot handle complex validation logic or async validation.
  When valid: Acceptable for progressive enhancement scenarios where JavaScript may not be available

## Risks

- Validation schema drift between client-side Zod schemas and server-side validation logic leading to security vulnerabilities
  Mitigation: Implement shared validation schemas using Zod on both client and server (if using Node.js backend), or establish automated tests comparing validation behaviors. Document that client-side validation is for UX only, server-side is authoritative.
  Owner: Engineering team with security review
- Performance degradation in forms with hundreds of fields due to validation overhead
  Mitigation: Use React Hook Form's mode configuration (onBlur, onChange, onSubmit) to control validation timing. Implement field-level validation and debouncing for expensive validation operations. Profile and optimize validation schemas.
  Owner: Frontend performance team
- Inconsistent error message formatting and internationalization across forms
  Mitigation: Create centralized error message utilities and Zod custom error maps. Integrate with i18n library for translated validation messages. Establish error message style guide.
  Owner: Frontend architecture team

## Implementation Notes

- Create a shared form utilities module exporting common Zod schemas (email, password, phone) for reuse across forms
- Establish form component templates or generators to scaffold new forms with React Hook Form and Zod integration
- Document common patterns for field arrays, conditional validation, and async validation in team wiki or storybook
- Configure React Hook Form default options (mode, reValidateMode) at application root for consistent behavior
- Integrate form validation errors with application-wide error tracking and monitoring systems

## Continuation Context


Verify commands:
- grep -r "useForm" src/ --include="*.tsx" --include="*.ts" | wc -l
- grep -r "zodResolver" src/ --include="*.tsx" --include="*.ts" | wc -l
- grep -r "z\.object\|z\.string\|z\.number" src/ --include="*.tsx" --include="*.ts" | wc -l

Accept when:
- All form components use useForm hook from react-hook-form library
- Form validation schemas are defined using Zod (z.object, z.string, etc.) and integrated via zodResolver
- Form components display validation errors inline using form state error objects

## Enforcement

- Verified by: Automated code review checks scanning for form components without React Hook Form usage
- Verified by: ESLint custom rules detecting form submissions without Zod validation schemas
- Verified by: Pull request checklist requiring form validation testing and error handling verification
- Verified by: Periodic architecture audits reviewing form implementations against this ADR
- Violation handling: CI pipeline warnings for forms not following React Hook Form pattern
- Violation handling: Code review rejection for new forms without proper validation schemas
- Violation handling: Technical debt tickets created for non-compliant legacy forms with prioritized remediation
- Violation handling: Architecture review required for any exceptions to this pattern
- Exception process: Developer submits exception request documenting specific constraints preventing compliance
- Exception process: Tech lead reviews technical justification and evaluates alternative approaches
- Exception process: Architecture team approves exception with documented rationale and migration plan if applicable
- Exception process: Exception documented in code comments and tracked in architecture decision log