# BR.dk Toy Store Scraper - Prices, Stock & GTIN

Extract structured data from [BR.dk](https://BR.dk) — the BR.dk toy store catalog: price in DKK with list price and discount, in stock status online and in stores, SKU and GTIN barcode, brand, category and specs like piece count. Search by keyword, brand or category, or export all 56,000+ products.

**[BR.dk Toy Store Scraper - Prices, Stock & GTIN on Apify →](https://apify.com/blackfalcondata/br-dk-scraper?fpr=1h3gvi)**

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

**Search with filters** — Search by keyword and location. Filter by description format, and more.

**Detail enrichment** — Fetch full descriptions, structured metadata for each product.

**Incremental mode** — Only get new or changed products since your last run. Content hash per product — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how products evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Description truncation** — Cap description length per ${NOUN} to control output size and cost.

**Result cap** — Stop after N products (up to 100.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every product returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured products from BR.dk on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor products, track trends, and analyze market dynamics with structured, deduplicated data from BR.dk.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**AI / LLM training data**
Structured JSON per product is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "lego",
  "maxResults": 100,
  "includeDetails": false,
  "descriptionMaxLength": 0,
  "compact": false,
  "descriptionFormat": "all",
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "skipReposts": false,
  "notifyOnlyChanges": false,
  "notificationLimit": 5
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | — | Product search keywords, e.g. "lego" or "barbie". Leave empty to browse everything, or combine with a category or brand to browse that slice. Use a JSON array for several searches. |
| `location` | string | — | Optional. Narrow results to one br.dk category, written exactly as the site names it (e.g. "Alt LEGO", "Byggesæt"). |
| `brand` | string | — | Optional. Only products from one brand, written as the store names it (e.g. "LEGO", "Barbie"). Combines with the search term and category. |
| `maxResults` | integer | `100` | Maximum total results (0 = unlimited). |
| `maxPages` | integer | `0` | Optional cap on result pages per search (1000 products per page). 0 = no cap. Raised automatically when Max Results needs more pages. |
| `startUrls` | array | `[]` | Optional list of br.dk product pages, product list pages (links containing "/pl/") or search URLs (www.br.dk/search?q=lego). Each URL becomes its own search or lookup; results are merged and deduped by product. When provided, Start URLs replace the search term and category fields. |
| `includeDetails` | boolean | `false` | Re-fetch each product record for the freshest price and stock. |
| `descriptionMaxLength` | integer | `0` | Truncate product description to N characters. 0 = no truncation. |
| `compact` | boolean | `false` | Core product fields only (for AI-agent/MCP workflows). |
| `descriptionFormat` | enum | `"all"` | Pick a single description representation. `all` keeps every variant; `text` keeps `description` + `descriptionText`, `html` keeps `descriptionHtml`, `markdown` keeps `descriptionMarkdown`. |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against previous run state and return only new or changed products (e.g. price or stock changes). stateKey is optional — defaults to a value derived from your search inputs. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked search universe. Leave empty to auto-generate from search inputs. |
| `skipReposts` | boolean | `false` | When incremental, skip products whose content matches a product that disappeared in a prior run. |
| `emitUnchanged` | boolean | `false` | When Incremental Mode is on, also return products that have not changed since the last run (marked UNCHANGED). Off by default so scheduled runs return only what is new or changed. |
| `emitExpired` | boolean | `false` | When Incremental Mode is on, also track products that disappeared from the store since the last run. Needs a complete scan of the search, so it ignores the Max Results cap. |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED products. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of products included in each notification message (1–20). |
| `telegramToken` | string | — | Telegram bot token (from @BotFather). Required for Telegram notifications. |
| `telegramChatId` | string | — | Telegram chat or channel ID (e.g. "-100123456789"). Required when telegramToken is set. |
| `discordWebhookUrl` | string | — | Discord incoming webhook URL. Server Settings → Integrations → Webhooks → New Webhook. |
| `slackWebhookUrl` | string | — | Slack incoming webhook URL. api.slack.com/messaging/webhooks. |
| `whatsappPhoneNumberId` | string | — | Your WhatsApp Business phone-number ID (numeric, from Meta dashboard). Required when whatsappAccessToken is set. |
| `whatsappAccessToken` | string | — | WhatsApp Cloud API permanent access token (System User token from Meta Business). Recipient must have messaged the business number within the last 24h (service-conversation window — free since Nov 2024). |
| `whatsappTo` | string | — | Recipient phone in E.164 format without + (e.g. "436641234567"). Recipient must have messaged your business number within last 24h. |
| `webhookUrl` | string | — | Receives a JSON POST with {metadata, items} after each run. Universal escape hatch for n8n / Make / Zapier / custom backends. |
| `webhookHeaders` | object | — | Optional JSON object of custom headers (e.g. {"Authorization":"Bearer ..."}). |
| `appConnector` | string | — | Optional. Pick a connected app under Settings → API & Integrations to receive your results. Best-effort across MCP connectors as Apify expands its catalog. |
| `mcpIssueTeam` | string | — | Only when the connected app is an issue tracker: the team (name or ID) the summary issue is created under, if that app requires one. |

---

## Output fields

Every product returns the same 42-field schema. Missing values are `null` — never omitted.

- `productId`
- `sku`
- `gtin`
- `title`
- `brand`
- `url`
- `price`
- `listPrice`
- `currency`
- `discountPercent`
- `onSale`
- `promotionText`
- `availability`
- `inStockOnline`
- `stockCountOnline`
- `storesInStockCount`
- `clickAndCollect`
- `soldOnline`
- `soldInStores`
- `category`
- `categories`
- `breadcrumbs`
- `imageUrl`
- `images`
- `description`
- `summary`
- `specifications`
- `numberOfParts`
- `material`
- `manufacturer`
- `licensee`
- `campaigns`
- `onlineFrom`
- `scrapedAt`
- `portalUrl`
- `listingId`
- `searchQuery`
- `contentQuality`
- `detailFetched`
- `source`
- `contentHash`
- `changeType`

---

## Sample output

One object per product. Here is a real example from a production run:

```json
{
  "productId": "200345541",
  "sku": "11150092-EA",
  "gtin": "5702018068212",
  "title": "LEGO Speed Champions Lynet McQueen 77255",
  "brand": "LEGO",
  "url": "https://www.br.dk/produkter/lego-speed-champions-lynet-mcqueen-77255/200345541/",
  "price": 169,
  "listPrice": 169,
  "currency": "DKK",
  "discountPercent": 0,
  "onSale": false,
  "promotionText": null
}
```

*Truncated — full records contain 42 fields. See Output fields for the complete schema.*

**[Try BR.dk Toy Store Scraper - Prices, Stock & GTIN now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/br-dk-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.009 |
| Result | $0.00099 |

See the [actor on Apify](https://apify.com/blackfalcondata/br-dk-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape BR.dk?**
Use this actor on Apify to extract structured data from BR.dk. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get BR.dk data as JSON, CSV, or Excel?**
The actor writes each product to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape BR.dk?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check BR.dk's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each product gets a content hash. On subsequent runs, only new or changed products are emitted — saving time, compute, and storage. Expired products can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [BR.dk Toy Store Scraper - Prices, Stock & GTIN](https://apify.com/blackfalcondata/br-dk-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 10*
