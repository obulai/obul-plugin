# Obul Plugin Marketplace for Claude Code

Pay-per-use API access for Claude Code agents — web scraping, search, social media, crypto, image generation, audio, and security. No external accounts needed — install `obulx` and log in.

## Prerequisites

1. Install the Obul CLI:
   ```sh
   npm install -g @obul.ai/obulx
   ```
2. Log in:
   ```sh
   obulx login
   ```

## Installation

Add this repo as a marketplace, then install all plugins:

```sh
claude plugin marketplace add https://github.com/obulai/obul-plugin.git
claude plugin install obul-core obul-scrape obul-search obul-social obul-crypto obul-media obul-security obul-leads
```

Or install only the categories you need:

```sh
claude plugin install obul-scrape obul-search
```

## Available Plugins

### obul-core — Infrastructure & Proxy
| Skill | Description | Pricing |
|---|---|---|
| obul-proxy | Generic x402 proxy for any supported upstream service | Varies |
| pinata | IPFS pinning and content retrieval via Pinata | $0.0001–$0.10 |
| cnvrting | File format conversion (PDF, DOCX, images, audio, video) | Free–$0.05 |

### obul-scrape — Web Scraping & Extraction
| Skill | Description | Pricing |
|---|---|---|
| firecrawl | Scrape, crawl, map, and extract structured data from websites | $0.001–$0.01 |
| browserbase | Cloud browser sessions with Puppeteer/Playwright for JS-heavy sites | $0.01 |
| zyte | Anti-bot scraping with JavaScript rendering at scale | $0.08–$0.76/1000 |
| minifetch | URL metadata extraction (OG tags, links, JSON-LD) | $0.002 |
| x402engine-web | Quick scrape and screenshot | $0.005–$0.01 |

### obul-search — Web Search
| Skill | Description | Pricing |
|---|---|---|
| firecrawl-search | Web search with auto-scraped markdown content | $0.002 |
| exa | Neural/semantic search, find-similar, AI answers | $0.002–$0.01 |

### obul-social — Social Media
| Skill | Description | Pricing |
|---|---|---|
| x-search | X/Twitter search, profiles, tweets, timelines, communities | $0.0025–$0.01 |
| neynar | Farcaster casts, profiles, channels, feeds | ~$0.01 |
| reddit | Reddit search and post comments | $0.02 |

### obul-crypto — Blockchain & DeFi
| Skill | Description | Pricing |
|---|---|---|
| heyelsa | DeFi actions — swap, bridge, stake, lend/borrow across chains | $0.001–$0.05 |
| zapper | Portfolio tracking, NFTs, token balances via GraphQL | $0.003–$0.012 |
| slamai | Smart money tracking, whale alerts, token analytics | $0.0001–$0.003 |
| silverback | DeFi analytics — TVL, yields, protocol metrics, liquidations | $0.001–$0.10 |
| blocksec | AML compliance, address risk scoring, transaction tracing | $0.10–$1.00 |
| x402engine-chain | Wallet balances, transactions, ENS, token prices | $0.001–$0.01 |
| ordiscan | Bitcoin Ordinals inscriptions and BRC-20 data | ~$0.01 |

### obul-media — Image, Audio & Video
| Skill | Description | Pricing |
|---|---|---|
| freepik | Mystic image generation with multiple styles | $0.02–$0.05 |
| x402engine-image | FLUX Schnell (fast) and FLUX.2 Pro (quality) image gen | $0.015–$0.05 |
| x402engine-audio | OpenAI TTS, ElevenLabs TTS, Deepgram transcription | $0.01–$0.10 |
| dtelecom | Speech-to-text transcription via dTelecom | $0.005/min |
| genbase | Video generation with Sora 2 — text-to-video and image-to-video | $0.02–$0.20 |

### obul-security — Security & Risk
| Skill | Description | Pricing |
|---|---|---|
| orac | Prompt injection detection and code security auditing | $0.005–$0.02 |
| blackswan | Real-time risk intelligence and threat detection | $0.01–$0.03 |

### obul-leads — Lead Enrichment
| Skill | Description | Pricing |
|---|---|---|
| stableenrich | People & company enrichment (Apollo), LinkedIn scraping (Clado), email verification (Hunter), social enrichment (Influencer), people lookup (Whitepages), places search (Google Maps) | $0.02–$0.495 |

## Commands

| Command | Plugin | Description |
|---------|--------|-------------|
| `/obul-core:proxy` | core | Proxy any request through Obul |
| `/obul-scrape:scrape` | scrape | Scrape a webpage |
| `/obul-scrape:screenshot` | scrape | Screenshot a webpage |
| `/obul-search:search` | search | Search the web |
| `/obul-social:x-search` | social | Search X/Twitter |
| `/obul-social:farcaster` | social | Search Farcaster |
| `/obul-crypto:price` | crypto | Get crypto prices |
| `/obul-crypto:wallet` | crypto | Look up wallet info |
| `/obul-media:imagine` | media | Generate an image |
| `/obul-media:transcribe` | media | Transcribe audio or generate speech |
| `/obul-security:audit` | security | Run security analysis |
| `/obul-leads:enrich` | leads | Enrich people, companies, LinkedIn, verify emails, search places |

## How It Works

All requests route through the [Obul proxy](https://obul.ai) via the `obulx` CLI, which handles [x402](https://www.x402.org/) payment negotiation and authentication automatically. Just pass upstream URLs to `obulx` — no crypto wallets, no USDC, no gas fees.

## Resources

- [Obul Dashboard](https://my.obul.ai) — manage usage and spending caps
- [x402 Protocol](https://www.x402.org/) — the payment protocol powering Obul
- [Skill Definitions](plugins/) — full API reference for each service

## Version

Current: **0.2.1**
