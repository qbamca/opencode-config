---
name: git-rules
description: Pull request titles and descriptions. Use when creating, editing, or proposing a pull request.
---

# Git Rules

## Pull Requests

Write the PR title as the intended conventional commit subject after the PR is merged, because the title becomes the merge commit message.

- Use the repository's conventional commit style, for example `fix: smooth KIM image rendering #DBS-4144`.
- Keep the title concise and imperative.
- Include the task ID in the title when one is available and follow the repository's existing placement convention.

Write a short, human-like PR description. Treat its content as the additional lines of the resulting commit message, so each line must remain meaningful in the commit history without PR-only context.

Do not include verification or testing sections.

Avoid deeply technical implementation details.

When setting a PR body via CLI, use actual line breaks and verify the result; never submit literal `\n` sequences.
