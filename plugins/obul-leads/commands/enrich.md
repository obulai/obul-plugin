---
name: enrich
description: Enrich people, companies, LinkedIn profiles, verify emails, or search places. Usage: /obul-leads:enrich <query>
---

Enrich leads and discover contacts using StableEnrich's x402 API.

## Usage

```
/obul-leads:enrich person John Smith at Acme Corp
/obul-leads:enrich company acme.com
/obul-leads:enrich linkedin https://linkedin.com/in/jsmith
/obul-leads:enrich email verify john@acme.com
/obul-leads:enrich place coffee shops near Times Square
```

## Workflow

1. Ensure you are logged in via `obulx login`
2. Determine intent from the query:
   - Person search (default, or query starts with "person"): use `POST /api/apollo/people-search`
   - Person enrich (query includes an email or "enrich person"): use `POST /api/apollo/people-enrich`
   - Company search (query starts with "company" or "org"): use `POST /api/apollo/org-search`
   - Company enrich (query starts with "company" + a domain): use `POST /api/apollo/org-enrich`
   - LinkedIn lookup (query starts with "linkedin" or contains a linkedin.com URL): use `POST /api/clado/linkedin-scrape`
   - Contact discovery (query starts with "contacts"): use `POST /api/clado/contacts-enrich`
   - Email verification (query starts with "email verify" or "verify email"): use `POST /api/hunter/email-verifier`
   - Social enrichment (query starts with "social"): use `POST /api/influencer/enrich-by-social` or `enrich-by-email`
   - People lookup (query starts with "whitepages" or "lookup person"): use `POST /api/whitepages/person-search`
   - Place search (query starts with "place" or "places"): use `POST /api/google-maps/text-search/full`
3. Send the request through `https://stableenrich.dev{path}` using obulx
4. Display results in a readable format with key fields highlighted
