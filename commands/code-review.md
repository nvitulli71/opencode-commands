---
description: Deep review of code changes for correctness, style, and anti-patterns
subtask: true
---

## Code Review

Perform a thorough code review of the changes on this branch. Go deep.

### Branch Info

Current branch: !`git rev-parse --abbrev-ref HEAD`

### Changes to Review

!`git diff $(git merge-base --fork-point HEAD 2>/dev/null || git merge-base HEAD main 2>/dev/null || git merge-base HEAD master 2>/dev/null)..HEAD -- .`

### Changed Files

!`git diff --name-only $(git merge-base --fork-point HEAD 2>/dev/null || git merge-base HEAD main 2>/dev/null || git merge-base HEAD master 2>/dev/null)..HEAD`

### Commit Messages

!`git log --oneline $(git merge-base --fork-point HEAD 2>/dev/null || git merge-base HEAD main 2>/dev/null || git merge-base HEAD master 2>/dev/null)..HEAD`

### What to Look For

1. **Correctness & Logic**
   - Logic errors, off-by-one mistakes, inverted conditions
   - Missing edge cases (null, empty, error states)
   - Race conditions in async/concurrent code
   - Implicit type coercion that could surprise (== vs ===, truthy/falsy gotchas)

2. **Anti-Patterns & Design**
   - Violations of the project's established patterns
   - Overly clever code that sacrifices readability
   - Missing abstractions or premature abstractions
   - Tight coupling, leaky abstractions, god objects
   - Inconsistent naming with the surrounding codebase

3. **Performance Pitfalls**
   - Unnecessary allocations in hot paths
   - N+1 query patterns
   - Missing memoization or caching where appropriate
   - Synchronous blocking in async contexts
   - Large objects passed by copy instead of reference

4. **Error Handling**
   - Swallowed errors (empty catch blocks)
   - Missing error propagation
   - Errors that lose context (no wrapping, no stack preservation)
   - Inappropriate recovery vs. fail-fast

5. **Test Adequacy**
   - Are the right things tested? (behavior, not implementation)
   - Missing edge case or negative path tests
   - Brittle tests (overly specific assertions, test ordering dependencies)
   - Test descriptions that don't match the assertion

6. **Maintainability**
   - Magic numbers and strings without explanation
   - Functions that do too many things
   - Dead or commented-out code left behind
   - TODOs without context or ticket references

### Output Format

Group findings by severity: **Must Fix** / **Should Fix** / **Nit**

For each finding:
- **File:Line** — exact location
- **Issue** — what's wrong and why it matters
- **Suggestion** — concrete fix or alternative approach
- **Code** — suggested diff if applicable

If the changes look solid, say so clearly at the top — don't manufacture issues.
