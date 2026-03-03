# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repo Is

The **Obul Plugin Marketplace** — a collection of Claude Code plugins providing pay-per-use API access to x402 services through the Obul proxy (`proxy.obul.ai`). Each plugin bundles related skills by category. Each skill wraps an external service's x402 API so that agents can call it with just an `OBUL_API_KEY`.

There is no application code, no build system, no tests. The repo is documentation only.

## Repository Structure

```
.claude-plugin/marketplace.json       # Marketplace manifest (lists all 8 plugins)
registry.md                           # Registry reference and publishing guide
plugins/
  <plugin-name>/                      # Category plugin (e.g., obul-scrape, obul-search)
    .claude-plugin/plugin.json        # Plugin manifest
    skills/
      <service-name>/
        SKILL.md                      # Skill definition file
    commands/
      <command>.md                    # User-invocable slash commands
    rules/
      setup.md                        # Auth setup rule (shared)
```

### Plugin Categories

| Plugin | Category | Skills |
|--------|----------|--------|
| `obul-core` | Infrastructure | obul-proxy, pinata, cnvrting |
| `obul-scrape` | Web scraping | firecrawl, browserbase, zyte, minifetch, x402engine-web |
| `obul-search` | Web search | firecrawl-search, exa, geo |
| `obul-social` | Social media | x-search, neynar, reddit |
| `obul-crypto` | Blockchain/DeFi | coingecko, heyelsa, zapper, slamai, silverback, blocksec, x402engine-chain, ordiscan |
| `obul-media` | Image/Audio/Video | freepik, x402engine-image, x402engine-audio, dtelecom, aibeats, genbase |
| `obul-security` | Security/Risk | orac, blackswan |
| `obul-leads` | Lead Enrichment | stableenrich |

## Skill File Format

Every `SKILL.md` follows this structure:

1. **YAML frontmatter** — `name` (kebab-case), `description`, `homepage`, `metadata.obul-skill` (emoji, requires.env, requires.primaryEnv), `registries` (dict of registry → published slug)
2. **H1 title** — Service name
3. **Overview paragraph** — What the service does, that it's pay-per-use through Obul
4. **Authentication** — Always the same Obul proxy pattern with `x-obul-api-key: {{OBUL_API_KEY}}` header and the base URL
5. **Common Operations** (3-5) — Each with H3 header, description, `**Pricing:** $X.XX`, JSON request example, and response description
6. **Endpoint Pricing Reference** — Table of all endpoints with price and purpose
7. **When to Use** — Bullet list of use cases
8. **Best Practices** — Service-specific tips
9. **Error Handling** — Table of error codes, causes, and solutions

## Key Conventions

- A linter auto-reformats SKILL.md on save (table alignment, YAML arrays, line wrapping) — do not revert linter changes
- Skill name in frontmatter must NOT include "x402"
- Pricing always in dollar amounts (e.g., `$0.01`), never credits
- All request examples use full Obul proxy URLs: `https://proxy.obul.ai/proxy/https/<upstream-host>/...`
- All requests include `Content-Type: application/json` and `x-obul-api-key: {{OBUL_API_KEY}}` headers
- Pricing tiers: `$0.00` (health checks), `$0.0001` (polling), `$0.001–$0.01` (simple queries), `$0.01–$0.05` (complex queries), `$0.05+` (heavy operations)
- When a skill is published to a registry, add a `registry-name: published-slug` entry to `registries` in that skill's frontmatter
- Registry names (keys) must match the H3 anchor IDs in `registry.md`
- The published slug is `obul-` prefixed to the skill name (e.g., `clawhub: "obul-browserbase"`)

## Writing a New Skill

Reference guide: https://github.com/dpbmaverick98/Agent_Army_Skills/blob/main/Agent_Army_Skills_Obul/how-to-write-obul-skills.md

Workflow: (1) Research the service's x402 API thoroughly via web search, (2) use an existing skill under `plugins/<category>/skills/` as a template, (3) create `plugins/<category>/skills/<service-name>/SKILL.md`.
