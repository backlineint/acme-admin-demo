# Adopt React Testing Library with Vitest for Component and Unit Testing: Test Suites Organized

These rules are ALWAYS ACTIVE for all React components, custom hooks, utility functions, and state management stores in the codebase.

### Rules

- **R-TEST-001** SHOULD: Test suites SHOULD be organized by feature domain (auth, users, tasks) matching the source code structure.
- **R-TEST-002** MUST: All React components in src/components and src/features/**/components MUST have corresponding .test.tsx files with at least one test case.
- **R-TEST-003** MUST: All custom hooks in src/hooks MUST have corresponding .test.ts files with at least one test case.
- **R-TEST-004** MUST: All utility functions in src/lib MUST have corresponding .test.ts files with at least one test case.
- **R-TEST-005** MUST: All state management stores in src/stores MUST have corresponding .test.ts files with at least one test case.
- **R-TEST-006** MUST: Test files MUST use React Testing Library for component testing, focusing on user behavior rather than implementation details.
- **R-TEST-007** MUST: Test files MUST be co-located with source files using .test.tsx (components) and .test.ts (utilities/hooks) naming conventions.
- **R-TEST-008** MUST: All tests MUST execute successfully with the vitest run command with no failures.
- **R-TEST-009** SHOULD: Test suites SHOULD include shared test utilities and custom render functions (e.g., renderWithProviders) for common setup like Redux stores, React Router, and theme providers.

### Verify

```bash
# Check for missing test files for components
find src -type f -name '*.tsx' ! -name '*.test.*' ! -name '*.spec.*' -exec sh -c 'test -f "${1%.tsx}.test.tsx" || echo "Missing test: $1"' _ {} \;

# Check for missing test files for utilities and hooks
find src -type f -name '*.ts' ! -name '*.test.*' ! -name '*.spec.*' -exec sh -c 'test -f "${1%.ts}.test.ts" || echo "Missing test: $1"' _ {} \;

# Verify testing dependencies are installed
grep -q '@testing-library/react' package.json && grep -q 'vitest' package.json && echo 'Testing dependencies found'

# Run test suite and generate coverage report
vitest run --coverage --reporter=json --reporter=text

# Count test files
grep -l 'describe\|test\|it' src/**/*.test.{ts,tsx} 2>/dev/null | wc -l
```

**Accept when:**
- All component files (.tsx) have corresponding .test.tsx files with at least one test case
- All utility and hook files (.ts) have corresponding .test.ts files with at least one test case
- Test suite executes successfully with vitest run command and all tests pass
- Package.json contains both @testing-library/react and vitest as dependencies
- CI/CD pipeline includes test execution step that blocks merges on test failures
- Code coverage reports are generated and reviewed during pull request process

<enforcement>
Claude Code MUST NOT skip or defer verification. All test files MUST exist and pass before code is accepted. Pull requests without corresponding tests MUST be blocked from merging. CI/CD pipeline MUST fail if test suite does not pass or coverage drops below configured thresholds.
</enforcement>