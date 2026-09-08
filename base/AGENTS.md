# Split OpenCode Configuration

This OpenCode setup is intentionally organized as layered configuration. The original global OpenCode configuration directory is the shared base layer. It defines the common foundation that should be available regardless of which context is active.

Separate work and private profile directories sit above that base layer. Each profile provides context-specific extensions and overrides without changing the shared foundation. This keeps the two contexts independent while allowing both to benefit from the same baseline capabilities.

Treat configuration as a merge with clear precedence: OpenCode starts with the shared base, then applies the selected profile over it. Repository-local configuration may then further specialize the result for the current codebase. When behavior is unclear, inspect the effective merged configuration rather than assuming that a definition from one layer is final.

Place configuration according to its scope:

- Put shared capabilities and defaults in the base layer.
- Put canonical shared agent definitions in the base layer.
- Put only sparse model overrides for shared agents in profiles when models differ.
- Put work-only or private-only policies and integrations in the applicable profile.
- Put repository-specific behavior in repository-local configuration.

Maintain this structure deliberately. Avoid duplicating shared configuration across profiles, and preserve the separation between work and private concerns. Before changing behavior, inspect the effective configuration and identify the layer responsible for it. Update the narrowest appropriate layer so that the change has only the intended scope. Keep this document architectural and safe to share: never copy secrets or profile-specific settings into it.

For canonical agent responsibilities, delegation boundaries, and continuation rules, see [ROLE-DELEGATION.md](ROLE-DELEGATION.md).
