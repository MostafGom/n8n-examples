# n8n examples

Portfolio-safe n8n workflow JSON. These files are meant to be imported into any n8n instance to show how the automations work. They are **not** live production copies.

Every file here is stripped of API tokens, credential objects, pinned execution data, and client or agency names.

## Import

1. In n8n: **… → Import from File** (or paste the JSON).
2. Attach **your own** credentials. Import will not include secrets.
3. Do not turn the workflow active until those credentials are yours.

## Example: Lead brief (Instagram + site)

[`workflows/lead-brief-ig-site-ai-agent.json`](workflows/lead-brief-ig-site-ai-agent.json)

Sub-workflow: a parent flow passes `instagram`, `website`, and `name`. This flow scrapes public Instagram and crawls the site (Apify), merges evidence, then an AI agent writes a cited lead brief (observed vs inferred vs unknown).

After import, attach:

- **HTTP Query Auth** (Apify API token as the `token` query param) on `Apify — IG scrape`, `Apify — Site crawl`, `Fetch web page`, and `Web search`
- **Anthropic** on `Anthropic Chat Model`

Call it from another workflow with an **Execute Sub-workflow** node. It is not a standalone webhook.

## Example: Teachable course follow-up (Telegram opt-in + daily)

Two connected workflows. Students message the bot, bind their purchase email, then receive progress DMs and an optional channel scoreboard.

- [`workflows/telegram-optin.json`](workflows/telegram-optin.json) — Telegram `/start` → ask for course email → match `email_norm` → store `telegram_chat_id` → welcome DM with live Teachable progress and a channel invite.
- [`workflows/telegram-daily.json`](workflows/telegram-daily.json) — daily job: fetch each member's progress one-by-one, update the roster table, DM if due, then post a batch scoreboard to a channel.

**Data tables:** you do **not** need to upload student rows. Recreate two empty n8n Data Tables from:

- [`datatables/course-telegram-members.schema.json`](datatables/course-telegram-members.schema.json)
- [`datatables/course-telegram-batches.schema.json`](datatables/course-telegram-batches.schema.json)

After import, re-select those tables on every Data Table node. Fill `Config` with your Teachable course id, Telegram channel id / invite link, and attach:

- **Telegram** on the trigger and send nodes
- **HTTP Header Auth** (Teachable API key) on the progress HTTP nodes

Showcase defaults: opt-in `test_mode=true` (no replies unless `test_chat_ids` is set); daily `dry_run=true` (progress still updates, nothing is sent).

## Adding another example

Paste exported JSON (or a local n8n URL) and sanitize before saving:

- Strip `credentials`, tokens, `pinData`, webhook IDs, instance IDs
- Rename nodes, prompts, URLs, and comments that identify a client or agency
- Keep the automation logic intact
