---
description: Generate a comprehensive and well-structured PR description
subtask: true
---

## PR Description Generator

Generate a thorough, ready-to-paste pull request description from the changes on this branch.

### Branch Info

Current branch: !`git rev-parse --abbrev-ref HEAD`

### Full Diff

!`git diff $(git merge-base --fork-point HEAD 2>/dev/null || git merge-base HEAD main 2>/dev/null || git merge-base HEAD master 2>/dev/null)..HEAD -- .`

### Changed Files

!`git diff --name-only $(git merge-base --fork-point HEAD 2>/dev/null || git merge-base HEAD main 2>/dev/null || git merge-base HEAD master 2>/dev/null)..HEAD`

### Commit History

!`git log --format="%h %s%n%b---" $(git merge-base --fork-point HEAD 2>/dev/null || git merge-base HEAD main 2>/dev/null || git merge-base HEAD master 2>/dev/null)..HEAD`

### Task

Write a PR description that includes:

1. **Title** (conventional commit style: `type(scope): description`)
   - Type: feat, fix, refactor, perf, docs, test, chore
   - Scope: the affected module/component
   - Description: imperative mood, concise, under 72 chars

2. **Summary** (2-4 sentences)
   - What problem does this solve? Why now?
   - High-level approach taken
   - Impact: who does this affect and how?

3. **Changes** (bulleted list)
   - Group related changes together
   - Focus on what changed from the user's perspective
   - Mention notable implementation decisions
   - Skip trivial/internal-only changes

4. **Testing**
   - How was this tested? (manual steps, unit tests, integration tests)
   - What edge cases were considered?
   - Any areas that need extra reviewer attention during testing?
   - Steps to reproduce/test the change locally

5. **Screenshots / Demos** (if UI changes)
   - Describe what visual changes to capture
   - Note before/after states

6. **Breaking Changes** (if any — call them out prominently)
   - What broke?
   - Migration path for consumers
   - Deprecation timeline if applicable

7. **Checklist**
   - [ ] Tests added/updated
   - [ ] Documentation updated
   - [ ] Breaking changes documented
   - [ ] Self-reviewed

### Guidelines

- Write for your reviewer: help them understand your intent quickly
- Be specific — avoid "various fixes" or "improvements"
- Link to related issues/tickets if visible in the branch name or commits
- If this is a draft/WIP, note what's remaining
- Keep it professional but not sterile — you're writing to humans
