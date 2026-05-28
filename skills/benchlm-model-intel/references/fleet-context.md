# Fleet Context for Model Recommendations

This file provides current agent roster and model assignments for generating fleet recommendations.
Update when models change.

## Agent Roster

| Agent | Role | Current Model | Notes |
|-------|------|---------------|-------|
| Heidi | Home Chief of Staff | claude-sonnet-4-6 | Tool use + instruction following dominant |
| Sage | Health Coach | google/gemini-3.5-flash | Morning briefs, health data synthesis |
| Finn | Financier | google/gemini-3.1-pro | Budgeting, expense tracking, financial summaries |
| Kristen | Work CoS | claude-sonnet-4-6 | Work ops, scheduling, drafting |
| Norah | Executive Coach | claude-sonnet-4-6 | Dormant — not yet reactivated |
| Anne | Horse Training Assistant | claude-sonnet-4-6 | Reasoning + knowledge retrieval |
| Frank | Ops Analyst | claude-sonnet-4-6 | This agent |

## Evaluation Criteria by Agent

- **Heidi**: Tool use quality is the primary constraint (email, calendar, reminders, Sheets). Instruction following matters. Cost secondary.
- **Sage**: Reasoning and synthesis from structured health data (Oura, Apple Health, Natural Cycles). Output quality matters; rate limits matter (free-tier Gemini: 15 RPM / 1,500 req/day).
- **Finn**: Factuality + instruction following for financial analysis. Cheap preferred — dormant most of the time.
- **Kristen**: Instruction following, drafting, structured output. Active but infrequent.
- **Anne**: Knowledge retrieval + reasoning about horse training principles. Occasional deep-reasoning tasks.
- **Frank**: Synthesis + cost reporting. Sonnet justified by digest complexity.

## Recommendation Guidance

- Flag a switch only if the new model is meaningfully better on the relevant dimension AND cheaper or within 20% cost
- Do not recommend switches for dormant agents (Norah, Finn) unless the improvement is significant
- Gemini free-tier rate limits are a real constraint for Sage — factor in if recommending a higher-tier Gemini model
- Haiku is the floor for lightweight cron tasks; don't recommend going below it
