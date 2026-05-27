# Adopt Collocated Unit Tests with .test Extension for Frontend Components: Test Files Not

Status: proposed
Date: 2024-01-20
Deciders: Detection Pipeline (automated)

## Activation

This ADR is ALWAYS ACTIVE for all frontend component and module development. All new components, hooks, utilities, and stores MUST include collocated unit tests following this pattern.

## Context

- The codebase contains 18 files following a consistent pattern of collocating unit tests with source files using the .test.tsx and .test.ts extensions
- Test files are distributed across multiple feature domains including auth, users, tasks, and shared components, indicating a project-wide testing strategy
- The pattern covers diverse code types: React components (.test.tsx), TypeScript utilities (.test.ts), hooks, stores, and test utilities
- This pattern emerged organically across the codebase with 91.45% confidence, suggesting strong team consensus and established practice
- The testing approach supports CI/CD pipelines by enabling automated test discovery and execution through standard tooling conventions

## Problem Statement

Frontend applications require a systematic approach to unit testing that ensures test discoverability, maintainability, and integration with CI/CD pipelines. Without a standardized naming and collocation strategy, tests become difficult to locate, maintain, and execute consistently across the development lifecycle.

## Decision

1. MUST_NOT: Test files MUST NOT use alternative naming conventions such as .spec.ts, _test.ts, or Tests/ subdirectories

## Policy Block

- MUST_NOT Test files MUST NOT use alternative naming conventions such as .spec.ts, _test.ts, or Tests/ subdirectories

In scope:
- React components in src/components/ and src/features/*/components/
- Custom React hooks in src/hooks/
- State management stores in src/stores/
- Utility functions and libraries in src/lib/
- Feature-specific modules under src/features/
- Test utilities and helpers in src/test-utils/

Out of scope:
- End-to-end tests (e2e)
- Integration tests spanning multiple services
- Configuration files and build scripts
- Static assets and style files
- Third-party dependencies in node_modules/
- Generated code or build artifacts

Exceptions:
- EX-001: Legacy code undergoing gradual migration may temporarily lack collocated tests
- EX-002: Experimental or prototype code in designated experimental/ directories

## Rationale

- The pattern demonstrates 91.45% confidence across 18 files, indicating strong organic adoption and team consensus on this testing approach
- Collocated tests improve developer experience by making tests immediately discoverable next to the code they verify, reducing cognitive overhead
- The .test.tsx/.test.ts naming convention aligns with industry-standard tooling (Jest, Vitest, Testing Library) enabling automatic test discovery without configuration
- Consistent test placement across components, hooks, stores, and utilities creates a predictable project structure that scales with codebase growth

## Consequences

Positive:
- Improved test discoverability: developers can immediately locate tests for any module without searching
- Simplified CI/CD integration: test runners automatically discover all .test.ts(x) files without manual configuration
- Enhanced maintainability: when refactoring or moving code, tests move with their source files maintaining cohesion
- Reduced onboarding time: new developers can quickly understand testing patterns through consistent conventions
- Better feature isolation: tests remain within feature boundaries, supporting modular architecture

Negative:
- Directory clutter: test files intermixed with source files may reduce visual clarity in file explorers
- Potential for large directories: features with many components will have twice as many files in the same directory
- Build tool configuration: requires proper exclusion patterns to prevent test files from production bundles
- Migration cost: existing codebases with different conventions require refactoring effort to adopt this pattern

## Alternatives

- Separate __tests__/ directories for each feature or component folder (rejected)
  Rejected because: Creates additional directory nesting and separates tests from their source files, reducing discoverability and increasing navigation overhead
  When valid: May be appropriate for projects with very large test suites requiring separate organization or when tests need to be excluded from certain build processes
- Use .spec.ts(x) extension following Angular/Jasmine conventions (rejected)
  Rejected because: The codebase has already established .test.ts(x) as the standard with 18 files following this pattern; changing would require unnecessary refactoring
  When valid: Appropriate for projects using Angular framework or teams with strong Jasmine/Karma background
- Centralized tests/ directory mirroring src/ structure (rejected)
  Rejected because: Breaks feature cohesion and requires maintaining parallel directory structures, increasing maintenance burden when refactoring
  When valid: May be suitable for projects with strict separation of concerns or when tests need independent versioning

## Risks

- Test files accidentally included in production bundles, increasing bundle size
  Mitigation: Configure build tools (Vite, Webpack, etc.) to explicitly exclude .test.ts(x) files from production builds; verify with bundle analysis
  Owner: Build/DevOps team
- Inconsistent test coverage across features as pattern adoption may be uneven
  Mitigation: Implement coverage thresholds in CI/CD pipeline; use pre-commit hooks to enforce test file creation for new components
  Owner: Engineering team leads
- Large directories become difficult to navigate with both source and test files intermixed
  Mitigation: Use IDE file filtering and grouping features; consider splitting large feature modules into smaller, focused submodules
  Owner: Development team

## Implementation Notes

- Configure test runner (Jest/Vitest) to automatically discover files matching **/*.test.ts(x) pattern
- Update build configuration to exclude .test.ts(x) files from production bundles using appropriate ignore patterns
- Establish test file templates or snippets in IDE to streamline creation of new test files with proper naming
- Document the pattern in project README and contributing guidelines with examples from each category (components, hooks, stores, utilities)
- Set up pre-commit hooks or CI checks to verify that new source files have corresponding test files
- Configure code coverage tools to report on test coverage by feature domain to ensure consistent adoption

## Continuation Context


Verify commands:
- find src -type f \( -name '*.tsx' -o -name '*.ts' \) ! -name '*.test.*' ! -path '*/test-utils/*' -exec sh -c 'test -f "${1%.tsx}.test.tsx" -o -f "${1%.ts}.test.ts" || echo "Missing test: $1"' _ {} \;
- grep -r "modulePathIgnorePatterns\|testMatch" jest.config.* package.json || echo 'Verify test configuration'
- npm test -- --listTests | grep -E '\.test\.(ts|tsx)$' | wc -l

Accept when:
- All React components (.tsx) and TypeScript modules (.ts) in src/ have corresponding .test.tsx or .test.ts files in the same directory
- Test runner configuration successfully discovers and executes all .test.ts(x) files without manual path specification
- Production build artifacts contain zero .test.ts(x) files as verified by bundle analysis
- Code coverage reports show consistent test coverage across all feature domains (components, hooks, stores, utilities)

## Enforcement

- Verified by: CI/CD pipeline runs automated test discovery and execution on every commit
- Verified by: Pre-commit hooks check for presence of test files alongside new source files
- Verified by: Code review checklist includes verification of collocated test files
- Verified by: Coverage reports generated in CI show per-file and per-feature coverage metrics
- Violation handling: CI build fails if new source files are committed without corresponding test files
- Violation handling: Pull requests blocked until test coverage meets minimum thresholds
- Violation handling: Automated comments on PRs identify missing test files
- Violation handling: Weekly reports to team leads highlighting modules with missing or inadequate tests
- Exception process: Developer creates GitHub issue documenting why test cannot be collocated or is temporarily deferred
- Exception process: Tech lead reviews and approves exception with documented justification
- Exception process: Exception tracked in technical debt backlog with priority and target resolution date
- Exception process: Exceptions reviewed quarterly to ensure they remain valid or are resolved