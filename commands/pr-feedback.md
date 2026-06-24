---
description: Generate constructive, friendly PR review feedback
subtask: true
---

## PR Feedback Generator

Generate constructive, empathetic code review feedback for a pull request. Write like a helpful senior engineer, not an automated audit.

### Context — Understand the PR's purpose first

Before reviewing, ground yourself in what this PR is trying to accomplish.

**Branch:**
!`git rev-parse --abbrev-ref HEAD`

**PR Description (if available):**
!`gh pr view --json title,body 2>/dev/null || echo "No PR description found — infer intent from commits below."`

**Commit History:**
!`git log --format="%h %s%n%b---" $(git merge-base --fork-point HEAD 2>/dev/null || git merge-base HEAD main 2>/dev/null || git merge-base HEAD master 2>/dev/null)..HEAD 2>/dev/null || echo "No commits found"`

### PR Changes

!`git diff --staged 2>/dev/null || git diff $(git merge-base --fork-point HEAD 2>/dev/null || git merge-base HEAD main 2>/dev/null || git merge-base HEAD master 2>/dev/null)..HEAD -- .`

### Changed Files

!`git diff --name-only --staged 2>/dev/null || git diff --name-only $(git merge-base --fork-point HEAD 2>/dev/null || git merge-base HEAD main 2>/dev/null || git merge-base HEAD master 2>/dev/null)..HEAD`

### Task

**Step 1: Understand what this PR is trying to do.** Read the PR description and commit messages. What problem is being solved? What's the approach? If context is sparse, infer intent from the changed files and the shape of the diff.

**Step 2: Write PR review feedback based on that understanding.** Evaluate the changes against the stated or inferred goal — don't review in a vacuum. Structure it as you would write a real PR comment.

### Tone Guidelines

- **Assume good intent.** The author made the best decisions they could with the information they had.
- **Be specific, not dismissive.** Never say "this is wrong" — say "this could cause X when Y happens, consider Z instead."
- **Explain the why.** "This could be a race condition because..." not "Fix this."
- **Praise the good.** If something is well-structured or clever, call it out. Reviewers should reinforce good patterns.
- **Ask, don't demand.** "What do you think about..." or "Have you considered..." not "Change this to..."
- **Don't nitpick style.** Unless it violates a project convention or causes actual confusion, let personal style preferences go.
- **Use "we" not "you".** "We might want to extract this" not "You should extract this."

### What to Cover

1. **Start positive.** Find something genuinely good to acknowledge — even in a rough PR there's something.
2. **Logic & correctness concerns.** Frame as questions when possible: "What happens if this array is empty?"
3. **Design suggestions.** Offer alternatives, not directives: "Another approach we could take here..."
4. **Test coverage gaps.** "This edge case might be worth a test — what happens when..."
5. **Readability & maintenance.** Only flag things that will genuinely confuse future readers.
6. **Performance concerns.** Only flag if it matters: "This runs inside the render loop — could be a hotspot."
7. **End encouraging.** "This is looking great, just a few thoughts." Even if there are many comments.

### Output Format

Write the feedback as if you're posting it directly on a GitHub PR. Use:

```markdown
**Great work on X!** The approach to Y is clean and well-structured.

---

**A few thoughts as I read through:**

### In `@path/to/file.ts`

**What about the empty case?** If `items` is an empty array, this loop silently produces an empty result. Should we handle that explicitly or is that the intended behavior?

```typescript
// Suggestion: add a guard
if (items.length === 0) return defaultValue;
```

**Nit:** The variable name `tmp` could be more descriptive here — maybe `intermediateResult`? Not a blocker, just a thought.

---

### In `@path/to/other.ts`

**This pattern looks familiar.** We have a similar transformation in `@utils/transformers.ts` — could we reuse that instead? Happy to pair on that if it helps.

---

**Overall this is really solid.** The error handling is thorough, and the tests cover the main paths well. Let me know when you've had a chance to look at these — happy to re-review after updates.
```

### Rules

- Never suggest changes that would break the build or tests.
- Don't flag issues you can't provide a concrete alternative for.
- If there are no issues to flag, write a short approving review. Don't invent criticism.
- Keep the full review under 20 comments — if there are more issues, group minor ones together and offer to pair.
