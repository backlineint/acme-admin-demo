# Adopt React Testing Library with Vitest for Component and Unit Testing: Tests Pass Pipeline

Status: proposed
Date: 2024-01-20
Deciders: Detection Pipeline (automated)

## Context

- The codebase contains 17 test files following a consistent testing pattern across components, hooks, utilities, and stores
- Test files are co-located with source files using the .test.tsx and .test.ts naming convention, indicating a strong testing culture
- The project uses React components with TypeScript, requiring a testing framework that supports both JSX/TSX and modern JavaScript features
- Testing spans multiple architectural layers including UI components (confirm-dialog, password-input, otp-form), business logic (auth-store), utilities (utils, handle-server-error), and custom hooks (use-table-url-state)
- The pattern shows 91.44% confidence across 17 files, indicating this is an established and consistently applied testing standard

## Problem Statement

Without a standardized testing framework and methodology, the codebase risks inconsistent test quality, difficult maintenance, slow test execution, and inadequate coverage across different architectural layers. Teams need a unified approach to testing React components, TypeScript utilities, custom hooks, and state management that integrates seamlessly with the CI/CD pipeline.

## Decision

1. MUST: All tests MUST pass in the CI/CD pipeline before code can be merged to main branches

## Policy Block

- MUST All tests MUST pass in the CI/CD pipeline before code can be merged to main branches

In scope:
- All React components in the src/components directory
- All feature components in src/features/**/components
- All custom hooks in src/hooks
- All utility functions in src/lib
- All state management stores in src/stores
- Form components and validation logic
- Dialog and drawer components
- Authentication and authorization flows

Out of scope:
- Third-party library code
- Auto-generated code or build artifacts
- Configuration files and environment setup
- Simple type definitions without logic
- Deprecated or legacy code scheduled for removal

Exceptions:
- EXC-001: Prototype or spike code explicitly marked as experimental
- EXC-002: Pure presentational components with no logic (props pass-through only)

## Rationale

- React Testing Library promotes testing best practices by focusing on user behavior rather than implementation details, leading to more maintainable and resilient tests
- Vitest provides fast test execution with native ESM support, TypeScript integration, and a Jest-compatible API, making it ideal for modern React applications
- Co-locating test files with source code improves discoverability and makes it easier to maintain tests alongside implementation changes
- The pattern detected across 17 files with 91.44% confidence demonstrates this approach is already successfully adopted and proven effective in the codebase
- Consistent testing patterns across components, hooks, utilities, and stores enable comprehensive coverage and reduce cognitive load for developers

## Consequences

Positive:
- Consistent testing approach across the entire codebase reduces learning curve for new team members
- Fast test execution with Vitest enables rapid feedback during development and CI/CD pipelines
- User-centric testing with React Testing Library leads to more reliable tests that catch real user-facing issues
- Co-located test files improve code organization and make it easier to identify untested code
- Strong test coverage across components, hooks, and utilities increases confidence in refactoring and feature development

Negative:
- Initial setup and learning curve for developers unfamiliar with React Testing Library or Vitest
- Test maintenance overhead increases as the codebase grows, requiring discipline to keep tests up-to-date
- Some complex component interactions may require additional setup or mocking, increasing test complexity
- Test execution time will increase proportionally with codebase size, potentially slowing down CI/CD if not optimized

## Alternatives

- Use Jest with Enzyme for testing React components (rejected)
  Rejected because: Enzyme focuses on implementation details rather than user behavior, leading to brittle tests. Enzyme is also no longer actively maintained for React 18+
  When valid: Legacy codebases already heavily invested in Enzyme with no migration path
- Use Cypress Component Testing for all component tests (rejected)
  Rejected because: Cypress Component Testing is slower than Vitest with React Testing Library and adds unnecessary overhead for unit-level component tests. Better suited for integration/E2E testing
  When valid: When testing complex component interactions that require full browser environment
- Use Jest instead of Vitest as the test runner (rejected)
  Rejected because: Jest requires additional configuration for ESM and TypeScript support, while Vitest provides native support out of the box with faster execution
  When valid: Projects with existing Jest infrastructure and no performance concerns

## Risks

- Test coverage may degrade over time if not actively monitored and enforced
  Mitigation: Implement coverage thresholds in CI/CD pipeline and require coverage reports in pull requests
  Owner: Engineering team and tech leads
- Developers may write tests that pass but don't actually verify correct behavior
  Mitigation: Establish code review guidelines for test quality, provide training on testing best practices, and use mutation testing to verify test effectiveness
  Owner: Engineering team and QA
- Test execution time may become prohibitive as the test suite grows
  Mitigation: Implement test parallelization, optimize slow tests, and consider test sharding in CI/CD pipeline
  Owner: DevOps and engineering team

## Implementation Notes

- Set up Vitest configuration with React Testing Library in vitest.config.ts, including jsdom environment and necessary plugins
- Create shared test utilities and custom render functions (e.g., renderWithProviders) to handle common setup like Redux stores, React Router, and theme providers
- Establish naming conventions for test files (.test.tsx for components, .test.ts for utilities) and ensure they are co-located with source files
- Configure CI/CD pipeline to run tests on every pull request and block merges if tests fail or coverage drops below thresholds
- Document testing patterns and examples in team documentation, including how to test common scenarios like forms, async operations, and user interactions

## Continuation Context


Verify commands:
- find src -type f \( -name '*.tsx' -o -name '*.ts' \) ! -name '*.test.*' ! -name '*.spec.*' -exec sh -c 'test -f "${1%.tsx}.test.tsx" -o -f "${1%.ts}.test.ts" || echo "Missing test: $1"' _ {} \;
- grep -r "@testing-library/react" package.json && grep -r "vitest" package.json
- vitest run --coverage --reporter=json --reporter=text
- grep -l "describe\|test\|it" src/**/*.test.{ts,tsx} | wc -l

Accept when:
- All component files (.tsx) have corresponding .test.tsx files with at least one test case
- All utility and hook files (.ts) have corresponding .test.ts files with at least one test case
- Test suite executes successfully with vitest run command and all tests pass
- Package.json contains both @testing-library/react and vitest as dependencies
- CI/CD pipeline includes test execution step that blocks merges on test failures

## Enforcement

- Verified by: Automated CI/CD pipeline checks that run tests on every pull request
- Verified by: Code coverage reports generated by Vitest and reviewed during pull request process
- Verified by: Static analysis tools that verify test file existence for each source file
- Verified by: Code review process that verifies test quality and coverage for new features
- Violation handling: Pull requests without corresponding tests are blocked from merging
- Violation handling: CI/CD pipeline fails if test suite does not pass or coverage drops below threshold
- Violation handling: Automated comments on pull requests identify missing test files
- Violation handling: Tech lead review required for any exceptions to testing requirements
- Exception process: Developer documents reason for exception in pull request description
- Exception process: Tech lead or senior engineer reviews and approves exception request
- Exception process: Exception is documented as technical debt in project tracking system with timeline for resolution
- Exception process: Exceptions are reviewed quarterly to ensure they are still valid and debt is being addressed