---
description: Generate meaningful changelog from recent commits
subtask: true
---

## Changelog Generation

Generate a well-structured changelog from recent git commits.

### Recent Commits

!`git log --oneline -20`

### Detailed Commits

!`git log --format="%h %s%n%b---" -20`

### Existing Changelog

!`if [ -f CHANGELOG.md ]; then head -100 CHANGELOG.md; elif [ -f CHANGES.md ]; then head -100 CHANGES.md; elif [ -f HISTORY.md ]; then head -100 HISTORY.md; else echo "No existing changelog found"; fi`

### Task

1. **Analyze commits**
   - Group commits by type: Features, Fixes, Performance, Security, Breaking Changes, Internal
   - Identify which commits are user-facing vs internal-only
   - Merge related commits into single changelog entries
   - Ignore: merge commits, chore commits, WIP commits, revert commits (unless the revert itself is notable)

2. **Write changelog entries**
   - **Format**: Follow [Keep a Changelog](https://keepachangelog.com/) conventions
   - **Style**: User-focused, not commit-focused. "Added X" not "Implemented X in module Y"
   - **Detail**: Include enough context for users to understand the impact
   - **Breaking changes**: Highlight prominently with migration notes if applicable
   - **Credits**: Include contributor names/PR numbers if available

3. **Output**
   - Generate the changelog section in markdown
   - Show where it should be inserted in the existing changelog
   - Flag any commits that are unclear and need human clarification
   - Suggest a version number bump based on semver (major/minor/patch)
