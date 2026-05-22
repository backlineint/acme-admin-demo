# Adopt React Context API for Cross-Cutting Provider Patterns: Cross Cutting Concerns

These rules are ALWAYS ACTIVE for all files implementing cross-cutting concerns (theme management, fonts, global configuration, data filtering) in the React component tree.

### Rules

- **R-CCC-001** MUST: Cross-cutting concerns (theme, fonts, global configuration) MUST be implemented using React Context API with dedicated provider components.
- **R-CCC-002** MUST: Each provider MUST export a custom hook that validates context availability and throws descriptive errors when used outside provider boundaries.
- **R-CCC-003** MUST: Provider components MUST follow consistent naming conventions: `[Feature]Provider` component with corresponding `use[Feature]` hook export.
- **R-CCC-004** MUST: All provider implementations MUST be organized in the `src/context/` directory.
- **R-CCC-005** SHOULD: Complex providers (like faceted filters) SHOULD encapsulate business logic within the provider and expose only the minimal necessary API through the hook.
- **R-CCC-006** SHOULD: Provider dependencies and required setup SHOULD be documented in component README or Storybook documentation.
- **R-CCC-007** MAY: Use React.memo for expensive consumers, split contexts by update frequency, or implement context selectors to mitigate performance concerns.

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
- All cross-cutting concerns identified in policy scope (theme, fonts, filtering, global configuration, user preferences) are implemented using Context API with dedicated providers
- Each provider exports a custom hook that validates context availability and throws descriptive errors
- Provider components follow consistent naming conventions (`[Feature]Provider` and `use[Feature]` hook)
- All provider implementations are located in `src/context/` directory
- Provider dependencies and setup requirements are documented

<enforcement>
Claude Code MUST NOT skip or defer verification of these rules. All R-CCC rules must be checked during code review and architecture validation.
</enforcement>