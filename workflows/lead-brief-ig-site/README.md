# Lead brief (Instagram + site)

[`workflow.json`](workflow.json)

Sub-workflow: a parent flow passes `instagram`, `website`, and `name`. This flow scrapes public Instagram and crawls the site (Apify), merges evidence, then an AI agent writes a cited lead brief (observed vs inferred vs unknown).

Call it from another workflow with an **Execute Sub-workflow** node. It is not a standalone webhook.

```mermaid
flowchart TB
  parent[When called by parent] --> norm[Normalize and validate]
  norm --> hasIg["Has IG?"]
  norm --> hasSite["Has site?"]
  hasIg -->|yes| igScrape[Apify IG scrape]
  hasIg -->|no| igAbsent[IG absent]
  igScrape --> igMetrics[Compute IG metrics]
  igMetrics --> merge[Merge sources]
  igAbsent --> merge
  hasSite -->|yes| siteCrawl[Apify site crawl]
  hasSite -->|no| siteAbsent[Site absent]
  siteCrawl --> sitePages[Collect site pages]
  sitePages --> merge
  siteAbsent --> merge
  merge --> evidence[Assemble evidence]
  evidence --> anySrc["Any source?"]
  anySrc -->|yes| agent[AI Agent brief]
  anySrc -->|no| noData[Return no-data]
  anthropic[Anthropic Chat Model] -.-> agent
  fetchPage[Fetch web page] -.-> agent
  webSearch[Web search] -.-> agent
  agent --> brief[Return brief]
```

## After import

Attach:

- **HTTP Query Auth** (Apify API token as the `token` query param) on `Apify — IG scrape`, `Apify — Site crawl`, `Fetch web page`, and `Web search`
- **Anthropic** on `Anthropic Chat Model`
