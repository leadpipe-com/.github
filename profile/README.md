# Leadpipe

**Know who's on your site. Know who's ready to buy.**

Leadpipe is a B2B data platform that turns anonymous website traffic into identified contacts and finds people actively researching topics that matter to your business.

## Products

### Identification

Drop a pixel on your website. We identify your anonymous visitors and deliver full contact profiles — name, email, company, job title, phone, LinkedIn — in real time via webhooks or integrations.

- **Real-time identification** — visitors resolved as they browse
- **60+ data fields** per person — business email, direct phone, seniority, company details, demographics
- **Webhook delivery** — push to your CRM, Slack, or any HTTP endpoint instantly
- **Native integrations** — Slack, HubSpot, Salesforce, 1000+ apps via Pipedream
- **Suppression rules** — exclude known customers, competitors, or specific industries
- **Credit-based pricing** — pay only for unique identifications

### Intent

Find people actively researching any of 20,000+ B2B and B2C topics — before they visit your site. Build audiences filtered by company size, seniority, industry, and 16 other ICP criteria. Export full contact profiles for outbound.

- **20,000+ intent topics** — from "Salesforce CRM" to "Cloud Migration" to "Nike"
- **Daily-refreshed signals** — scored 1-100 based on recency, frequency, and behavioral deviation
- **ICP filtering** — seniority, industry, company size, department, revenue, geography, and more
- **Audience builder** — save, activate, and auto-refresh audiences daily
- **Instant preview** — see audience size and sample contacts before committing
- **CSV export** — download full audiences with 58 contact fields
- **Website analysis** — paste a URL, get matched topics automatically

## API

RESTful API with OpenAPI spec and interactive documentation.

```
Base URL: https://api.leadpipe.ai/v1
Auth:     X-API-Key: sk_...
Docs:     https://api.leadpipe.ai/docs/intent-api
```

### Quick Example

```bash
# Find people interested in "Cloud Computing" with VP+ seniority
curl -X POST https://api.leadpipe.ai/v1/intent/audiences/preview \
  -H "X-API-Key: sk_your_key" \
  -H "Content-Type: application/json" \
  -d '{
    "topicIds": [117805],
    "minScore": 70,
    "filters": {
      "seniority": ["vp", "cxo"],
      "hasBusinessEmail": true
    }
  }'
```

### Endpoints

| Section | Endpoints | What it does |
|---------|-----------|-------------|
| **Topic Discovery** | 7 endpoints | Browse 20K+ topics, trends, movers, search, website analysis |
| **Audience Builder** | 8 endpoints | Create audiences, configure filters, preview, ad-hoc query |
| **Audience Results** | 5 endpoints | Get profiles, check status, run history, fill rates, CSV export |
| **Identification** | 4 endpoints | Manage pixels, view account, retrieve visitor data |

## SDK

```bash
npm install @leadpipe/sdk
```

```typescript
import { Leadpipe } from '@leadpipe/sdk';

const lp = new Leadpipe('sk_your_key');

// Discover topics
const topics = await lp.topics.list({ type: 'b2b', q: 'CRM' });

// Preview an audience
const preview = await lp.audiences.preview({
  topicIds: [117805],
  minScore: 70,
  filters: { seniority: ['director', 'vp', 'cxo'] }
});
console.log(`${preview.totalCount} people found`);

// Create and activate
const audience = await lp.audiences.create({
  name: 'Enterprise VPs - Cloud',
  config: { topicIds: [117805], minScore: 70, filters: { seniority: ['vp', 'cxo'] } }
});
await lp.audiences.update(audience.id, { status: 'active' });

// Wait for results and iterate pages
await lp.audiences.waitForReady(audience.id);
for await (const page of lp.audiences.resultPages(audience.id)) {
  for (const person of page.data) {
    console.log(`${person.firstName} ${person.lastName} - ${person.company}`);
  }
}
```

## How It Works

```
1. Pick topics        Browse 20K+ topics or analyze your website
                      ↓
2. Set filters        Seniority, industry, company size, geography...
                      ↓
3. Preview            See audience size + sample contacts instantly
                      ↓
4. Activate           Audience materializes in background (~10-60s)
                      ↓
5. Get results        Full contact profiles, paginated, exportable
                      ↓
6. Daily refresh      Audiences update automatically with new signals
```

## Data Coverage

| Category | Fields |
|----------|--------|
| **Contact** | Email, business email, personal email, phone, direct number, mobile, LinkedIn |
| **Professional** | Job title, seniority, department, job functions, headline |
| **Company** | Name, domain, industry, size, revenue, country, LinkedIn, NAICS, SIC |
| **Personal** | City, state, zip, country, age range, gender, income range |
| **Intent** | Score (1-100), matched topics, topic overlap count |
| **Hashes** | SHA256, SHA1, MD5 for identity matching |

**58 fields per person. 4.4 billion profiles. Updated daily.**

## Intent Scoring

Every person gets a score from 1-100 based on behavioral signals:

| Score | Level | Signal |
|-------|-------|--------|
| 90-100 | High Intent | Consistent, recent, frequent engagement — ready to buy |
| 70-89 | Medium Intent | Above-baseline activity — actively interested |
| 60-69 | Low Intent | Slight interest above baseline — early stage |
| <60 | Interest Only | General topic affinity, no purchase signal |

Scores refresh daily. Set your minimum threshold per audience to control quality vs volume.

## Integrations

| Platform | Type | How |
|----------|------|-----|
| **Slack** | Native | Real-time alerts when visitors are identified |
| **HubSpot** | Via Pipedream | Push contacts to lists and workflows |
| **Salesforce** | Via Pipedream | Create leads and contacts automatically |
| **Webhooks** | Native | POST to any URL on every identification |
| **1000+ apps** | Via Pipedream | Connect to any app in the Pipedream catalog |
| **CSV Export** | Native | Download full audiences as CSV files |
| **API** | REST | Build custom integrations with the full API |

## Links

- [API Documentation](https://api.leadpipe.ai/docs/intent-api)
- [Website](https://leadpipe.ai)

## License

Copyright 2026 Leadpipe. All rights reserved.
