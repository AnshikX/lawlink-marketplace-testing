---
name: court-records-vs-firm-data
description: Choose between the firm's own records and public court data, and never confuse a Trellis case with a matter. Use when researching an opposing party, opposing counsel, a judge, prior lawsuits, or looking up a case by court case number.
---

# Two different worlds

LawLink exposes two kinds of data that share vocabulary but mean entirely
different things. Picking the wrong one produces a confident answer to a
question nobody asked.

## The firm's own records

`contacts` · `matters` · `tasks` · `notes` · `events` · `documents` ·
`folders` · `activities` · `billing` · `invoices` · `intake_process`

These read the firm's practice-management and CRM systems — Filevine, Clio
Manage, Lawmatics, Lead Docket, SmartAdvocate and the rest. A **matter** is the
firm's own engagement. A **contact** is someone in the firm's system.

## Public court data

`trellis_cases` · `trellis_documents` · `trellis_rulings` · `trellis_alerts` ·
`trellis_webhooks` · `trellis_pacer`

These read state and federal court records. A Trellis **case** is a public
court case. It is **never** the firm's matter, and the two are not linked.

## Which to use

Reach for the `trellis_*` tools whenever the question is about public record or
litigation intelligence — **even if the user never says "Trellis"**:

- a person's or company's prior lawsuits
- an opposing party's or opposing counsel's case history
- a judge's rulings or tendencies
- looking up a case by its court case number
- court documents or rulings

Use the firm's-data tools when the question is about the firm's own work: its
clients, its matters, its deadlines, its bills.

## The phrasing that catches people out

> "Pull up the Henderson case."

If Henderson is the firm's client, that is `matters`. If Henderson is an
opposing party being researched, that is `trellis_cases`. Different records,
different IDs, no relationship between them.

When it is genuinely unclear, ask. Do not search both and merge the results —
that implies a link between a firm matter and a public case that does not
exist.

## Searching Trellis

`trellis_cases` with `action="search"` takes a query string supporting field
filters and boolean syntax:

```
county:losangeles AND judge:abbot
"Apple Inc."
"john smith"~1 AND (negligen* OR dismiss*)
```

Narrow with `search_type` (`state` or `federal`) and `state` (two-letter,
lower-case). Omitting `search_type` searches both.

`fetch` and `refresh` spend the firm's upstream Trellis quota. Use them when a
case genuinely is not indexed yet — not as a retry for a search that returned
nothing.
