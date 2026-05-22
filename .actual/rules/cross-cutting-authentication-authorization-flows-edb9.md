# Adopt Comprehensive Test Coverage for Frontend Components and Utilities: Authentication Authorization Flows

These rules are ALWAYS ACTIVE for all React components (.tsx files) in src/components and src/features, all utility functions and helpers in src/lib and src/hooks, all state management stores in src/stores, and custom test utilities and helpers in src/test-utils.

### Rules

- **R-AUTH-001** SHOULD: Authentication and authorization flows SHOULD have comprehensive test coverage including OTP, sign-up, and session management.
- **R-AUTH-002** MUST: Use consistent test file naming: *.test.tsx for React components and *.test.ts for TypeScript utilities and functions.
- **R-AUTH-003** MUST: Co-locate test files with source files in the same directory to improve discoverability and maintainability.
- **R-AUTH-004** SHOULD: Create shared test utilities in src/test-utils for common testing patterns (e.g., table testing helpers, mock data factories, custom render functions).
- **R-AUTH-005** SHOULD: Ensure tests cover critical paths: user interactions for components, edge cases for utilities, state transitions for stores, and error handling across all layers.
- **R-AUTH-006** MUST: Configure test runner to automatically discover and execute all *.test.ts and *.test.tsx files in the src directory.

### Verify

```bash
# Check for missing test files
find src -type f \( -name '*.tsx' -o -name '*.ts' \) ! -name '*.test.*' ! -name '*.d.ts' -exec sh -c 'test -f "${1%.tsx}.test.tsx" -o -f "${1%.ts}.test.ts" || echo "Missing test: $1"' _ {} \;

# Run test suite with coverage
npm test -- --coverage --passWithNoTests

# Count test cases
grep -r "describe\|it\|test" src/**/*.test.{ts,tsx} | wc -l
```

**Accept when:**
- All components in src/components and src/features have corresponding .test.tsx files co-located in the same directory
- All utilities in src/lib and src/hooks have corresponding .test.ts files with test coverage for primary functionality
- Test suite executes successfully in CI pipeline with no failing tests and meets minimum coverage thresholds
- Authentication and authorization flows (OTP, sign-up, session management) have dedicated test coverage

<enforcement>
Claude Code MUST NOT skip or defer verification. All test files must be present and passing before code review approval. Coverage reports must be generated and reviewed. Violations block merge to main branch.
</enforcement>