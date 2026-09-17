---
name: billing-reconciliation
description: Trace a settlement, payout or processor deposit back to the bills it paid on Clio Manage. Use for questions about payouts, settlements, reconciliation, "what was this deposit for", or matching bank deposits to invoices.
---

# Reconciling a payout to its bills

## Why this needs a recipe

The platform publishes no payout object and no batch object. A processor
deposit lands in the bank as one lump sum, and nothing on it names the bills it
covers. There is no endpoint that answers "what was in this payout" — it has to
be walked, in order, backwards from the bank.

## The walk

Four hops, all on the `billing` tool:

1. **`action="bank_transactions"`** — find the deposit. Rows flagged `PAYOUT`
   are processor deposits. Scope with `bank_account_id` (from
   `action="bank_accounts"`) and a date range.
2. **`action="online_payments"`** — the individual card/eCheck payments the
   processor collected. Only `state="completed"` has actually settled; every
   other state has not moved money.
3. **`action="allocations"`** — how each payment was applied. One payment can
   be split across several bills, and the allocation is the only record of
   that split.
4. **`action="bills"`** — the invoices, for the matter and client context that
   makes the answer readable.

## Start at the bank, not the payments

`online_payments` **accepts no date range.** That is a platform limitation, not
a missing LawLink feature.

So never begin at step 2 trying to bound it by date — you cannot. Begin at
`bank_transactions`, which is date-bounded, and let the deposit scope
everything downstream.

If asked "what settled last month", the honest shape of the answer is: the bank
transactions in that window, then the payments reachable from them. Say which
of those you actually filtered on.

## Trust vs operating money

`action="bank_accounts"` distinguishes trust from operating accounts. Never
present a combined total across the two without labelling it. They are
different money, and in most jurisdictions mixing them in a report is a
compliance problem rather than a rounding one.

## Before reporting a total

Check whether the result was capped. A partial scan renders almost identically
to a complete one, and an empty partial result reads exactly like "there were
no settlements". If the reply carries a truncation or per-platform error
notice, say so rather than reporting the figure as final.
