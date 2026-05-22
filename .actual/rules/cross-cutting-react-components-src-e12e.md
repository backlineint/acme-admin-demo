# Adopt Comprehensive Test Coverage for Frontend Components and Utilities: React Components Src

These rules are ALWAYS ACTIVE for all React components, utilities, hooks, and state management code in the src/ directory.

### Rules

- **R-TEST-001** MUST: All React components in the src/components and src/features directories MUST have corresponding test files with the .test.tsx extension.
- **R-TEST-002** MUST: All utility functions and helpers in src/lib and src/hooks MUST have corresponding test files with the .test.ts extension.
- **R-TEST-003** MUST: All state management stores in src/stores MUST have corresponding test files with the .test.ts extension.
- **R-TEST-004** SHOULD: Tests SHOULD be co-located with source files in the same directory to improve discoverability and maintainability.
- **R-TEST-005** SHOULD: Shared test utilities SHOULD be created in src/test-utils for common testing patterns (e.g., table testing helpers, mock data factories, custom render functions).
- **R-TEST-006** SHOULD: Tests SHOULD cover critical paths: user interactions for components, edge cases for utilities, state transitions for stores, and error handling across all layers.
- **R-TEST-007** MAY: Simple presentational components with no logic (pure props pass-through) MAY be exempted from test requirements with documented justification (EXC-001).
- **R-TEST-008** MAY: Prototype or experimental features marked as temporary MAY be exempted from test requirements with documented justification (EXC-002).

### Verify

```bash
# Find all source files without corresponding test files
find src -type f \( -name '*.tsx' -o -name '*.ts' \) ! -name '*.test.*' ! -name '*.d.ts' -exec sh -c 'test -f "${1%.tsx}.test.tsx" -o -f "${1%.ts}.test.ts" || echo "Missing test: $1"' _ {} \;

# Run test suite with coverage reporting
npm test -- --coverage --passWithNoTests

# Count test cases across the codebase
grep -r "describe\|it\|test" src/**/*.test.{ts,tsx} | wc -l
```

**Accept when:**
- All components in src/components and src/features have corresponding .test.tsx files co-located in the same directory
- All utilities in src/lib and src/hooks have corresponding .test.ts files with test coverage for primary functionality
- Test suite executes successfully in CI pipeline with no failing tests and meets minimum coverage thresholds
- All new code additions include corresponding test files or documented exceptions with tech lead approval

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests must pass the verify commands above before merge. Code coverage reports must be reviewed during CI builds. Code review process must verify that new components and utilities include corresponding test files. Violations block merge to main branch.
</enforcement>