# Day 6: Oct 11, 2026

## Top 3 for today

1. Close my gaps
2. Design feedback
3. Security fix

## Problems explained in my own words

<!-- Write this before fixing. If I can't write it, I don't understand it yet. -->

### clearCart fails after the payment

The customer pays and the cart still has the items. The process: `clearCart` fails → the webhook sends an error → Stripe retries the webhook → line 22 gets an error (P2002, the event already exists) → the code does `return` → Stripe gets 200 and stops retrying → `clearCart` never runs again.

The default status in `schemaERD.sql:305` is `'processing'`, so the event stays `'processing'` forever. For next steps, I would use `processing_failed` and the index `idx_stripe_events_stuck`.

## Done

- Closed 2 gaps from memory (not recorded out loud yet): Q1 overselling (with the race condition and "atomic") and Q24 `clearCart` (found a hidden bug).
- Caught another AI that was wrong about my code: `POST /orders` doesn't decrement stock, so the race test must race the webhooks.
- Design: finished Wednesday → [Vello #1](https://github.com/CAMIANAIS/vello-fidelity-review/pull/1)
- Security fix: updated TanStack Start for a known XSS vulnerability → [Vello #2](https://github.com/CAMIANAIS/vello-fidelity-review/pull/2) (merged)
- Report: added the design work → [report #2](https://github.com/CAMIANAIS/polish-week-report/pull/2) (merged)

## What I learned

- When I saw "You're all set!", I knew it was a bug that affects the user, because they think there is nothing left to do.
- I made a product decision, the 24-hour wait, based on the user: how the requester feels while waiting for the provider to respond.

## Blockers / questions for my mentor

-

## Mentor update (sent ✅ / ❌)

To the design manager: ❌ not sent yet

> Hi! I applied all the feedback my design mentors gave me. I finished Wednesday and wrote myself all the unhappy paths I could think of. After that, I made a decision: the requester waits 24 hours maximum. I also found a bug: Vello shows the user "You're all set!" when they still need to wait for the provider's approval.
>
> PR: https://github.com/CAMIANAIS/vello-fidelity-review/pull/1
>
> Does this approach make sense?
