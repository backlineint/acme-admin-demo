# Adopt Comprehensive Test Coverage for Frontend Components and Utilities: Shared Test Utilities

These rules are ALWAYS ACTIVE for all React components in src/components and src/features, utility functions in src/lib and src/hooks, state management stores in src/stores, and custom test utilities in src/test-utils.

### Rules

- **R-TEST-001** SHOULD: Shared test utilities and helpers SHOULD be centralized in src/test-utils for reuse across test suites.
- **R-TEST-002** MUST: Use consistent test file naming: *.test.tsx for React components and *.test.ts for TypeScript utilities and functions.
- **R-TEST-003** MUST: Co-locate test files with source files in the same directory to improve discoverability and maintainability.
- **R-TEST-004** SHOULD: Create shared test utilities in src/test-utils for common testing patterns (e.g., table testing helpers, mock data factories, custom render functions).
- **R-TEST-005** SHOULD: Ensure tests cover critical paths: user interactions for components, edge cases for utilities, state transitions for stores, and error handling across all layers.
- **R-TEST-006** MUST: Configure test runner to automatically discover and execute all *.test.ts and *.test.tsx files in the src directory.

### Verify

```bash
# Check for missing test files alongside source files
find src -type f \( -name '*.tsx' -o -name '*.ts' \) ! -name '*.test.*' ! -name '*.d.ts' -exec sh -c 'test -f "${1%.tsx}.test.tsx" -o -f "${1%.ts}.test.ts" || echo "Missing test: $1"' _ {} \;

# Run full test suite with coverage
npm test -- --coverage --passWithNoTests

# Count test cases across the codebase
grep -r "describe\|it\|test" src/**/*.test.{ts,tsx} | wc -l
```

**Accept when:**
- All components in src/components and src/features have corresponding .test.tsx files co-located in the same directory
- All utilities in src/lib and src/hooks have corresponding .test.ts files with test coverage for primary functionality
- Test suite executes successfully in CI pipeline with no failing tests and meets minimum coverage thresholds
- Shared test utilities are centralized in src/test-utils and reused across multiple test suites
- Test files follow consistent naming conventions (*.test.tsx, *.test.ts)

<enforcement>
Claude Code MUST NOT skip or defer verification. All test files must be co-located with source files, follow naming conventions, and pass the verification commands before code is considered compliant.
</enforcement>