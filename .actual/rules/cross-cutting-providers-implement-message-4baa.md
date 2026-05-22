# Adopt React Context API for Cross-Cutting Provider Patterns: Providers Implement Message

These rules are ALWAYS ACTIVE for all files in `src/context/` and components that implement cross-cutting provider patterns for theming, fonts, data filtering, and global application configuration.

### Rules

- **R-CONTEXT-001** MUST: Implement cross-cutting concerns (theme management, font loading, data filtering, global configuration, user preferences) using React Context API with dedicated providers.
- **R-CONTEXT-002** MUST: Create a context directory at `src/context/` to house all provider implementations.
- **R-CONTEXT-003** MUST: Follow the pattern: create context with `createContext`, implement `[Feature]Provider` component, export `use[Feature]` hook that validates context availability.
- **R-CONTEXT-004** MUST: Each provider's custom hook MUST validate context availability and throw descriptive errors when used outside provider boundaries.
- **R-CONTEXT-005** MUST: Provider components MUST follow consistent naming conventions (`*-provider.tsx` or `*Provider.tsx`) and be organized in the context directory.
- **R-CONTEXT-006** SHOULD: For complex providers, encapsulate business logic within the provider and expose only the minimal necessary API through the hook.
- **R-CONTEXT-007** SHOULD: Document provider dependencies and required setup in component README or Storybook documentation.
- **R-CONTEXT-008** SHOULD: Use React.memo for expensive consumers, split contexts by update frequency, and implement context selectors or useMemo for derived values to mitigate performance concerns.
- **R-CONTEXT-009** MAY: Providers MAY implement message queue or event-driven patterns for asynchronous state updates when appropriate.
- **R-CONTEXT-010** MUST NOT: Use prop drilling for in-scope cross-cutting concerns; use Context API instead.
- **R-CONTEXT-011** MUST NOT: Create module-level singletons or global variables for configuration that should be reactive.

### Verify

```bash
# Count context files
grep -r 'createContext' src/context/ | wc -l

# Count exported custom hooks
grep -r 'export.*use[A-Z]' src/context/ | grep -v 'React' | wc -l

# Count provider files
find src/context -name '*-provider.tsx' -o -name '*Provider.tsx' | wc -l
```

**Accept when:**
- All cross-cutting concerns identified in policy scope (theme management, font loading, data filtering, global configuration, user preferences) are implemented using Context API with dedicated providers.
- Each provider exports a custom hook that validates context availability and throws descriptive errors.
- Provider components follow consistent naming conventions and are organized in the `src/context/` directory.
- No prop drilling is used for in-scope cross-cutting concerns.
- Provider dependencies and setup are documented.

<enforcement>
Claude Code MUST NOT skip or defer verification of these rules. All providers MUST follow the established pattern, and violations MUST be flagged during code review.
</enforcement>