# Adopt Collocated Unit Tests with .test Extension for Frontend Components: Test Utilities Helpers

These rules are ALWAYS ACTIVE for all frontend component and module development, including React components, custom hooks, state management stores, utility functions, and test utilities across the project.

### Rules

- **R-TEST-001** MUST: Test utilities and helpers MUST be placed in dedicated test-utils directories with appropriate .ts extensions.
- **R-TEST-002** MUST: All React components (.tsx) and TypeScript modules (.ts) in src/ MUST have corresponding .test.tsx or .test.ts files in the same directory.
- **R-TEST-003** MUST: Test runner configuration MUST successfully discover and execute all .test.ts(x) files without manual path specification.
- **R-TEST-004** MUST: Production build artifacts MUST contain zero .test.ts(x) files as verified by bundle analysis.
- **R-TEST-005** MUST: Code coverage reports MUST show consistent test coverage across all feature domains (components, hooks, stores, utilities).
- **R-TEST-006** MUST: Configure build tools (Vite, Webpack, etc.) to explicitly exclude .test.ts(x) files from production builds.
- **R-TEST-007** SHOULD: Establish test file templates or snippets in IDE to streamline creation of new test files with proper naming.
- **R-TEST-008** SHOULD: Set up pre-commit hooks or CI checks to verify that new source files have corresponding test files.
- **R-TEST-009** SHOULD: Document the pattern in project README and contributing guidelines with examples from each category (components, hooks, stores, utilities).

### Verify

```bash
# Check for missing test files alongside source files
find src -type f \( -name '*.tsx' -o -name '*.ts' \) ! -name '*.test.*' ! -path '*/test-utils/*' -exec sh -c 'test -f "${1%.tsx}.test.tsx" -o -f "${1%.ts}.test.ts" || echo "Missing test: $1"' _ {} \;

# Verify test runner configuration
grep -r "modulePathIgnorePatterns\|testMatch" jest.config.* package.json || echo 'Verify test configuration'

# Count discovered test files
npm test -- --listTests | grep -E '\.test\.(ts|tsx)$' | wc -l

# Verify .test files are excluded from production bundle
npm run build && grep -r '\.test\.(ts|tsx)' dist/ && echo 'ERROR: Test files found in production build' || echo 'PASS: No test files in production build'
```

**Accept when:**
- All React components (.tsx) and TypeScript modules (.ts) in src/ have corresponding .test.tsx or .test.ts files in the same directory
- Test runner configuration successfully discovers and executes all .test.ts(x) files without manual path specification
- Production build artifacts contain zero .test.ts(x) files as verified by bundle analysis
- Code coverage reports show consistent test coverage across all feature domains (components, hooks, stores, utilities)
- Pre-commit hooks or CI checks prevent new source files from being committed without corresponding test files

<enforcement>
Claude Code MUST NOT skip or defer verification. CI/CD pipeline MUST run automated test discovery and execution on every commit. Pull requests MUST be blocked until test coverage meets minimum thresholds. Violations result in CI build failure and automated PR comments identifying missing test files.
</enforcement>