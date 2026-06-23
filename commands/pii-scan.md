---
description: Scan code, logs, and configs for PII/PHI exposure
subtask: true
---

## PII & PHI Scan

Scan the codebase for personally identifiable information (PII) and protected health information (PHI) that could be accidentally logged, serialized, exposed in error messages, or leaked through APIs.

### Scope

Target: $ARGUMENTS

If no scope is provided, scan staged changes first, then the full codebase.

### Staged / Branch Changes

!`git diff --staged 2>/dev/null || git diff $(git merge-base --fork-point HEAD 2>/dev/null || git merge-base HEAD main 2>/dev/null || git merge-base HEAD master 2>/dev/null)..HEAD -- .`

### What to Find

Scan for these categories in code, log statements, error messages, API responses, serialization logic, and debug output:

1. **Contact PII**
   - Email addresses
   - Phone numbers (various international formats)
   - Physical addresses, ZIP/postal codes
   - Full names concatenated with other identifiers

2. **Government IDs**
   - Social Security Numbers (SSN) — `###-##-####`
   - Passport numbers
   - Driver's license numbers
   - National ID numbers (various country formats)
   - Tax identification numbers (TIN, EIN, VAT)

3. **Financial Data**
   - Credit card numbers (PAN) — 13-19 digit patterns passing Luhn check
   - Bank account numbers, routing numbers
   - IBAN numbers
   - CVV/CVC codes (3-4 digit security codes)

4. **Authentication & Secrets**
   - API keys, tokens, passwords accidentally in log/trace output
   - JWTs or session tokens in debug logs
   - Private keys or certificates in source or logs
   - Connection strings with embedded credentials in log output

5. **Sensitive Identifiers**
   - IP addresses (when logged alongside user activity — could be PII under GDPR)
   - Device fingerprints or advertising IDs
   - User-agent strings when combined with other identifiers
   - Account numbers, customer IDs exposed publicly

6. **PHI (Healthcare)**
   - Medical record numbers
   - Health insurance IDs
   - Diagnosis codes (ICD-10), procedure codes (CPT/HCPCS)
   - Lab results, prescription data in logs
   - Any 18 HIPAA identifiers

7. **Location Data**
   - GPS coordinates in metadata or logs
   - Precise geolocation data
   - IP geolocation details

8. **Biometric & Behavioral**
   - Facial recognition data references
   - Fingerprint or voiceprint data
   - Keystroke dynamics or behavioral patterns

9. **Cookies & Tracking**
   - Tracking IDs, analytics user IDs in error payloads
   - Session IDs in URLs or log output

10. **Children's Data**
    - Any age fields that could indicate under-13 (COPPA)
    - Parental consent flags or guardian contact info

### How to Check

For each file in the diff/scope, examine:

1. **Log statements** — `console.log`, `logger.info`, `log.debug`, `print()`, `println!`, `tracing::info`, etc.
   - Are any objects containing user data being serialized in full?
   - Are request/response bodies being logged without sanitization?

2. **Error messages** — `throw new Error(...)`, `return res.status(500).json({...})`, `panic!()`, etc.
   - Do errors leak internal state, stack traces with user data, or raw SQL?
   - Are validation errors echoing back sensitive input values?

3. **API responses** — JSON serialization of domain objects
   - Do response DTOs exclude sensitive fields, or is the ORM model serialized directly?
   - Are there `toJSON()`, `Serialize`, or `toString()` implementations that leak fields?

4. **Debug endpoints** — `/debug`, `/health`, `/status`, `/metrics`
   - Do these expose user counts, request details, or internal state?

5. **Serialization annotations** — `@Expose`, `@JsonProperty`, `#[serde]`, `json:"-"` usage
   - Are sensitive fields excluded? Are there fields that should be excluded but aren't?

6. **Test fixtures** — test data files, seed scripts, migration data
   - Do they contain real PII instead of synthetic data?
   - Are there `.csv` or `.json` fixtures with real email addresses or names?

7. **Commit history** — accidentally committed files
   - !`git log --all --full-history --diff-filter=A -- '*.pem' '*.key' '*.pfx' '*.p12' '*.env' '*.credentials' 'credentials.*' 2>/dev/null | head -30`

### Output Format

Group findings by severity:

- **Critical** — PII in logs/errors/responses that will reach production monitoring, crash reporting, or end users. Fix immediately.
- **High** — PII in debug output, test fixtures, or committable files. Should be fixed before merge.
- **Medium** — Missing serialization exclusions, potential future leak paths. Address before next release.
- **Low** — Best practice improvements (masking, tokenization, field-level encryption).

For each finding:
- **Severity**
- **File:Line** — exact location
- **Category** — which PII type (email, SSN, credit card, etc.)
- **Issue** — what's exposed and how
- **Risk** — who could see it, regulatory impact (GDPR, HIPAA, PCI-DSS, CCPA)
- **Fix** — specific remediation (redact, mask, exclude from serialization, use tokenization)

### Rules

- Flag even test data if it looks like real PII (real-looking email patterns, valid credit card number patterns)
- `localhost`, `127.0.0.1`, `0.0.0.0`, `::1`, `example.com`, `test@example.com` are safe — don't flag
- Placeholder values like `XXXX`, `****`, `REDACTED` are intentional — acknowledge but don't flag as issues
- Environment variable references (`process.env.X`, `$ENV_VAR`) are safe — the value isn't in source
- If no PII issues are found, confirm the changes look clean and note any borderline cases that warrant human review
