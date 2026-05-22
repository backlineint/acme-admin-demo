# Adopt Comprehensive Test Coverage for Frontend Components and Utilities: Complex Components Dialogs

These rules are ALWAYS ACTIVE for all React components (.tsx files) in src/components and src/features, all utility functions and helpers in src/lib and src/hooks, all state management stores in src/stores, and custom test utilities and helpers in src/test-utils.

### Rules

- **R-DIALOG-001** SHOULD: Complex UI components (dialogs, forms, drawers) SHOULD test user interactions, validation logic, and error states.
- **R-DIALOG-002** MUST: Use consistent test file naming: *.test.tsx for React components and *.test.ts for TypeScript utilities and functions.
- **R-DIALOG-003** MUST: Co-locate test files with source files in the same directory to improve discoverability and maintainability.
- **R-DIALOG-004** SHOULD: Create shared test utilities in src/test-utils for common testing patterns (e.g., table testing helpers, mock data factories, custom render functions).
- **R-DIALOG-005** SHOULD: Ensure tests cover critical paths: user interactions for components, edge cases for utilities, state transitions for stores, and error handling across all layers.

### Verify

```bash
# Find components without corresponding test files
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
- Tests cover user interactions, validation logic, and error states for complex UI components
- Shared test utilities exist in src/test-utils for common testing patterns

<enforcement>
Claude Code MUST NOT skip or defer verification. All test files must be present and passing before code is merged. Coverage reports must be reviewed and meet minimum thresholds. Code review must verify that new components and utilities include corresponding test files.
</enforcement>