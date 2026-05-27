# Adopt React Context API for Cross-Cutting Provider Patterns: Provider Components Placed

These rules are ALWAYS ACTIVE for all files implementing cross-cutting concerns like theming, fonts, data filtering, and global application configuration that need to be shared across multiple components without prop drilling.

### Rules

- **R-CONTEXT-001** SHOULD: Provider components SHOULD be placed in a dedicated context directory (e.g., `src/context/`) for discoverability.
- **R-CONTEXT-002** SHOULD: Each provider SHOULD export a custom hook that validates context availability and throws descriptive errors when used outside provider boundaries.
- **R-CONTEXT-003** SHOULD: Provider components SHOULD follow consistent naming conventions: `[Feature]Provider` component with corresponding `use[Feature]` hook export.
- **R-CONTEXT-004** MUST: Context API MUST be used for cross-cutting concerns in scope (theme management, font loading, data table filtering, global configuration, user preferences spanning component boundaries).
- **R-CONTEXT-005** MUST NOT: Context API MUST NOT be used for component-local state, server state, single-form state, temporary UI state, or state efficiently passed via props through 1-2 levels.
- **R-CONTEXT-006** SHOULD: Complex providers (like faceted filters) SHOULD encapsulate business logic within the provider and expose only minimal necessary API through the hook.
- **R-CONTEXT-007** SHOULD: Provider dependencies and required setup SHOULD be documented in component README or Storybook documentation.

### Verify

```bash
# Count createContext usage in context directory
grep -r 'createContext' src/context/ | wc -l

# Count custom hook exports from context directory
grep -r 'export.*use[A-Z]' src/context/ | grep -v 'React' | wc -l

# Count provider component files
find src/context -name '*-provider.tsx' -o -name '*Provider.tsx' | wc -l
```

**Accept when:**
- All cross-cutting concerns identified in policy scope are implemented using Context API with dedicated providers in `src/context/`
- Each provider exports a custom hook that validates context availability and throws descriptive errors
- Provider components follow consistent naming conventions (`[Feature]Provider` + `use[Feature]` hook)
- Provider dependencies and setup requirements are documented
- No prop drilling is used for in-scope cross-cutting concerns

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules MUST be checked during code review and architectural validation. Violations MUST be addressed before merge unless an approved exception (EXC-001 or EXC-002) is documented.
</enforcement>