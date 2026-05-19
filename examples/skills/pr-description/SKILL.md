---
name: pr-description
description: Generate a PR description from git diff. Use when the user asks for a PR description, summary of changes, or "what changed".
---

## Diff
!`git diff main...HEAD`

## Commits
!`git log main..HEAD --oneline`

## Task
From the context above:

1. **Summary** (1-2 sentences): what this PR does
2. **Changes** (bulleted): what changed and why, per file
3. **Testing**: how to test it
4. **Breaking changes**: list any, or write "none"
5. **Screenshots**: if UI changed, remind the user to attach
