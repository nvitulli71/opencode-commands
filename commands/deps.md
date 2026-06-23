---
description: Audit dependencies for vulnerabilities and bloat
subtask: true
---

## Dependency Audit

Audit project dependencies for security vulnerabilities, outdated packages, and unnecessary bloat.

### Package Manager Detection

!`ls package.json Cargo.toml go.mod requirements.txt pyproject.toml Gemfile composer.json 2>/dev/null`

### Current Dependencies

!`if [ -f package.json ]; then echo "=== npm ===" && npm ls --depth=0 2>/dev/null; fi`
!`if [ -f Cargo.toml ]; then echo "=== cargo ===" && cargo tree --depth=1 2>/dev/null; fi`
!`if [ -f requirements.txt ]; then echo "=== pip ===" && pip list 2>/dev/null; fi`
!`if [ -f go.mod ]; then echo "=== go ===" && go list -m all 2>/dev/null; fi`

### Security Audit

!`if [ -f package.json ]; then npm audit 2>/dev/null; fi`
!`if [ -f Cargo.toml ]; then cargo audit 2>/dev/null; fi`
!`if [ -f requirements.txt ]; then pip audit 2>/dev/null; fi`

### Task

1. **Security scan**
   - Report all known CVEs in current dependencies
   - Flag dependencies with no recent security patches
   - Identify transitive vulnerabilities (deps of deps)

2. **Outdated packages**
   - List packages with major version updates available
   - Note packages that haven't been updated in 12+ months
   - Flag deprecated or unmaintained packages

3. **Bloat analysis**
   - Identify unused or dead dependencies (imported but not used)
   - Flag oversized packages where a smaller alternative exists
   - Check for duplicate packages at different versions in the tree
   - Note devDependencies that leaked into production builds

4. **License compliance**
   - Flag any dependencies with restrictive licenses (GPL, AGPL) in proprietary projects
   - Note any license changes in recent updates

5. **Recommendations**
   - Prioritized list of updates (security first, then features, then maintenance)
   - Suggested replacements for bloated or unmaintained packages
   - Commands to run for safe updates
