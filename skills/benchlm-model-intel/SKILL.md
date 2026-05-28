---
name: benchlm-model-intel
description: Fetch the weekly BenchLM model leaderboard and deliver a structured intelligence report covering top models by use case, fleet upgrade recommendations, and personal toolkit suggestions. Use when: (1) asked to run a model intelligence report, (2) checking which models are best this week, (3) generating fleet model recommendations, (4) running the weekly BenchLM cron. Designed for ops-analyst agents monitoring a multi-agent fleet but usable standalone.
---

# BenchLM Model Intelligence

Fetches four BenchLM pages, synthesizes the data, and delivers a concise structured report.

## Sources

Fetch all four in one pass using `web_fetch`:

| Page | URL |
|------|-----|
| Main leaderboard | `https://benchlm.ai` |
| Tool use | `https://benchlm.ai/best/tool-use` |
| Factuality | `https://benchlm.ai/best/factuality` |
| Web research | `https://benchlm.ai/best/web-research` |

Coding leaders are readable from the main leaderboard — no separate page needed.

## Report Format

Deliver as a single message (Telegram or channel of choice). Use this structure:

```
🧠 Weekly Model Intelligence — [Date]

🏆 Overall #1: [model, provider, score, price input/output]

Best by use case:
- 🤖 Tool use / agents: [model + score]
- 🔍 Web research: [model + score]
- ✅ Factuality: [model + score]
- 💻 Coding (main leaderboard): [model + score]
- ⚡ Best value (cost-efficient): [model + price]

Fleet recommendations:
- [Agent name] ([role, current model]): [keep / switch to X — one-line rationale]
- ...

[OPERATOR]'s personal toolkit:
- Hard reasoning / complex problems: [model]
- Writing / editing: [model]
- Research: [model]
- Coding: [model]
- Quick / cheap: [model]

🆕 New this week: [models appearing new vs. last week — omit if none]
```

One message. No narration. No preamble.

## Fleet Context

See `references/fleet-context.md` for current agent roster, models, and roles — load this before generating fleet recommendations.

## Operational Notes

- Run on Haiku — the task is fetch + synthesis, not reasoning-heavy
- If a page fetch fails, note it in the report and continue with available data
- "New this week" requires comparing against prior report or memory; omit if no baseline available
- Price format: `$X.XX input / $X.XX output per 1M tokens`
