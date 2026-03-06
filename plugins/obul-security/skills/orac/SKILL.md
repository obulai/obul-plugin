---
name: orac
description: Scan prompts for injection attacks or audit code for security vulnerabilities using Orac Safety Layer. Use when users need prompt injection detection, code security auditing, or input sanitization checks.
---

# orac Skill

AI agent security infrastructure via the Orac Safety Layer. Provides prompt injection scanning and code vulnerability auditing, paid per request through x402.

## When to Use
- User asks to check a prompt or user input for injection attacks
- User wants to audit code (skill source, plugin code, agent logic) for security vulnerabilities
- User needs to sanitize or validate LLM inputs before execution
- User mentions "prompt injection", "code audit", or "security scan"

## Endpoints

### Prompt Injection Scan
- **URL**: `https://orac-safety.orac.workers.dev/v1/scan`
- **Method**: POST
- **Pricing**: ~$0.005 USDC per request

**Request:**
```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{"input": "the text to scan for prompt injection"}' \
  "https://orac-safety.orac.workers.dev/v1/scan"
```

**Response:** JSON object with injection detection results including a risk assessment and classification of whether the input contains prompt injection attempts.

### Code Vulnerability Audit
- **URL**: `https://orac-safety.orac.workers.dev/v1/audit`
- **Method**: POST
- **Pricing**: ~$0.02 USDC per request

**Request:**
```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{"code": "function example() { eval(userInput); }", "language": "javascript"}' \
  "https://orac-safety.orac.workers.dev/v1/audit"
```

**Response:** JSON object with identified vulnerabilities, severity ratings, and remediation suggestions for the submitted code.

## Notes
- All requests go through the Obul proxy; do not call `orac-safety.orac.workers.dev` directly.
- Orac also offers agent trust scoring powered by a 265-entity knowledge graph with PageRank reputation, though this endpoint is not yet documented in detail.
- Payment is in USDC on Base, handled automatically by the proxy.
