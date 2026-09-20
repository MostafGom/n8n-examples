# Daily

[`workflow.json`](workflow.json)

Scheduled job: fetch each member's Teachable progress one-by-one, update the roster, DM if due, then post a batch scoreboard to a Telegram channel.

Parent: [Teachable course follow-up](../)

```mermaid
flowchart TB
  subgraph ingest [Ingest]
    daily[Daily 19:00] --> cfg[Config]
    cfg --> batches[Load Batches]
    batches --> members[Load Members]
    members --> select[Select Batch Members]
  end
  select --> loop[Loop Over Members]
  loop -->|each member| progress[Get Course Progress]
  progress --> state[Compute Member State]
  state --> updateRow[Update Member Progress]
  updateRow --> sendQ["Should Send DM?"]
  sendQ -->|yes| dm[Send Progress DM]
  sendQ -->|no| throttle[Throttle 1s]
  dm --> record[Record DM Result]
  record --> okQ["Send OK?"]
  okQ -->|yes| stamp[Stamp DM Sent]
  okQ -->|no| fail[Mark DM Failure]
  stamp --> throttle
  fail --> throttle
  throttle --> loop
  loop -->|done| reload[Reload Members For Totals]
  reload --> scoreboard[Build Channel Post]
  scoreboard --> dryQ["Channel Dry Run?"]
  dryQ -->|yes| dryOnly[Channel Post Dry Run Only]
  dryQ -->|no| post[Post To Channel]
  post --> counters[Update Batch Counters]
```
