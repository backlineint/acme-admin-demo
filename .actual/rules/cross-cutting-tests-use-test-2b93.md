# Adopt React Testing Library with Vitest for Component and Unit Testing: Tests Use Test

These rules are ALWAYS ACTIVE for all React components, custom hooks, utility functions, and state management stores in the codebase.

### Rules

- **R-TEST-001** MUST: All React components (.tsx files) in `src/components` and `src/features/**/components` directories have corresponding `.test.tsx` files co-located with the source file.
- **R-TEST-002** MUST: All custom hooks (.ts files) in `src/hooks` have corresponding `.test.ts` files co-located with the source file.
- **R-TEST-003** MUST: All utility functions (.ts files) in `src/lib` have corresponding `.test.ts` files co-located with the source file.
- **R-TEST-004** MUST: All state management stores (.ts files) in `src/stores` have corresponding `.test.ts` files co-located with the source file.
- **R-TEST-005** MUST: Test files use React Testing Library for component testing and follow user-centric testing patterns rather than implementation details.
- **R-TEST-006** MUST: Tests are executed using Vitest as the test runner with jsdom environment configured.
- **R-TEST-007** MUST: All tests pass successfully when running `vitest run` command.
- **R-TEST-008** MAY: Tests MAY use test utilities and custom render functions (e.g., `renderWithProviders`) to reduce boilerplate and improve maintainability.
- **R-TEST-009** SHOULD: Test files follow naming conventions: `.test.tsx` for components and `.test.ts` for utilities and hooks.
- **R-TEST-010** SHOULD: Each test file contains at least one test case using `describe` or `test`/`it` blocks.

### Verify

```bash
# Verify all component files have corresponding test files
find src -type f \( -name '*.tsx' -o -name '*.ts' \) ! -name '*.test.*' ! -name '*.spec.*' -exec sh -c 'test -f "${1%.tsx}.test.tsx" -o -f "${1%.ts}.test.ts" || echo "Missing test: $1"' _ {} \;

# Verify React Testing Library and Vitest are installed
grep -q "@testing-library/react" package.json && grep -q "vitest" package.json && echo "Dependencies verified"

# Run test suite and generate coverage report
vitest run --coverage --reporter=json --reporter=text

# Count test files with test cases
grep -l "describe\|test\|it" src/**/*.test.{ts,tsx} | wc -l
```

**Accept when:**
- All component files (.tsx) have corresponding .test.tsx files with at least one test case
- All utility and hook files (.ts) have corresponding .test.ts files with at least one test case
- Test suite executes successfully with `vitest run` command and all tests pass
- Package.json contains both `@testing-library/react` and `vitest` as dependencies
- CI/CD pipeline includes test execution step that blocks merges on test failures
- No missing test files are reported by the verification commands

<enforcement>
Claude Code MUST NOT skip or defer verification. All tests MUST pass before code is considered compliant. Missing test files MUST be identified and remediated. Coverage reports MUST be reviewed to ensure adequate test coverage across components, hooks, utilities, and stores.
</enforcement>