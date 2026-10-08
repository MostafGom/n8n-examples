# n8n examples

Portfolio-safe n8n workflow JSON. Import these into any n8n instance to see how the automations work. They are **not** live production copies.

Every file here is stripped of API tokens, credential objects, pinned execution data, and client or agency names.

## Import

1. Open the example folder and import its `workflow.json` in n8n (**… → Import from File**).
2. Attach **your own** credentials. Import will not include secrets.
3. Do not turn the workflow active until those credentials are yours.

## Examples

| Example | What it does |
| --- | --- |
| [Lead brief (Instagram + site)](workflows/lead-brief-ig-site/) | Sub-workflow: scrape IG + site, then an AI agent writes a cited lead brief |
| [Teachable course follow-up (Telegram)](workflows/teachable-telegram-followup/) | Opt-in bot binds a student email, daily job DMs progress and posts a channel scoreboard |
| [Chatwoot WhatsApp](workflows/chatwoot-whatsapp/) | Look up a contact by phone and send a WhatsApp template on their conversation |

## Adding another example

Create a folder under `workflows/` with `README.md` (short description + Mermaid) and `workflow.json`. If several flows share data tables, keep them in one parent folder.

Sanitize before saving:

- Strip `credentials`, tokens, `pinData`, webhook IDs, instance IDs
- Rename nodes, prompts, URLs, and comments that identify a client or agency
- Keep the automation logic intact
