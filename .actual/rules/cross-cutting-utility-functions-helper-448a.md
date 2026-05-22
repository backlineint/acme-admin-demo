# Adopt Comprehensive Test Coverage for Frontend Components and Utilities: Utility Functions Helper

These rules are ALWAYS ACTIVE for all utility functions, helper modules, React components, state management stores, and custom test utilities in the src/ directory.

### Rules

- **R-TEST-001** MUST: All utility functions and helper modules in src/lib and src/hooks directories MUST have corresponding test files with the .test.ts extension.
- **R-TEST-002** MUST: All React components (.tsx files) in src/components and src/features MUST have corresponding test files with the .test.tsx extension.
- **R-TEST-003** MUST: All state management stores in src/stores MUST have corresponding test files with the .test.ts extension.
- **R-TEST-004** MUST: Test files MUST be co-located with source files in the same directory.
- **R-TEST-005** SHOULD: Tests MUST cover critical paths: user interactions for components, edge cases for utilities, state transitions for stores, and error handling across all layers.
- **R-TEST-006** MAY: Simple presentational components with no logic (pure props pass-through) may be exempted from test requirements (EXC-001).
- **R-TEST-007** MAY: Prototype or experimental features marked as temporary may be exempted from test requirements (EXC-002).

### Verify

```bash
# Find all source files without corresponding test files
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
- All state management stores in src/stores have corresponding .test.ts files
- Custom test utilities in src/test-utils are properly documented and used consistently across test files

<enforcement>
Claude Code MUST NOT skip or defer verification. All test files must be verified to exist and pass before code is merged. Coverage reports must be reviewed and thresholds must be met. Exceptions require explicit documentation and tech lead approval.
</enforcement>