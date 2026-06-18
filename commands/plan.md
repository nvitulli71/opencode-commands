---
description: Plan a feature with review, no code changes
agent: plan
subtask: true
---

## Feature Request

$ARGUMENTS

## Workflow

Follow these steps in order. YOU MUST NOT MAKE ANY CODE CHANGES OR EDIT ANY EXISTING FILES.

0. **Clarify ambiguities first.** Before writing the plan, use the `question` tool to ask the user about anything unclear or underspecified:
   - Scope boundaries (what's in vs. out)
   - Preferred patterns or conventions when multiple valid approaches exist
   - Edge cases that could be interpreted multiple ways
   - Dependencies or constraints you're unsure about
   - Non-functional requirements (performance targets, accessibility, browser support, etc.)

1. **Create a detailed implementation plan.** Include:
   - Overview and goals
   - Technical approach
   - Files to be created or modified
   - Step-by-step implementation order
   - Dependencies, risks, and edge cases

2. **Save the initial plan** to the `docs/plans/` directory. Derive a filename from the feature name using kebab-case and appending draft to the end of it (e.g., `docs/plans/user-authentication-draft.md`).

3. **Critically review the plan** for:
   - Missing edge cases or requirements
   - Architectural flaws or inconsistencies
   - Potential bugs or security issues
   - Integration concerns with existing code
   - Missing test coverage

4. **Save the reviewed version** as `<original-name>-final.md` in `docs/plans/` (e.g., `docs/plans/user-authentication-final.md`).

5. After saving the final plan, report back the plan filename so the user can review and then run `/implement <plan-filename>` to proceed.
