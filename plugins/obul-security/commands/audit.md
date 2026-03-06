---
name: audit
description: Run security analysis on code or prompts. Usage: /obul-security:audit <target>
---

Run a security audit on the provided target using Orac and optionally BlackSwan.

## Workflow

1. **Check prerequisites**: Ensure you are logged in via `obulx login`.

2. **Determine target type**:
   - If the target is a file path or code snippet, run a **code vulnerability audit** via Orac `/v1/audit`.
   - If the target is a prompt or natural language text, run a **prompt injection scan** via Orac `/v1/scan`.
   - If unclear, ask the user to clarify.

3. **Run Orac scan**:

   For prompt injection detection:
   ```bash
   obulx -X POST -H "Content-Type: application/json" \
     -d '{"input": "<target text>"}' \
     "https://orac-safety.orac.workers.dev/v1/scan"
   ```

   For code vulnerability audit (read the file first, then send its contents):
   ```bash
   obulx -X POST -H "Content-Type: application/json" \
     -d '{"code": "<file contents>", "language": "<detected language>"}' \
     "https://orac-safety.orac.workers.dev/v1/audit"
   ```

4. **Optional risk assessment**: If the user requests a broader risk check or the audit target involves crypto/market operations, also call BlackSwan Flare for a quick precursor signal:
   ```bash
   obulx -X POST -H "Content-Type: application/json" \
     -d '{}' \
     "https://x402.blackswan.wtf/smart-agents/flare"
   ```

5. **Report results**: Display findings with severity levels, risk scores, and remediation suggestions. Summarize the cost of each API call made.
