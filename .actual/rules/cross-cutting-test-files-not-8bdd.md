# Adopt Collocated Unit Tests with .test Extension for Frontend Components: Test Files Not

These rules are ALWAYS ACTIVE for all frontend component and module development, including React components, custom hooks, state management stores, utility functions, and feature-specific modules.

### Rules

- **R-TEST-001** MUST_NOT: Test files MUST NOT use alternative naming conventions such as .spec.ts, _test.ts, or Tests/ subdirectories

### Verify

```bash
# Check for missing test files alongside source files
find src -type f \( -name '*.tsx' -o -name '*.ts' \) ! -name '*.test.*' ! -path '*/test-utils/*' -exec sh -c 'test -f "${1%.tsx}.test.tsx" -o -f "${1%.ts}.test.ts" || echo "Missing test: $1"' _ {} \;

# Verify test runner configuration includes .test.ts(x) pattern
grep -r "modulePathIgnorePatterns\|testMatch" jest.config.* package.json || echo 'Verify test configuration'

# Count discovered test files
npm test -- --listTests | grep -E '\.test\.(ts|tsx)$' | wc -l
```

**Accept when:**
- All React components (.tsx) and TypeScript modules (.ts) in src/ have corresponding .test.tsx or .test.ts files in the same directory
- Test runner configuration successfully discovers and executes all .test.ts(x) files without manual path specification
- Production build artifacts contain zero .test.ts(x) files as verified by bundle analysis
- Code coverage reports show consistent test coverage across all feature domains (components, hooks, stores, utilities)
- No files matching .spec.ts(x), _test.ts(x), or Tests/ directory patterns exist in the codebase

<enforcement>
Claude Code MUST NOT skip or defer verification of collocated test file presence and naming conventions. All new source files committed to the repository MUST have corresponding .test.ts(x) files in the same directory, and CI/CD pipeline MUST enforce this requirement.
</enforcement>