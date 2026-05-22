# Adopt React Context API for Cross-Cutting Provider Patterns: Complex Filtering Data

These rules are ALWAYS ACTIVE for all files implementing cross-cutting concerns like theming, fonts, and data filtering across multiple components in the application.

### Rules

- **R-CONTEXT-001** SHOULD: Complex filtering or data transformation logic SHOULD be encapsulated within provider components rather than scattered across consumers.

### Verify

```bash
# Count context definitions in the context directory
grep -r 'createContext' src/context/ | wc -l

# Count exported custom hooks following the use[Feature] pattern
grep -r 'export.*use[A-Z]' src/context/ | grep -v 'React' | wc -l

# Count provider component files
find src/context -name '*-provider.tsx' -o -name '*Provider.tsx' | wc -l
```

**Accept when:**
- All cross-cutting concerns identified in policy scope (theme management, font loading, data table filtering, global configuration, user preferences) are implemented using Context API with dedicated providers
- Each provider exports a custom hook that validates context availability and throws descriptive errors
- Provider components follow consistent naming conventions and are organized in the src/context/ directory
- Provider implementations follow the pattern: createContext → [Feature]Provider component → use[Feature] hook export
- Complex providers encapsulate business logic and expose only minimal necessary API through the hook

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code review and must be checked before accepting changes to cross-cutting concern implementations.
</enforcement>