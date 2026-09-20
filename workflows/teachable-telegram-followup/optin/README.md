# Opt-in

[`workflow.json`](workflow.json)

Telegram `/start` → ask for the purchase email → match `email_norm` against the members table → store `telegram_chat_id` → welcome DM with live Teachable progress and a channel invite.

Parent: [Teachable course follow-up](../)

```mermaid
flowchart TB
  tg[On Telegram Message] --> waitNode[Wait]
  waitNode --> cfg[Config]
  cfg --> classify[Classify Message]
  classify --> load[Load All Members]
  load --> resolve[Resolve Bind]
  resolve --> route[Route Outcome]
  route -->|bind_ok| bind[Bind Member Row]
  route -->|already_bound| alreadyBound[Send Already Bound DM]
  route -->|ask_email| askEmail[Send Ask Email DM]
  route -->|not_found| unmatched[Record Unmatched Attempt]
  route -->|already_taken| taken[Send Already Taken DM]
  route -->|too_many_attempts| tooMany[Send Too Many Attempts DM]
  route -->|help| helpDm[Send Help DM]
  route -->|ignored| ignored[Ignored No Reply]
  unmatched --> notFound[Send Not Found DM]
  bind --> progress[Fetch Live Progress]
  progress --> welcome[Compose Welcome]
  welcome --> sendWelcome[Send Welcome DM]
  sendWelcome --> record[Record Welcome Result]
  record --> stamp[Stamp Welcome Result]
```
