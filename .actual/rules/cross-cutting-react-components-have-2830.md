# Adopt Collocated Unit Tests with .test Extension for Frontend Components: React Components Have

These rules are ALWAYS ACTIVE for all frontend component and module development. All new components, hooks, utilities, and stores MUST include collocated unit tests following this pattern.

### Rules

- **R-COLLOCATED-001** MUST: All React components MUST have a corresponding unit test file with the .test.tsx extension collocated in the same directory.

### Verify

```bash
# Check for React components without corresponding test files
find src -type f \( -name '*.tsx' -o -name '*.ts' \) ! -name '*.test.*' ! -path '*/test-utils/*' -exec sh -c 'test -f "${1%.tsx}.test.tsx" -o -f "${1%.ts}.test.ts" || echo "Missing test: $1"' _ {} \;

# Verify test runner configuration includes test discovery patterns
grep -r "modulePathIgnorePatterns\|testMatch" jest.config.* package.json || echo 'Verify test configuration'

# Count discovered test files
npm test -- --listTests | grep -E '\.test\.(ts|tsx)$' | wc -l
```

**Accept when:**
- All React components (.tsx) and TypeScript modules (.ts) in src/ have corresponding .test.tsx or .test.ts files in the same directory
- Test runner configuration successfully discovers and executes all .test.ts(x) files without manual path specification
- Production build artifacts contain zero .test.ts(x) files as verified by bundle analysis
- Code coverage reports show consistent test coverage across all feature domains (components, hooks, stores, utilities)

<enforcement>
Claude Code MUST NOT skip or defer verification of collocated test file presence and test runner configuration. CI/CD pipeline MUST fail if new source files are committed without corresponding test files.
</enforcement>