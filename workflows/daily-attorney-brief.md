---
name: daily-attorney-brief
workflow_type: Automated
frequency: Daily
library_order: 1
priority: High
status: Active
practice_areas: All
users: Attorneys; paralegals
---

# Daily Attorney Brief — setup

Automated workflow #1. Creates a personalised morning briefing from the
attorney's connected systems: today's calendar, upcoming deadlines, priority
tasks, and matters that moved.

The logic lives in the **`daily-attorney-brief` skill**, shipped in the
LawLink plugin. This file is only the setup instruction — the attorney sets
it up once and it runs every weekday thereafter.

## Prerequisites

1. **A paid Claude plan** — Pro, Max, Team or Enterprise. Scheduled tasks are
   not available on the free plan.
2. **The LawLink plugin installed**, or provisioned by the firm's admin.
3. **The LawLink connector authorised** — the attorney completes the OAuth
   flow once, and the brief then runs as them.

## Setup — once per attorney

1. In Claude, open **Scheduled** in the left sidebar
2. **New task**
3. Instruction:

   ```
   Run my LawLink daily attorney brief.
   ```

4. Schedule: **weekdays, 7:00am** (or whenever they start)
5. Save

That is the whole setup. One sentence, because the skill holds the logic.

## What happens each morning

Claude fires the task on its own infrastructure — the attorney's machine can
be off. It loads the `daily-attorney-brief` skill, resolves who the attorney
is on each connected platform, and makes four calls: today's events, tasks
due in the next seven days, overdue tasks, and matters updated since
yesterday. It ranks everything into one list and writes the brief.

Output sections: **Start here**, **Today**, **Needs attention**, **Matters
that moved**, and **Data notes** when something was incomplete.

## Test before scheduling

Run it as a normal chat first:

```
/lawlink:daily-attorney-brief
```

Confirm it identifies the right attorney and that the ranking looks sensible.
Debugging a scheduled task means waiting until the next morning; debugging a
chat does not.

## Changing the brief

Edit the skill in the `lawlink-skills` repository, rebuild, and publish.
Every attorney gets the new version without touching their task — the task
only names the work, it does not contain it.

Do **not** put brief logic in the task instruction. It would then have to be
edited on every attorney's account individually, which is the situation this
design exists to avoid.

## When a brief looks too quiet

An expired platform token returns nothing and reads exactly like a free
morning. The skill reports this under **Data notes** and will suggest
`connections action="check_connection"`. If briefs start arriving thin, check
that before assuming the diary is clear.
