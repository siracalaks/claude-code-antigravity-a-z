---
name: clean-imports
description: Remove unused imports and sort the rest. Use any time the user asks to clean imports, sort imports, or tidy imports.
---

For each file the user asks to clean:

1. Remove imports not referenced in file
2. Sort remaining by: standard library, third-party, local
3. Group sections with blank line between

Do not touch side-effect imports (imports without name binding).
