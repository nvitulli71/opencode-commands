---
description: Review branch changes and generate unit tests
---

You are reviewing code changes on the current git branch and generating unit tests.

## Branch Info

Current branch:!`git rev-parse --abbrev-ref HEAD`

Commits on this branch:!`git log --oneline $(git merge-base --fork-point HEAD 2>/dev/null || git merge-base HEAD main 2>/dev/null || git merge-base HEAD master 2>/dev/null)..HEAD`

## Diff of Changes!`git diff $(git merge-base --fork-point HEAD 2>/dev/null || git merge-base HEAD main 2>/dev/null || git merge-base HEAD master 2>/dev/null)..HEAD -- .`

## Task

1. Review the code changes above. Check for bugs, logic errors, style issues, security concerns, and potential improvements.
2. Create comprehensive unit tests for the new and modified code. Follow the project's existing test patterns, frameworks, and conventions.
3. Run the tests to confirm they pass. Fix any issues.
