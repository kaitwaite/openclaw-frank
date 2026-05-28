# Frank — Snapshot Schema

Snapshots are saved to `memory/snapshots/YYYY-MM-DD-{morning|evening}.json` at each report run.
The weekly scorecard reads the last 7 days of snapshots to compute derived fields.

---

## Per-agent record (saved fields)

```json
{
  "date": "2026-04-20",
  "period": "morning",
  "agent_name": "heidi",
  "session_count": 2,
  "successful_task_count": null,
  "estimated_cost": 154.092,
  "avg_input_tokens": null,
  "avg_output_tokens": null,
  "max_context_seen": 200000,
  "retry_count": null,
  "loop_flag_count": null,
  "dominant_model": "claude-sonnet-4-6",
  "quota_warning": false,
  "notes": "Session age 6 days. Context bloat flagged."
}
```

## Field notes

- `successful_task_count` — not yet derivable; will populate once task outcome detection is built
- `avg_input_tokens` / `avg_output_tokens` — requires per-message token breakdown; not in sessions.json today; future enhancement
- `retry_count` / `loop_flag_count` — requires transcript parser; in progress
- `quota_warning` — boolean; set true when cost velocity exceeds threshold (needs quota ceiling from [OPERATOR])
- `notes` — free text; used for flags, anomalies, context on derived values

## Derived fields (computed at report time, not stored)

| Field | Formula |
|---|---|
| `cost_per_successful_task` | `estimated_cost / successful_task_count` (when available) |
| `avg_tokens_per_session` | `(avg_input_tokens + avg_output_tokens) * session_count / session_count` |
| `week_over_week_cost_change` | `this_week_cost - last_week_cost` |
| `week_over_week_cost_per_task_change` | delta in `cost_per_successful_task` |
| `context_growth_rate` | `(max_context_seen_today - max_context_seen_7d_ago) / 7` |
| `retry_rate` | `retry_count / total_turns` |

## What's live today vs. future

| Field | Status |
|---|---|
| date, period, agent_name | ✅ Live |
| session_count | ✅ Live |
| estimated_cost | ✅ Live |
| max_context_seen | ✅ Live |
| dominant_model | ✅ Live |
| quota_warning | ✅ Live (threshold TBD — needs [OPERATOR]'s quota ceiling) |
| notes | ✅ Live |
| avg_input_tokens / avg_output_tokens | 🔜 Needs transcript parser |
| successful_task_count | 🔜 Needs task outcome detection |
| retry_count / loop_flag_count | 🔜 Needs transcript parser |
| cost_per_successful_task | 🔜 Blocked on above |
| avg_tokens_per_session | 🔜 Blocked on above |
| week_over_week_* | ✅ Live after 7 days of snapshots |
| context_growth_rate | ✅ Live after 7 days of snapshots |
| retry_rate | 🔜 Blocked on transcript parser |
