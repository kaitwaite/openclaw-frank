# HEARTBEAT.md — Frank's Trigger Checklist

_This file is the WHAT and WHEN. For HOW each workflow runs, see DAILY_REPORT_TEMPLATE.md and WEEKLY_REPORT_TEMPLATE.md._

---

## Scheduled Crons

| Job | Schedule | Notes |
|-----|----------|-------|
| Morning Digest | 9:00 AM EDT, Tue–Sun | Isolated session. Skips Monday — scorecard covers that day. |
| Weekly Scorecard | 9:00 AM EDT, Monday | Isolated session. Sole Monday report. |
| Model Intelligence | 9:15 AM EDT, Monday | Isolated session. Runs after scorecard. Fetches BenchLM, delivers use-case rankings + fleet recommendations. |
| Evening Digest | 6:00 PM EDT, daily | **Disabled.** Re-enable if daily delta tracking becomes useful again. |

---

## Morning Digest — What to Include

1. Run `scripts/slim_sessions.py` first — use output as sole data source
2. Load most recent snapshot from `memory/snapshots/` for delta calculations
3. Follow structure in `DAILY_REPORT_TEMPLATE.md`
4. Section 6 requires monthly forecast — sum snapshot costs this month, divide by days elapsed, multiply by days in month. Flag if projection > $160.
5. Save snapshot to `memory/snapshots/YYYY-MM-DD-morning.json`
6. Deliver via Telegram (see TOOLS.md). Hard limit: 3 messages maximum.

---

## Weekly Scorecard — What to Include

1. Run `scripts/slim_sessions.py` first
2. Load last 7 days of snapshots from `memory/snapshots/`
3. Follow structure in `WEEKLY_REPORT_TEMPLATE.md`
4. Save snapshot to `memory/snapshots/YYYY-MM-DD-morning.json`
5. Deliver via Telegram (see TOOLS.md)

---

## Memory Maintenance

Every few days during a quiet session:
- Review recent `memory/YYYY-MM-DD.md` daily notes
- Update `MEMORY.md` with anything worth keeping long-term
- Remove outdated entries from MEMORY.md

---

## When to Reach Out Proactively

- Any agent shows consecutive cron failures
- Monthly cost projection clears $160
- A model change or infrastructure issue needs Kate's attention

## When to Stay Quiet

- Digests and scorecards are the primary output — no additional unprompted messages
- Routine fluctuations that don't cross any threshold
