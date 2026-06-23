---
description: Scan for unused exports, dead code paths, and orphaned code
subtask: true
---

## Dead Code Detection

Scan the codebase for dead code — exports, functions, and code paths that are never reached or no longer used.

### Scope

Target: $ARGUMENTS

If no scope is provided, scan the entire codebase.

### Language-Specific Scans

!`if [ -f package.json ]; then echo "=== TypeScript/JavaScript ===" && npx ts-prune 2>/dev/null || npx ts-unused-exports 2>/dev/null || echo "No TS dead code tools found"; fi`
!`if [ -f Cargo.toml ]; then echo "=== Rust ===" && cargo udeps 2>/dev/null || echo "cargo-udeps not installed (run: cargo install cargo-udeps)"; fi`
!`if [ -f go.mod ]; then echo "=== Go ===" && go run honnef.co/go/tools/cmd/staticcheck@latest -checks U1000 ./... 2>/dev/null || echo "staticcheck not available"; fi`
!`if [ -f pyproject.toml ] || [ -f setup.py ] || [ -f requirements.txt ]; then echo "=== Python ===" && vulture . 2>/dev/null || echo "vulture not installed (pip install vulture)"; fi`

### If No Automated Tools Available

Use grep/ripgrep to find candidates manually:
!`rg --no-heading -n "^export (const|function|class|type|interface|enum|default)" --include='*.ts' --include='*.tsx' 2>/dev/null | head -100 || true`

### What to Look For

1. **Unused Exports**
   - Exported functions, classes, types, constants never imported elsewhere
   - Public API surface that accumulated cruft over refactors
   - Re-exported symbols from barrel files that no one consumes

2. **Unreachable Code**
   - Code after unconditional `return`, `throw`, `break`, `continue`
   - Branches guarded by impossible conditions
   - Functions that only call themselves recursively with no base case
   - Callbacks that are registered but never triggered

3. **Orphaned Code**
   - Test files for deleted source files
   - Config files referencing removed features
   - Scripts that reference deleted entry points
   - Documentation referencing dead commands or APIs

4. **Dependency Ghosts**
   - Dependencies imported but only their types are used (should move to devDependencies)
   - Packages imported but never actually called
   - Polyfills for features now natively supported

### Output Format

Organize findings into:
- **Confirmed Dead** — safe to delete with high confidence
- **Suspicious** — appears unused but warrants manual verification (e.g., test utilities, plugin hooks)
- **Dependency Issues** — import hygiene problems

For each finding:
- **File:Line** — location
- **Symbol** — the function/class/export name
- **Confidence** — High / Medium / Low
- **Reason** — why it's flagged
- **Action** — what to do (delete, move to devDeps, add `@deprecated` tag, etc.)

### Safety Notes

- Be conservative with public API surface — flag but don't recommend deleting exports that are part of the documented public API
- Dynamic imports and reflection-based calls will not be detected — note this limitation
- Export default symbols often have false positives — investigate before recommending deletion
- Don't count test files or `.d.ts` re-exports as "used" when analyzing source file usage
