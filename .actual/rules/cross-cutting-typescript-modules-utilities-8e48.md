# Adopt Collocated Unit Tests with .test Extension for Frontend Components: Typescript Modules Utilities

These rules are ALWAYS ACTIVE for all frontend component and module development, including React components, custom hooks, state management stores, utility functions, and feature-specific modules.

### Rules

- **R-COLLOCATE-001** MUST: All TypeScript modules (utilities, hooks, stores) MUST have a corresponding unit test file with the .test.ts extension collocated in the same directory.
- **R-COLLOCATE-002** MUST: All React components (.tsx) MUST have a corresponding unit test file with the .test.tsx extension collocated in the same directory.
- **R-COLLOCATE-003** MUST: Test files MUST follow the naming convention of `{source-filename}.test.{ts|tsx}` to enable automatic discovery by test runners.
- **R-COLLOCATE-004** MUST: Build configuration MUST explicitly exclude .test.ts(x) files from production bundles using appropriate ignore patterns.
- **R-COLLOCATE-005** SHOULD: Use IDE file filtering and grouping features to manage visual clarity when source and test files are intermixed in the same directory.

### Verify

```bash
# Find all TypeScript/TSX files without corresponding test files
find src -type f \( -name '*.tsx' -o -name '*.ts' \) ! -name '*.test.*' ! -path '*/test-utils/*' -exec sh -c 'test -f "${1%.tsx}.test.tsx" -o -f "${1%.ts}.test.ts" || echo "Missing test: $1"' _ {} \;

# Verify test runner configuration includes test discovery patterns
grep -r "modulePathIgnorePatterns\|testMatch" jest.config.* package.json || echo 'Verify test configuration'

# Count discovered test files
npm test -- --listTests | grep -E '\.test\.(ts|tsx)$' | wc -l

# Verify production build excludes test files
npm run build && find dist -name '*.test.*' && echo 'ERROR: Test files found in build' || echo 'OK: No test files in build'
```

**Accept when:**
- All React components (.tsx) and TypeScript modules (.ts) in src/ have corresponding .test.tsx or .test.ts files in the same directory
- Test runner configuration successfully discovers and executes all .test.ts(x) files without manual path specification
- Production build artifacts contain zero .test.ts(x) files as verified by bundle analysis
- Code coverage reports show consistent test coverage across all feature domains (components, hooks, stores, utilities)
- CI/CD pipeline runs automated test discovery and execution on every commit without failures

<enforcement>
Claude Code MUST NOT skip or defer verification of collocated test file presence and test runner configuration. All new source files committed to the repository MUST have corresponding test files following the .test.ts(x) naming convention, verified by CI/CD pipeline checks before merge.
</enforcement>