---
name: run-tests
description: Run tests matching a pattern. Use when user says "test", "run tests", or asks to verify changes.
allowed-tools: Bash(npm *), Bash(npx *), Read, Edit
argument-hint: [pattern]
---

Run tests matching: $ARGUMENTS

1. Detect framework (jest, vitest, pytest)
2. Run with given pattern, or all if empty
3. If failures: analyze, propose fix, re-run
4. Report: X passed, Y failed
