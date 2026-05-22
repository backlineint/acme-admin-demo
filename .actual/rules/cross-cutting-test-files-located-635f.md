# Adopt Comprehensive Test Coverage for Frontend Components and Utilities: Test Files Located

These rules are ALWAYS ACTIVE for all React components, utility functions, hooks, and state management stores in the src/ directory.

### Rules

- **R-TEST-001** MUST: Test files MUST be co-located with their source files in the same directory.

### Verify

```bash
# Find source files without corresponding test files
find src -type f \( -name '*.tsx' -o -name '*.ts' \) ! -name '*.test.*' ! -name '*.d.ts' -exec sh -c 'test -f "${1%.tsx}.test.tsx" -o -f "${1%.ts}.test.ts" || echo "Missing test: $1"' _ {} \;

# Run test suite with coverage
npm test -- --coverage --passWithNoTests

# Count test cases across the codebase
grep -r "describe\|it\|test" src/**/*.test.{ts,tsx} | wc -l
```

**Accept when:**
- All components in src/components and src/features have corresponding .test.tsx files co-located in the same directory
- All utilities in src/lib and src/hooks have corresponding .test.ts files with test coverage for primary functionality
- Test suite executes successfully in CI pipeline with no failing tests and meets minimum coverage thresholds

<enforcement>
Claude Code MUST NOT skip or defer verification. All test files must be co-located with source files before code is considered complete.
</enforcement>