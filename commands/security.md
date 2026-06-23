---
description: Security review of staged changes before commit
subtask: true
---

## Security Review — Staged Changes

Review the currently staged changes for security issues before committing.

### Staged Diff

!`git diff --staged`

### What to Check

1. **Secrets & Credentials**
   - Hardcoded API keys, tokens, passwords, connection strings
   - Accidental `.env` or credential file staging
   - Private keys or certificates in source

2. **Injection Risks**
   - SQL/NoSQL queries with string concatenation
   - Command injection via `exec()`, `spawn()`, `system()` with user input
   - `eval()`, `new Function()`, dynamic code execution

3. **Input Validation**
   - User input used without sanitization or validation
   - Missing type checks on external data
   - Path traversal via unsanitized file paths

4. **Auth & Authorization**
   - New endpoints or routes missing auth checks
   - Permission checks only on client side
   - JWT verification weaknesses (alg:none, missing signature check)

5. **Data Exposure**
   - Logging sensitive data (passwords, tokens, PII)
   - Stack traces or debug info in error responses
   - Overly verbose error messages leaking internals

6. **Cryptography**
   - Weak hashing (MD5, SHA1 for passwords)
   - ECB mode, fixed IVs, hardcoded salts
   - Custom crypto instead of established libraries

### Output Format

For each finding:
- **Severity**: Critical / High / Medium / Low
- **File:Line**
- **Issue**: What's wrong and why it matters
- **Fix**: Specific remediation

If no issues found, confirm the changes look clean from a security perspective.
