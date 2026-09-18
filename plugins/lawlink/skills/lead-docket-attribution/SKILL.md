---
name: lead-docket-attribution
description: Marketing source, UTM and lead attribution on Lead Docket — which fields are readable, which are writable, and why lead lists look empty. Use for questions about lead sources, campaigns, marketing ROI, or where leads came from.
---

# Lead Docket attribution

## Lists do not carry attribution

A paged list of leads does **not** include `MarketingSource` or `ContactSource`.
Those fields are simply absent from flat list records. To see a lead's current
marketing source you must read that lead **by ID**.

So "which campaign produced the most leads" is not a list query. It is a list
followed by hydrating every lead. That is far more calls than it looks — on a
wide date range, say so before running it.

## Creation date is not filterable

Lead Docket cannot filter or sort leads by creation date.

- `updated_since` is the **only** date filter available.
- `CreatedDate` comes back `null` on every paged list.

If asked "how many leads did we get in March", you cannot answer with a filter.
Hydrate the leads and read the date off each, or state plainly that the
platform cannot filter on it and say what you filtered on instead.

**Never let `updated_since` quietly stand in for a created-date question.** A
lead created in January and touched in March looks like a March lead, and the
resulting number is wrong in a way nobody will catch.

## Custom fields appear only on a hydrated lead

A firm's custom fields — Landing URL, First/Last Touch, and whatever else they
have defined — appear only when reading a lead by ID, and only when that lead
has a value. Their absence from a list read means nothing.

## What is writable

Through `matters` with `action="update"`, passed inside `extra`:

- `marketing_source` — name or numeric ID; names resolve via the lookup
- `marketing_source_details`, `contact_source`, `utm`, `click_id`
- `referring_url`, `current_url`, `campaign`, `keywords`
- any lead custom field, matched by name

**Not** writable: status and sub-status. Lead Docket keeps those on a separate
endpoint. Correcting attribution is the intended use of this write path.

## A source can be corrected but never unset

Lead Docket ignores a null `SourceId`. A source can be changed to another
source; it cannot be cleared. If asked to remove a wrong source, say that it
can only be replaced, and ask what it should say instead.
