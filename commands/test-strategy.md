---
description: Analyze changes and identify missing test coverage
subtask: true
---

## Test Strategy — Recent Changes

Analyze the code changes and identify what needs to be tested.

### Changes

!`git diff HEAD~1`

### New/Modified Files

!`git diff --name-only HEAD~1`

### Existing Test Patterns

!`find . -name "*.test.*" -o -name "*.spec.*" -o -name "*_test.*" | head -20`

### Task

1. **Identify what changed**
   - List all new functions, methods, classes, or modules
   - List all modified logic paths
   - Note any deleted code that had tests

2. **Map test gaps**
   - For each new/changed function: what test cases are missing?
   - What edge cases are not covered? (null/undefined, empty arrays, boundary values, error paths)
   - What integration scenarios need testing?
   - Are there race conditions or async behaviors that need specific test patterns?

3. **Prioritize**
   - **Must test**: Core business logic, auth, data mutations, public APIs
   - **Should test**: Error handling, edge cases, utility functions with complex logic
   - **Nice to test**: Trivial getters/setters, pure formatting functions

4. **Generate test plan**
   - For each gap, specify: test file, test name, what it verifies, and expected behavior
   - Follow the project's existing test framework and conventions

5. **Write the tests**
   - Create the identified test files
   - Run tests and confirm they pass
   - Report coverage impact
