# 📊 Frank — Agent Operations Analyst

Frank is a production AI agent that monitors a multi-agent household system for efficiency, cost, and health. He tracks session costs, flags context bloat, detects waste patterns, and delivers daily digests and weekly scorecards — so the operator always knows which agents are healthy, which need attention, and what to change next.

Frank has been running daily since April 2026.

---

## Architecture

Frank is the ops layer in a multi-agent household system:

```
┌─────────────────────────────────────────────────────────┐
│                     Multi-Agent System                  │
├──────────────┬──────────────────┬───────────────────────┤
│    Frank     │      Heidi       │        Sage           │
│  (Ops Agent) │  (Chief of Staff)│  (Health Agent)       │
├──────────────┼──────────────────┼───────────────────────┤
│ Daily/weekly │ Daily operations │ Weekly health notes   │
│ cost digests │ Morning briefs   │ for meal planning     │
│              │ Email triage     │                       │
│ Session      │ Meal planning    │                       │
│ resets for   │ CRM & reminders  │                       │
│ all agents   │ Farm ops         │                       │
│ at 3 AM      │ Calendar mgmt    │                       │
│              │ Family comms     │                       │
│ Weekly model │                  │                       │
│ intelligence │                  │                       │
└──────────────┴──────────────────┴───────────────────────┘
         │              ▲                    │
         └────────────► │ ◄──────────────────┘
                  Shared state via
                  workspace files + daily snapshots
```

**Frank** runs nightly session resets for every agent at 3 AM. Without active session management, a single continuous session can hit $150+ in a week as growing context drives up per-turn costs. Frank keeps the fleet lean and costs predictable.

**Shared state** lives in workspace files and daily snapshots — not in session history. Session history is treated as temporary; files are treated as truth.

---

## What Frank Does

### Daily Digest (9 AM, Tue–Sun)
- Runs `slim_sessions.py` to extract cost/token/status data across all agents
- Compares against prior-day snapshot for deltas
- Reports: most expensive agents, biggest movers, context bloat signals, loop/retry flags, model right-sizing candidates, monthly cost forecast
- Delivers via Telegram — 3 messages maximum, signal over noise

### Weekly Scorecard (Monday 9 AM)
- Covers the prior 7 days of snapshot data
- Reports: efficiency improvers, agents getting more expensive (and why), mature/stable agents, onboarding overhead, top workflow changes, session health dashboard
- Sole Monday report — no separate morning digest that day

### Weekly Model Intelligence (Monday 9:15 AM)
- Fetches BenchLM leaderboard and use-case rankings
- Reports: best-in-class by use case, fleet recommendations, operator's personal toolkit
- Flags new model releases since last week
- Helps the operator know which LLM to use for any given task

### Fleet Infrastructure
- Nightly session resets for all agents (3 AM, Python file-deletion method)
- Nightly Google OAuth token refresh for agents with API integrations
- Delivery of reset confirmation messages to each agent on startup

---

## Design Principles

**Trend over snapshot.** A single data point is noise. Direction over time is signal. Frank looks for patterns before raising flags.

**Quality before cheapness.** The goal is appropriate cost for the work being done — not the lowest possible cost. Degrading agent quality to save pennies is not a win.

**Recommendation over alarm.** Frank surfaces what to do next, not just what went wrong. Every flag comes with a likely cause and a concrete next step.

**Diagnose before prescribing.** Solutions proposed before the root cause is understood are guesses. Frank identifies what's actually broken before suggesting how to fix it.

**Simple over elaborate.** If the right answer is two lines, say two lines. Infrastructure built to solve a behavior problem is waste.

**Intellectual honesty.** If the data calls for a conclusion the operator won't love, say it clearly and early. Softening it into irrelevance is noise, not kindness.

---

## File Structure

```
frank/
├── AGENTS.md                  # Workspace rules, memory conventions (stub — security details private)
├── SOUL.md                    # Identity, values, operating principles (stub — operational details private)
├── IDENTITY.md                # Name, role, tone, emoji
├── HEARTBEAT.md               # Trigger schedule — what runs when
├── TOOLS.md                   # Scripts, paths, endpoints (stub — values private)
├── MEMORY.md                  # Curated long-term memory (stub — private in production)
├── DAILY_REPORT_TEMPLATE.md   # 8-question structure for daily digests
├── WEEKLY_REPORT_TEMPLATE.md  # 7-question structure for weekly scorecards
└── SNAPSHOT_SCHEMA.md         # Schema for daily cost/token snapshots
```

---

## Tech Stack

- **Agent framework:** Claude (Anthropic) via [OpenClaw](https://openclaw.ai) 🦞
- **Session management:** Self-managed — Frank resets his own session nightly and manages resets for all other agents
- **Data source:** `sessions.json` files per agent, processed via `slim_sessions.py`
- **Snapshot storage:** Local markdown + JSON files in `memory/snapshots/`
- **Model intelligence:** BenchLM (https://benchlm.ai) — fetched weekly
- **Primary channel:** Telegram
- **Language:** Python 3 (scripts), Markdown (workspace files)

---

## What's Not In This Repo

- `MEMORY.md` — operational fleet state, cost history, lessons learned. Stub only.
- `TOOLS.md` — Telegram target IDs, spend ceilings, session paths. Stub only.
- `memory/` — raw daily notes and cost snapshots. Operational logs, not for public consumption.
- `scripts/` — internal automation scripts. Implementation detail, not part of the template.
- `USER.md` — private context about the operator.

The architecture, reporting structure, and operational patterns are all here. The operational data isn't.

---

## Status (May 2026)

Frank is in active daily use. Daily digests, weekly scorecards, and model intelligence reports are running. All 7 agents in the fleet have nightly session resets managed by Frank.

---

*Built with Claude (Anthropic) + [OpenClaw](https://openclaw.ai) 🦞. Runs on data, pattern recognition, and a healthy skepticism of single data points.*
