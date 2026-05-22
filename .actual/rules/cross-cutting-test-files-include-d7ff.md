# Adopt Comprehensive Test Coverage for Frontend Components and Utilities: Test Files Include

These rules are ALWAYS ACTIVE for all React components, utility functions, hooks, and state management stores in the src/ directory.

### Rules

- **R-TEST-001** MAY: Test files MAY include integration tests that verify component interactions with external dependencies.

### Verify

```bash
# Find source files without corresponding test files
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
- Test files follow consistent naming convention (*.test.tsx for React components, *.test.ts for utilities)
- Shared test utilities exist in src/test-utils for common testing patterns

<enforcement>
Claude Code MUST NOT skip or defer verification. All test files must be verified to exist and pass before accepting changes to components, utilities, hooks, or state management stores.
</enforcement>