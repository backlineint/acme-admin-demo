# Adopt Comprehensive Test Coverage for Frontend Components and Utilities: State Management Stores

These rules are ALWAYS ACTIVE for all state management stores in src/stores and related test files in the project.

### Rules

- **R-STORES-001** MUST: State management stores in src/stores MUST have corresponding test files validating state transitions and side effects.

### Verify

```bash
# Find all store files without corresponding test files
find src/stores -type f \( -name '*.tsx' -o -name '*.ts' \) ! -name '*.test.*' ! -name '*.d.ts' -exec sh -c 'test -f "${1%.tsx}.test.tsx" -o -f "${1%.ts}.test.ts" || echo "Missing test: $1"' _ {} \;

# Run full test suite with coverage
npm test -- --coverage --passWithNoTests

# Count test cases in store test files
grep -r "describe\|it\|test" src/stores/**/*.test.{ts,tsx} | wc -l
```

**Accept when:**
- All stores in src/stores have corresponding .test.ts or .test.tsx files co-located in the same directory
- Test files validate state transitions, side effects, and critical paths through store logic
- Test suite executes successfully in CI pipeline with no failing tests
- Code coverage meets minimum thresholds for store files
- All store tests follow consistent naming convention (*.test.ts or *.test.tsx)

<enforcement>
Claude Code MUST NOT skip or defer verification of store test coverage. All state management stores require corresponding test files before code review approval.
</enforcement>