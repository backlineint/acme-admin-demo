# Adopt React Context API for Cross-Cutting Provider Patterns: Context Consumers Validate

These rules are ALWAYS ACTIVE for all React components and providers that consume or expose cross-cutting concerns such as theming, fonts, data filtering, and global application configuration.

### Rules

- **R-CONTEXT-001** MUST: Context consumers MUST validate that they are used within the appropriate provider and throw descriptive errors if the context is undefined.

### Verify

```bash
# Count context definitions in the context directory
grep -r 'createContext' src/context/ | wc -l

# Count exported custom hooks that validate context
grep -r 'export.*use[A-Z]' src/context/ | grep -v 'React' | wc -l

# Count provider component files
find src/context -name '*-provider.tsx' -o -name '*Provider.tsx' | wc -l
```

**Accept when:**
- All cross-cutting concerns identified in policy scope (theme management, font loading, data filtering, global configuration, user preferences) are implemented using Context API with dedicated providers
- Each provider exports a custom hook that validates context availability and throws descriptive errors when used outside the provider boundary
- Provider components follow consistent naming conventions (e.g., `[Feature]Provider` component with `use[Feature]` hook) and are organized in the `src/context/` directory
- Error messages from context validation hooks include clear setup instructions and reference the required provider

<enforcement>
Claude Code MUST NOT skip or defer verification of context consumer validation. All context-consuming hooks MUST include runtime checks that throw descriptive errors when context is undefined, and these checks MUST be verified before accepting any code that introduces new context consumers.
</enforcement>