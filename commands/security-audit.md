---
description: Scan code for security vulnerabilities
subtask: true
---

## Target

$ARGUMENTS

If no target is provided, audit the entire codebase.

## What to search for

1. **Injection vulnerabilities**
   - SQL/NoSQL injection: unparameterized queries, string concatenation in queries (`"SELECT * FROM " + input`, template literals in raw queries)
   - Command injection: `exec()`, `spawn()`, `system()` with user-controlled input
   - Code injection: `eval()`, `new Function()`, `vm.runInNewContext()` with dynamic input

2. **Cross-site scripting (XSS)**
   - `dangerouslySetInnerHTML`, `innerHTML`, `document.write()` with unsanitized input
   - Unescaped template output (`{{{ }}}` in Handlebars, missing `| safe` guards)

3. **Authentication & authorization**
   - Missing auth checks on routes/endpoints (compare to how other routes enforce auth)
   - Role/permission checks done only client-side
   - Hardcoded credentials or API keys (look for `password = "`, `secret = "`, `apiKey = "`, `token = "`)
   - Weak JWT verification (alg:none, no signature check, long expiry)

4. **Exposed secrets**
   - `.env` files committed, config files with credentials
   - Private keys, tokens, or passwords in source code or comments
   - AWS/GCP/Azure keys, Stripe/PayPal keys, database connection strings with credentials

5. **Path traversal & file access**
   - `fs.readFile()`, `fs.writeFile()`, `path.join()` with user-controlled paths
   - File upload without path sanitization or extension whitelisting
   - Zip slip in archive extraction

6. **Insecure deserialization**
   - `JSON.parse()` followed by arbitrary code execution
   - `serialize-javascript`, `node-serialize`, or pickle deserialization without type validation

7. **Cryptography weaknesses**
   - MD5, SHA1 used for password hashing instead of bcrypt/scrypt/argon2
   - ECB mode, fixed IVs, weak key generation
   - Hardcoded salts or keys

8. **Leaky logging & errors**
   - `console.log(user.password)`, logging tokens/sessions
   - Stack traces exposed in production error responses
   - Debug mode enabled in non-development environments

9. **CSRF & CORS**
   - State-changing endpoints (POST/PUT/DELETE) without CSRF tokens (if cookie-based auth)
   - Overly permissive CORS (`Access-Control-Allow-Origin: *` with credentials)
   - Missing `SameSite` cookie attribute

10. **Dependency risks**
    - Run `npm audit` / `pip audit` / `cargo audit` to surface known CVEs
    - Check for unmaintained or deprecated packages
    - Typosquatting: look for package names that resemble popular packages with slight misspellings

## Workflow

1. Determine the target scope from `$ARGUMENTS`. If empty, audit all source files.
2. Search the target for each category above using code search and file reads.
3. Run dependency audit with the project's package manager.
4. For each finding, report:
   - Severity (Critical / High / Medium / Low)
   - File path and line number
   - Snippet of the vulnerable code
   - Why it's dangerous
   - Specific fix recommendation
5. Present a summary table of findings sorted by severity.
