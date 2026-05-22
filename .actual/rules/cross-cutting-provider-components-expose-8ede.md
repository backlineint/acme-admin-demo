# Adopt React Context API for Cross-Cutting Provider Patterns: Provider Components Expose

These rules are ALWAYS ACTIVE for all provider components and context implementations that manage cross-cutting concerns like theming, fonts, data filtering, and global application configuration.

### Rules

- **R-CONTEXT-001** MUST: Provider components MUST expose custom hooks (e.g., useTheme, useFont) as the public API for consuming context values.
- **R-CONTEXT-002** MUST: Provider components MUST validate context availability in custom hooks and throw descriptive errors when used outside provider boundaries.
- **R-CONTEXT-003** MUST: All provider implementations MUST be organized in the `src/context/` directory.
- **R-CONTEXT-004** MUST: Provider components MUST follow consistent naming conventions: `[Feature]Provider` component and `use[Feature]` hook export.
- **R-CONTEXT-005** SHOULD: Complex providers SHOULD encapsulate business logic within the provider and expose only the minimal necessary API through the hook.
- **R-CONTEXT-006** SHOULD: Providers managing state with high update frequency SHOULD be split into separate contexts or use context selectors to minimize re-renders.
- **R-CONTEXT-007** SHOULD: Provider dependencies and required setup SHOULD be documented in component README or Storybook documentation.

### Verify

```bash
# Count context implementations
grep -r 'createContext' src/context/ | wc -l

# Count exported custom hooks
grep -r 'export.*use[A-Z]' src/context/ | grep -v 'React' | wc -l

# Count provider files
find src/context -name '*-provider.tsx' -o -name '*Provider.tsx' | wc -l
```

**Accept when:**
- All cross-cutting concerns identified in policy scope (theme management, font loading, data filtering, global configuration, user preferences) are implemented using Context API with dedicated providers
- Each provider exports a custom hook that validates context availability and throws descriptive errors when used outside provider boundaries
- Provider components follow consistent naming conventions (`[Feature]Provider` and `use[Feature]` hook) and are organized in the `src/context/` directory
- Provider implementations encapsulate business logic and expose only minimal necessary APIs through custom hooks
- TypeScript strict mode is enforced with proper context type definitions and null checks

<enforcement>
Claude Code MUST NOT skip or defer verification of these rules. All provider implementations MUST be reviewed against R-CONTEXT-001 through R-CONTEXT-007 before acceptance. Violations MUST be flagged in code review and refactored to comply with the established pattern.
</enforcement>