---
name: daily-attorney-brief
description: Assemble this attorney's prioritised morning briefing from LawLink — today's calendar, deadlines, overdue and due tasks, and matters that moved. Use when the user asks for their daily brief, morning briefing, "what's on today", "what do I need to know", or when a scheduled task asks for the LawLink daily attorney brief.
---

# Daily Attorney Brief

Produce the brief and nothing else. No preamble, no narration of the steps,
no closing offer of further help. The reader is an attorney at 8am, often on
a phone, between other things.

## Step 1 — Establish who this is for

Call `firm_users` with `action="list"` and identify the user running this
brief. Note their user id on every platform where they appear; it is needed
to separate their work from the rest of the firm's.

If no user can be identified on a platform, record it under **Data notes**
and continue. Never guess at a similar name, and never fall back to
unfiltered firm-wide results — showing one attorney another's caseload is
worse than showing nothing.

## Step 2 — Gather

Omit the `platform` argument on every call so each covers whatever the
attorney has connected.

| What | Call |
|------|------|
| Today's calendar | `events` `action="list"`, `date_from`=today, `date_to`=today |
| Due in 7 days | `tasks` `action="list"`, `assignee_id`=theirs, `complete=false`, `due_at_from`=today, `due_at_to`=today+7 |
| Already overdue | `tasks` `action="list"`, `assignee_id`=theirs, `complete=false`, `due_at_to`=yesterday |
| Matters that moved | `matters` `action="list"`, `responsible_attorney_id`=theirs, `updated_since`=yesterday, `order="updated_at(desc)"`, `limit=25` |

If a call errors, returns a partial result, or reports that a platform
cannot apply a filter, carry on and record it under **Data notes**. A
section built on missing data must never be presented as an empty one.

## Step 3 — Rank

Merge everything into one ordered list. The ranking is the point — the
attorney wants somewhere to start, not four lists to reconcile.

1. Court appearances and hearings today
2. Court or statutory deadlines within 7 days
3. Overdue tasks, oldest first
4. Tasks due today
5. Matters that changed and appear to need a response
6. Anything else due this week

Within a tier: by time of day, then by proximity of the deadline.

## Step 4 — Write it

```
## Start here
One sentence: the single most important thing today, and why.

## Today
time — what — which matter — where or with whom
"No scheduled events today." if genuinely empty.

## Needs attention
The ranked list, at most 10 items, one line each: what, which matter,
when it is due, why it ranks there. Then "+N more due this week."

## Matters that moved
Up to 5. One line each: matter, what changed, whether it needs them.

## Data notes
Only when something was incomplete. Name the platform and what failed or
was truncated. Omit the whole section when everything returned cleanly.
```

## Rules

- Never invent a deadline, a time, or a matter name. Everything comes from
  tool results.
- Do not pad. After a quiet night a four-line brief is the correct output.
- **An empty result and a failed call look identical to the reader.**
  "No tasks due" is a finding; "Clio did not respond" is a warning. Say
  which.
- If more than one organization appears, keep them separate and label them.
- If the brief looks unexpectedly thin, consider `connections`
  `action="check_connection"` — a platform holding stale credentials returns
  nothing and reads exactly like a free morning.
