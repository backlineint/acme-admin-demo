# Adopt React Testing Library with Vitest for Component and Unit Testing: Tests Pass Pipeline

These rules are ALWAYS ACTIVE for all React components, TypeScript utilities, custom hooks, state management stores, and form/dialog components in the codebase.

### Rules

- **R-TEST-001** MUST: All tests MUST pass in the CI/CD pipeline before code can be merged to main branches.
- **R-TEST-002** MUST: All React component files (.tsx) in src/components and src/features/**/components directories MUST have corresponding .test.tsx files with at least one test case.
- **R-TEST-003** MUST: All utility and hook files (.ts) in src/lib and src/hooks directories MUST have corresponding .test.ts files with at least one test case.
- **R-TEST-004** MUST: All state management stores in src/stores MUST have corresponding .test.ts files with at least one test case.
- **R-TEST-005** MUST: Test files MUST be co-located with source files using .test.tsx (components) and .test.ts (utilities/hooks) naming conventions.
- **R-TEST-006** MUST: Tests MUST use React Testing Library for component testing, focusing on user behavior rather than implementation details.
- **R-TEST-007** MUST: Tests MUST use Vitest as the test runner with jsdom environment configured.
- **R-TEST-008** SHOULD: Shared test utilities and custom render functions (e.g., renderWithProviders) SHOULD be created to handle common setup like Redux stores, React Router, and theme providers.
- **R-TEST-009** MAY: Prototype or spike code explicitly marked as experimental MAY be exempted from test requirements (EXC-001).
- **R-TEST-010** MAY: Pure presentational components with no logic (props pass-through only) MAY be exempted from test requirements (EXC-002).

### Verify

```bash
# Check for missing test files
find src -type f \( -name '*.tsx' -o -name '*.ts' \) ! -name '*.test.*' ! -name '*.spec.*' -exec sh -c 'test -f "${1%.tsx}.test.tsx" -o -f "${1%.ts}.test.ts" || echo "Missing test: $1"' _ {} \;

# Verify testing dependencies are installed
grep -q "@testing-library/react" package.json && grep -q "vitest" package.json && echo "Testing dependencies found"

# Run test suite with coverage
vitest run --coverage --reporter=json --reporter=text

# Count test files
grep -l "describe\|test\|it" src/**/*.test.{ts,tsx} 2>/dev/null | wc -l
```

**Accept when:**
- All component files (.tsx) have corresponding .test.tsx files with at least one test case
- All utility and hook files (.ts) have corresponding .test.ts files with at least one test case
- Test suite executes successfully with `vitest run` command and all tests pass
- Package.json contains both @testing-library/react and vitest as dependencies
- CI/CD pipeline includes test execution step that blocks merges on test failures
- Code coverage reports are generated and reviewed during pull request process

<enforcement>
Claude Code MUST NOT skip or defer verification. All tests MUST pass in the CI/CD pipeline before code can be merged. Pull requests without corresponding tests are blocked from merging. CI/CD pipeline fails if test suite does not pass or coverage drops below threshold.
</enforcement>