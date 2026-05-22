# Adopt React Testing Library with Vitest for Component and Unit Testing: Tests Cover Critical

These rules are ALWAYS ACTIVE for all React components, custom hooks, utility functions, and state management stores in the codebase.

### Rules

- **R-TEST-001** SHOULD: Tests SHOULD cover critical user interactions, edge cases, and error handling scenarios for each component or module.

### Verify

```bash
# Check for test file existence alongside source files
find src -type f \( -name '*.tsx' -o -name '*.ts' \) ! -name '*.test.*' ! -name '*.spec.*' -exec sh -c 'test -f "${1%.tsx}.test.tsx" -o -f "${1%.ts}.test.ts" || echo "Missing test: $1"' _ {} \;

# Verify testing dependencies are installed
grep -q "@testing-library/react" package.json && grep -q "vitest" package.json && echo "Testing dependencies found"

# Run test suite and generate coverage report
vitest run --coverage --reporter=json --reporter=text

# Count test files in the codebase
grep -l "describe\|test\|it" src/**/*.test.{ts,tsx} | wc -l
```

**Accept when:**
- All component files (.tsx) have corresponding .test.tsx files with at least one test case
- All utility and hook files (.ts) have corresponding .test.ts files with at least one test case
- Test suite executes successfully with `vitest run` command and all tests pass
- Package.json contains both @testing-library/react and vitest as dependencies
- CI/CD pipeline includes test execution step that blocks merges on test failures
- Tests verify critical user interactions, edge cases, and error handling scenarios

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests without corresponding tests MUST be blocked from merging. CI/CD pipeline MUST fail if test suite does not pass or coverage drops below configured thresholds.
</enforcement>