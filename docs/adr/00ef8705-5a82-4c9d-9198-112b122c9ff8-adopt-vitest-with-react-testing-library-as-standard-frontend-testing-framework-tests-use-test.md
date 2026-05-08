# Adopt Vitest with React Testing Library as Standard Frontend Testing Framework: Tests Use Test

Status: proposed
Date: 2024-01-20
Deciders: Detection Pipeline (automated)

## Activation

This ADR is ACTIVE for all frontend test file creation and modification. All test files MUST follow the patterns and conventions established herein.

## Context

- The project requires a consistent, fast, and maintainable testing strategy for React components and TypeScript utilities across 18+ test files
- Modern frontend applications need testing frameworks that support ESM, TypeScript, and React hooks out of the box without complex configuration
- Test files are distributed across features (users, tasks, auth) and shared components, requiring a unified testing approach that scales across the codebase
- The pattern shows consistent use of .test.tsx and .test.ts extensions with co-located tests near source files, indicating a preference for proximity and discoverability
- CI/CD pipelines require fast, reliable test execution with clear failure reporting to maintain development velocity

## Problem Statement

Without a standardized testing framework and conventions, frontend test quality becomes inconsistent, test execution slows down, and developers face friction when writing or maintaining tests. The codebase needs a unified approach to component testing, hook testing, and utility testing that integrates seamlessly with TypeScript and modern React patterns.

## Decision

1. MAY: Tests MAY use test.each or describe.each for parameterized testing when testing multiple similar scenarios

## Policy Block

- MAY Tests MAY use test.each or describe.each for parameterized testing when testing multiple similar scenarios

In scope:
- All React component files (.tsx) in src/components and src/features
- All TypeScript utility functions and hooks in src/lib and src/hooks
- Form components, dialog components, and authentication flows
- Client-side business logic and data transformation utilities

Out of scope:
- End-to-end tests (use dedicated E2E framework like Playwright or Cypress)
- Backend API tests (use appropriate backend testing framework)
- Visual regression tests (use dedicated visual testing tools)
- Performance and load testing

Exceptions:
- EXC-001: Legacy test files using Jest may remain temporarily during migration period
- EXC-002: Third-party component wrappers that require specialized testing setup

## Rationale

- Pattern detected across 18 files with 91.45% confidence indicates strong, consistent adoption of Vitest and React Testing Library throughout the codebase
- Vitest provides superior performance over Jest with native ESM support, faster execution, and better TypeScript integration without additional configuration
- React Testing Library encourages testing user behavior rather than implementation details, leading to more maintainable and resilient tests
- Co-located test files improve discoverability and make it easier to keep tests synchronized with source code changes

## Consequences

Positive:
- Consistent testing patterns across the entire frontend codebase reduce cognitive load and onboarding time for developers
- Fast test execution with Vitest enables rapid feedback loops and improves developer productivity
- React Testing Library's focus on user behavior results in tests that are resilient to refactoring and implementation changes
- Co-located tests make it easy to find and update tests when modifying components, reducing the likelihood of stale tests

Negative:
- Teams familiar with Jest may face a learning curve when adopting Vitest-specific APIs and configuration
- Co-located tests increase the number of files in feature directories, potentially making navigation more complex in large features
- React Testing Library's opinionated approach may require workarounds for testing certain implementation-specific scenarios
- Migration effort required for any existing tests using different frameworks or patterns

## Alternatives

- Continue using Jest with React Testing Library (rejected)
  Rejected because: Jest has slower startup times, requires additional configuration for ESM and TypeScript, and lacks the modern DX improvements that Vitest provides. The pattern evidence shows clear adoption of Vitest.
  When valid: For projects with extensive existing Jest infrastructure where migration cost outweighs benefits
- Use Enzyme for component testing (rejected)
  Rejected because: Enzyme encourages testing implementation details, is no longer actively maintained, and has poor support for modern React features like hooks and concurrent rendering
  When valid: Never recommended for new projects; only for legacy codebases with no migration path
- Separate test directories (e.g., __tests__ or test/ folder) (rejected)
  Rejected because: Pattern evidence shows consistent co-location. Separate directories reduce discoverability and make it harder to maintain test-source synchronization
  When valid: For projects with very large test suites that need separate organization or when tests require extensive fixtures

## Risks

- Vitest is relatively newer than Jest and may have undiscovered edge cases or breaking changes in future versions
  Mitigation: Pin Vitest version in package.json, monitor release notes, and maintain comprehensive test coverage to catch regressions early
  Owner: Engineering team / DevOps
- Developers may write tests that pass but don't actually validate user-facing behavior, creating false confidence
  Mitigation: Establish code review guidelines emphasizing behavior testing, provide training on React Testing Library best practices, and use linting rules to discourage implementation testing
  Owner: Engineering team / Tech leads
- Test execution time may grow as the codebase scales, impacting CI/CD pipeline performance
  Mitigation: Implement test parallelization, use Vitest's watch mode for local development, and consider test sharding in CI for large test suites
  Owner: DevOps / Engineering team

## Implementation Notes

- Configure vitest.config.ts with appropriate test environment (jsdom for component tests), coverage thresholds, and test file patterns
- Create shared test utilities and custom render functions that wrap React Testing Library's render with common providers (Router, Theme, etc.)
- Establish naming conventions for test suites: describe blocks should match component/function names, test blocks should describe user actions or expected behavior
- Set up CI pipeline to run 'vitest run' with coverage reporting and fail builds on test failures or coverage drops below threshold

## Continuation Context


Verify commands:
- find src -name '*.test.tsx' -o -name '*.test.ts' | wc -l
- grep -r "import.*vitest" src/**/*.test.{ts,tsx} | wc -l
- grep -r "@testing-library/react" src/**/*.test.tsx | wc -l
- vitest run --reporter=verbose

Accept when:
- All test files use .test.tsx or .test.ts naming convention and are co-located with source files
- Vitest is configured as the test runner and all tests execute successfully via 'vitest run' command
- Component tests import and use React Testing Library for rendering and assertions
- CI pipeline successfully runs all tests and reports coverage metrics

## Enforcement

- Verified by: Automated CI/CD pipeline runs vitest on every pull request and blocks merge on test failures
- Verified by: Code review checklist includes verification of test file naming conventions and co-location
- Verified by: ESLint rules enforce React Testing Library best practices (e.g., no-wait-for-empty-callback, prefer-screen-queries)
- Violation handling: Pull requests with failing tests are automatically blocked from merging
- Violation handling: Tests not following naming conventions are flagged during code review and must be corrected
- Violation handling: Tests using deprecated patterns (e.g., Enzyme, implementation testing) require refactoring before approval
- Exception process: Developer documents exception rationale in pull request description with specific justification
- Exception process: Tech lead or architect reviews exception request and approves/rejects based on technical merit
- Exception process: Approved exceptions are documented in test file header comments with ADR reference and expiration date if temporary