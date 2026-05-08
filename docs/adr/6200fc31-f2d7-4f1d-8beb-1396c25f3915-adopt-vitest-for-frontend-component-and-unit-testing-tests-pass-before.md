# Adopt Vitest for Frontend Component and Unit Testing: Tests Pass Before

Status: proposed
Date: 2024-01-15
Deciders: Detection Pipeline (automated)

## Context

- The codebase contains 19 test files with .test.tsx and .test.ts extensions, indicating a comprehensive testing strategy for React components and TypeScript utilities
- Test files are co-located with source code across multiple feature domains (users, tasks, auth) and shared components, suggesting a modular architecture requiring consistent testing patterns
- The presence of component tests for forms, dialogs, and hooks indicates a need for DOM rendering and user interaction testing capabilities
- Modern frontend applications require fast test execution and hot module replacement during development to maintain developer productivity
- The testing infrastructure must support both component testing (with JSX/TSX) and unit testing for utility functions and business logic

## Problem Statement

Frontend applications require a testing framework that can efficiently test React components with TypeScript, provide fast feedback during development, integrate seamlessly with CI/CD pipelines, and support both component rendering tests and unit tests for utilities. The framework must handle JSX/TSX syntax, provide good developer experience with watch mode, and execute tests quickly enough to be run on every commit.

## Decision

1. MUST: All tests MUST pass before code can be merged to the main branch

## Policy Block

- MUST All tests MUST pass before code can be merged to the main branch

In scope:
- All React component files (.tsx)
- All TypeScript utility and library files (.ts)
- Form components and validation logic
- Dialog and modal components
- Custom React hooks
- Authentication and authorization flows
- Data fetching and state management utilities

Out of scope:
- End-to-end tests (handled by separate E2E framework)
- Visual regression tests
- Performance benchmarking tests
- Backend API tests
- Database integration tests

Exceptions:
- EXC-001: Prototype or experimental code in a feature branch not intended for production
- EXC-002: Third-party component wrappers with minimal logic that only pass props through

## Rationale

- Pattern detected across 19 files with 91.46% confidence indicates a well-established testing practice in the codebase
- Vitest provides native ESM support, TypeScript integration, and compatibility with Vite's build system, enabling faster test execution than Jest
- Co-locating tests with source code improves discoverability and makes it easier to maintain tests alongside implementation changes
- Comprehensive test coverage across components, hooks, and utilities reduces regression risk and enables confident refactoring

## Consequences

Positive:
- Fast test execution enables rapid feedback loops during development and in CI/CD pipelines
- High test coverage across 19+ files provides confidence in code changes and reduces production bugs
- Co-located test files make it easy for developers to find and update tests when modifying code
- Consistent testing patterns across features (users, tasks, auth) reduce cognitive load and onboarding time
- Native TypeScript and JSX support eliminates configuration complexity and transformation overhead

Negative:
- Maintaining test files alongside source code increases the number of files developers must manage
- Test suite execution time will grow linearly with codebase size, potentially slowing CI/CD pipelines
- Developers must learn Vitest-specific APIs and testing patterns, adding to onboarding complexity
- Mocking complex dependencies (API clients, authentication) requires additional setup and maintenance

## Alternatives

- Use Jest as the test runner instead of Vitest (rejected)
  Rejected because: Jest requires additional configuration for ESM and TypeScript, has slower execution times, and lacks native Vite integration
  When valid: For projects not using Vite or requiring Jest-specific ecosystem plugins
- Separate test files into a dedicated __tests__ directory instead of co-location (rejected)
  Rejected because: Reduces discoverability and makes it harder to maintain tests alongside implementation changes
  When valid: For projects with very large test files that would clutter source directories
- Use React Testing Library with Vitest for component testing (accepted)
  When valid: This is the recommended approach for testing React components with user-centric queries

## Risks

- Test suite execution time may exceed acceptable thresholds as the codebase grows beyond 100+ test files
  Mitigation: Implement test parallelization, use test sharding in CI, and monitor test execution metrics to identify slow tests
  Owner: Engineering team
- Developers may skip writing tests under time pressure, leading to coverage gaps
  Mitigation: Enforce minimum coverage thresholds in CI (e.g., 80%), make test failures block merges, and include test writing in definition of done
  Owner: Engineering team and tech leads
- Brittle tests that break frequently due to implementation details may reduce confidence in the test suite
  Mitigation: Follow testing best practices (test behavior not implementation, use semantic queries), conduct test code reviews, and refactor flaky tests promptly
  Owner: Engineering team

## Implementation Notes

- Configure Vitest in vitest.config.ts with appropriate test environment (jsdom for component tests, node for utilities)
- Set up React Testing Library with @testing-library/react and @testing-library/user-event for component interaction testing
- Create test utilities and custom render functions to reduce boilerplate across test files
- Configure coverage reporting with v8 or istanbul provider and set minimum coverage thresholds
- Add npm scripts for running tests in watch mode (npm test) and CI mode (npm run test:ci)
- Document testing patterns and examples in the project README or testing guide

## Continuation Context


Verify commands:
- find src -name '*.test.tsx' -o -name '*.test.ts' | wc -l
- grep -r "import.*vitest" src/**/*.test.{ts,tsx} | head -5
- npm test -- --run --reporter=verbose 2>&1 | grep -E '(Test Files|Tests|passed)'

Accept when:
- At least 15 test files with .test.tsx or .test.ts extensions exist in the src directory
- Test files import from 'vitest' package for test definitions and assertions
- Running the test command executes all tests successfully with passing status

## Enforcement

- Verified by: CI pipeline runs all tests on every pull request and blocks merge if tests fail
- Verified by: Code coverage reports are generated and reviewed to ensure minimum thresholds are met
- Verified by: Pull request templates include checklist item for adding/updating tests
- Verified by: Automated checks verify that new component files have corresponding test files
- Violation handling: Pull requests with failing tests cannot be merged until tests are fixed
- Violation handling: Pull requests that decrease coverage below threshold are flagged for review
- Violation handling: Missing test files trigger automated comments requesting test coverage
- Violation handling: Repeated violations are escalated to tech lead for discussion and remediation plan
- Exception process: Developer documents exception request in pull request description with justification
- Exception process: Tech lead reviews exception request and approves or requests changes
- Exception process: Approved exceptions are documented in code comments and tracked in technical debt backlog
- Exception process: Exceptions are reviewed quarterly to determine if they can be resolved