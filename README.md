# Resolver Co UK Scraper

Extract structured data from [resolver.co.uk](https://resolver.co.uk) — resolver.co.uk — UK company complaint profiles with rating, sectors and common complaint types, plus the latest customer reviews. Search by company name or paste resolver.co.uk search URLs, and track new or changed profiles on scheduled runs.

**[Resolver Co UK Scraper on Apify →](https://apify.com/blackfalcondata/resolver-co-uk-scraper?fpr=1h3gvi)**

---

## 🚀 How to use this actor

> ### 💚 $5 free Apify credits — every month
> No credit card required. No commitment. Cancel anytime.

### 👉 [Sign up free on Apify →](https://console.apify.com/sign-up?fpr=1h3gvi)

1. **Click sign up** — pick GitHub, Google, or email; takes ~30 seconds
2. **Open this actor** — input is pre-filled with a working example
3. **Click Start** — export results as JSON, CSV, or Excel

Your **$5 monthly platform credit** is enough to run this actor right away — and again every month — scraping typically several hundred to several thousand results per run, depending on your input.

## Key features

**Search with filters** — Search by keyword. Filter by 📝 description format, and more.

**Detail enrichment** — Fetch employer profiles, structured metadata for each record.

**Incremental mode** — Only get new or changed records since your last run. Content hash per record — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how records evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Result cap** — Stop after N records (up to 1.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every record returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured records from resolver.co.uk on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor records, track trends, and analyze market dynamics with structured, deduplicated data from resolver.co.uk.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**AI / LLM training data**
Structured JSON per record is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "bank",
  "maxResults": 25,
  "includeDetails": true,
  "compact": false,
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "notifyOnlyChanges": false,
  "notificationLimit": 5,
  "descriptionFormat": "all"
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | — | Company name or keyword to look up (e.g. "bank", "Lloyds", "broadband"). Paste a JSON array like ["bank","energy"] to run several searches in one go. Each search returns up to 25 matching companies. Required if no start URLs are provided. |
| `maxResults` | integer | `100` | Maximum total companies across the whole run (0 = unlimited). Each search term or start URL returns at most 25 companies (no pagination), so the total is also capped by 25 x the number of searches. |
| `startUrls` | array | `[]` | Paste company-search URLs from resolver.co.uk (e.g. https://www.resolver.co.uk/home/search?term=bank). Each URL becomes its own search; results are merged and deduped across all URLs. When provided, start URLs REPLACE the search field (unsupported URLs are skipped, and if none is supported the run stops with an input error). |
| `includeDetails` | boolean | `true` | Fetch each company's profile page: sectors, rating, common complaint types, review counts and the latest reviews. Turn off for a fast name/URL-only list. |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against previous run state and return only new or changed company profiles (a new review, a changed rating, a new sector). stateKey is optional — defaults to a value derived from your search inputs so different search sets never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked search universe. Leave empty to auto-generate from search inputs. |
| `emitUnchanged` | boolean | `false` | When incremental, also emit company profiles that haven't changed since the last run (marked UNCHANGED). |
| `emitExpired` | boolean | `false` | When incremental, also emit previously seen companies that are no longer found (marked EXPIRED). Scans the complete result set to decide. |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED companies. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of companies included in each notification message (1–20). |
| `telegramToken` | string | — | Telegram bot token (from @BotFather). Required for Telegram notifications. |
| `telegramChatId` | string | — | Telegram chat or channel ID (e.g. "-100123456789"). Required when telegramToken is set. |
| `discordWebhookUrl` | string | — | Discord incoming webhook URL. Server Settings → Integrations → Webhooks → New Webhook. |
| `slackWebhookUrl` | string | — | Slack incoming webhook URL. api.slack.com/messaging/webhooks. |
| `whatsappPhoneNumberId` | string | — | Your WhatsApp Business phone-number ID (numeric, from Meta dashboard). Required when whatsappAccessToken is set. |
| `whatsappAccessToken` | string | — | WhatsApp Cloud API permanent access token (System User token from Meta Business). Recipient must have messaged the business number within the last 24h (service-conversation window — free since Nov 2024). |
| `whatsappTo` | string | — | Recipient phone in E.164 format without + (e.g. "447700900123"). Recipient must have messaged your business number within last 24h. |
| `webhookUrl` | string | — | Receives a JSON POST with {metadata, items} after each run. Universal escape hatch for n8n / Make / Zapier / custom backends. |
| `webhookHeaders` | object | — | Optional JSON object of custom headers (e.g. {"Authorization":"Bearer ..."}). |
| `appConnector` | string | — | Optional. Pick a connected app under Settings → API & Integrations to receive your results. Best-effort across MCP connectors as Apify expands its catalog. |
| `mcpIssueTeam` | string | — | Only when the connected app is an issue tracker: the team (name or ID) the summary issue is created under, if that app requires one. |
| `descriptionFormat` | enum | `"all"` | Choose which representation of the listing description to include. `all` keeps every variant; the others keep only the selected one. |
| `skipReposts` | boolean | `false` | In incremental mode, skip listings that appear to be reposts of a previously-seen expired listing with matching details. |

---

## Output fields

Every record returns the same 25-field schema. Missing values are `null` — never omitted.

- `listingId`
- `companyId`
- `companyName`
- `companyUrl`
- `portalUrl`
- `slug`
- `website`
- `activeOnResolver`
- `notOnResolver`
- `sectors`
- `rating`
- `commonComplaintTypes`
- `aboutText`
- `reviewPages`
- `reviewsPerPage`
- `latestReviews`
- `contactDetailsUrl`
- `logoUrl`
- `searchQuery`
- `contentQuality`
- `detailFetched`
- `scrapedAt`
- `source`
- `contentHash`
- `changeType`

---

## Sample output

One object per record. Here is a real example from a production run:

```json
{
  "listingId": "25c70570c939446b356419199c3be752f629c288a8d8d7f79fdb9cc53689e188",
  "companyId": "700",
  "companyName": "Lloyds Bank",
  "companyUrl": "https://www.resolver.co.uk/companies/lloyds-bank-complaints",
  "portalUrl": "https://www.resolver.co.uk/companies/lloyds-bank-complaints",
  "slug": "lloyds-bank-complaints",
  "website": null,
  "activeOnResolver": true,
  "notOnResolver": false,
  "sectors": [
    "Insurance",
    "Finance"
  ],
  "rating": 3,
  "commonComplaintTypes": [
    "Card limit issue",
    "Other (none of the above)"
  ]
}
```

*Truncated — full records contain 25 fields. See Output fields for the complete schema.*

**[Try Resolver Co UK Scraper now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/resolver-co-uk-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.01 |
| Result | $0.002 |

See the [actor on Apify](https://apify.com/blackfalcondata/resolver-co-uk-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape resolver.co.uk?**
Use this actor on Apify to extract structured data from resolver.co.uk. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get resolver.co.uk data as JSON, CSV, or Excel?**
The actor writes each record to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape resolver.co.uk?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check resolver.co.uk's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each record gets a content hash. On subsequent runs, only new or changed records are emitted — saving time, compute, and storage. Expired records can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

- [Djinni Scraper](https://apify.com/blackfalcondata/djinni-scraper?fpr=1h3gvi) — Ukraine & CEE tech jobs with salary and English level
- [Work.ua Scraper](https://apify.com/blackfalcondata/work-ua-scraper?fpr=1h3gvi) — Ukraine's work.ua job listings
- [NoFluffJobs Scraper](https://apify.com/blackfalcondata/nofluffjobs-scraper?fpr=1h3gvi) — Poland & CEE IT jobs with transparent salary ranges
- [OLX Jobs Scraper](https://apify.com/blackfalcondata/olx-jobs-scraper?fpr=1h3gvi) — OLX job listings across 7 CEE countries
- [Prace.cz Scraper](https://apify.com/blackfalcondata/prace-scraper?fpr=1h3gvi) — Czech Republic's prace.cz job board
- [Profession.hu Scraper](https://apify.com/blackfalcondata/profession-scraper?fpr=1h3gvi) — Hungary's profession.hu job board

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [Resolver Co UK Scraper](https://apify.com/blackfalcondata/resolver-co-uk-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 11*
