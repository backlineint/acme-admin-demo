# Adopt React Context API for Cross-Cutting Provider Patterns: Provider Follow Naming

These rules are ALWAYS ACTIVE for all files implementing cross-cutting concerns like theming, fonts, data filtering, and global application configuration using React Context API.

### Rules

- **R-CONTEXT-001** SHOULD: Provider APIs SHOULD follow naming conventions: [Feature]Provider component and use[Feature] hook

### Verify

```bash
# Count context definitions in the context directory
grep -r 'createContext' src/context/ | wc -l

# Count exported custom hooks following the use[Feature] pattern
grep -r 'export.*use[A-Z]' src/context/ | grep -v 'React' | wc -l

# Count provider component files following naming convention
find src/context -name '*-provider.tsx' -o -name '*Provider.tsx' | wc -l
```

**Accept when:**
- All cross-cutting concerns identified in policy scope (theme management, font loading, data filtering, global configuration, user preferences) are implemented using Context API with dedicated providers
- Each provider exports a custom hook that validates context availability and throws descriptive errors
- Provider components follow consistent naming conventions ([Feature]Provider) and are organized in the src/context/ directory
- Custom hooks follow the use[Feature] naming pattern and are exported from their respective provider modules

<enforcement>
Claude Code MUST NOT skip or defer verification. All three verify commands MUST execute successfully before accepting changes to context-based providers.
</enforcement>