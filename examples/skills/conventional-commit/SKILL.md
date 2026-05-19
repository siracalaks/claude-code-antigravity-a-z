---
name: conventional-commit
description: Use this skill any time the user asks for a git commit. Writes messages in Conventional Commit format.
---

# Conventional Commit

When you create a git commit, follow these rules:

1. Start the subject line with one of: feat, fix, chore, docs, refactor, test, perf
2. Add a colon and a space, then a short imperative summary, no period
3. Maximum 72 characters
4. If the change is breaking, add ! before the colon

## Examples

Input: Added user authentication with JWT tokens
Output: feat(auth): implement JWT-based authentication

Input: Fixed null pointer in date parser
Output: fix(parser): handle null input in formatDate

## DON'T

- Never use vague messages like "WIP" or "tmp"
- Don't say "this commit" in the body; it's obviously a commit
- Don't capitalize the type
