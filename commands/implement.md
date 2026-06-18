---
description: Implement a finalized plan
subtask: true
---

## Plan File

$ARGUMENTS

If no plan file was provided, ask the user which plan to implement from `docs/plans/`.

## Workflow

1. Read the finalized plan at the path provided (e.g., `docs/plans/user-authentication-final.md`).

2. **Clarify any gaps.** If the plan is missing details needed to proceed (e.g., exact API contracts, UI behavior, naming conventions), use the `question` tool to ask the user before implementing.

3. **Implement the plan** step by step. Build tests alongside the implementation. Run tests to confirm everything passes.

4. If you encounter an unexpected blocker or design decision not covered by the plan, stop and use the `question` tool to ask the user before proceeding.

5. Report a summary of what was implemented, what tests were added, and what files were changed.
