---
name: orac
description: AI agent security infrastructure through the Orac Safety Layer. Use when the user needs to scan prompts for injection attacks or audit code for security vulnerabilities.
---

# Orac

Orac provides AI agent security infrastructure through the Safety Layer. Scan prompts for injection attacks and audit code for security vulnerabilities.

## Common Operations

### Prompt Injection Scan

Scan a prompt or user input for injection attacks.

**Pricing:** $0.005

**Request:**
```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{"input": "the text to scan for prompt injection"}' \
  "https://orac-safety.orac.workers.dev/v1/scan"
```

**Response:** JSON object with injection detection results including risk assessment and classification.

### Code Vulnerability Audit

Audit code for security vulnerabilities.

**Pricing:** $0.02

**Request:**
```sh
obulx -X POST -H "Content-Type: application/json" \
  -d '{"code": "function example() { eval(userInput); }", "language": "javascript"}' \
  "https://orac-safety.orac.workers.dev/v1/audit"
```

**Response:** JSON object with identified vulnerabilities, severity ratings, and remediation suggestions.

## Endpoint Pricing Reference

| Endpoint         | Price | Purpose                           |
|------------------|-------|-----------------------------------|
| `POST /v1/scan` | $0.005 | Prompt injection scan            |
| `POST /v1/audit` | $0.02 | Code vulnerability audit          |

## When to Use

- **Prompt security** — User asks to check a prompt or user input for injection attacks
- **Code audit** — User wants to audit code (skill source, plugin code, agent logic) for security vulnerabilities
- **Input validation** — User needs to sanitize or validate LLM inputs before execution
- **Security scanning** — User mentions "prompt injection", "code audit", or "security scan"

## Best Practices

- **Scan before execution** — Always scan user inputs before executing them as prompts
- **Audit agent code** — Periodically audit your agent's code for security vulnerabilities
- **Use appropriate language** — Specify the correct programming language for code audits

## Error Handling

| Error                       | Cause                                    | Solution                                                                     |
|-----------------------------|------------------------------------------|------------------------------------------------------------------------------|
| `402 Payment Required`      | Payment not processed or insufficient    | Check that `obulx` is configured correctly and your account has balance.     |
| `400 Bad Request`           | Invalid request body                    | Ensure required fields are present and correctly formatted.                   |
| `429 Too Many Requests`    | Rate limit exceeded                      | Add a short delay between requests.                                           |
| `500 Internal Server Error` | Orac service issue                      | Wait a few seconds and retry. If persistent, the service may be experiencing downtime. |
