---
description: Generate a plan, review it for flaws, and implement it
---

## Feature Request

$ARGUMENTS

## Workflow

Follow these steps in order:

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

5. **Clarify any gaps in the plan** before implementing. If the plan is missing details needed to proceed (e.g., exact API contracts, UI behavior, naming conventions), use the `question` tool to ask the user.

6. **Implement the plan** step by step. Build tests alongside the implementation. Run tests to confirm everything passes. If you encounter an unexpected blocker or design decision not covered by the plan, stop and use the `question` tool to ask the user before proceeding.

7. Report a summary of what was implemented, what tests were added, and what files were changed.
