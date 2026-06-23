---
description: Generate or update documentation for changed code
subtask: true
---

## Documentation — Changed Code

Review recent changes and generate or update documentation accordingly.

### Changes

!`git diff HEAD~1`

### Changed Files

!`git diff --name-only HEAD~1`

### Existing Documentation Structure

!`find . -name "*.md" -not -path "*/node_modules/*" -not -path "*/.git/*" | head -30`

### Task

1. **Identify what needs documentation**
   - New public APIs, functions, classes, or modules without docs
   - Modified behavior that makes existing docs inaccurate
   - New configuration options, environment variables, or feature flags
   - Architectural changes that affect README or architecture docs

2. **Generate documentation**
   - **Code-level**: JSDoc/TSDoc/docstrings for new public interfaces
   - **API-level**: Update OpenAPI/Swagger specs if applicable
   - **User-level**: Update README, getting started guides, or changelog entries
   - **Internal**: Update architecture decision records (ADRs) if patterns changed

3. **Follow project conventions**
   - Match existing doc style, tone, and format
   - Use the same documentation framework (Docusaurus, MkDocs, etc.)
   - Place docs in the correct directory structure

4. **Quality checks**
   - No stale references to old function names or paths
   - Code examples in docs actually work
   - Links between docs are valid
   - New docs are discoverable from existing navigation

5. **Output**
   - List all documentation created or updated
   - Highlight any docs that need human review (complex architectural decisions, user-facing copy)
