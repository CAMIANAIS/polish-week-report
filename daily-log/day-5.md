# Day 5

## Top 3 for today
1.
2.
3.

## Problems explained in my own words
<!-- Write this before fixing. If I can't write it, I don't understand it yet. -->

## Gaps I closed today
<!-- My own words. Question → what I understand now → next step. -->

### Refund inside or after the transaction?

Inside, because if the refund fails, the rollback erases "cancelled" and Stripe retries the webhook. If the refund is after, buyer B never gets the money back. The idempotency key stops a double refund when Stripe retries.

## Done
- [ ] Task: … → PR [#](link)

## What I learned
-

## Blockers / questions for my mentor
-

## Mentor update (sent ✅ / ❌)
> **Done:** …
> **Learned:** …
> **Next / blocked:** …
