# Day 7: Monday, Oct 12 (evaluation week)

## Top 3 for today
1. Decide: fix or document the clearCart gap (Q24)
2. Sort my 25 hard questions 🟢 🟡 🔴
3. Say Q1 (overselling) out loud and record it

## Problems explained in my own words
<!-- Write this before fixing. If I can't write it, I don't understand it yet. -->

### clearCart can fail after payment (Q24)
- **Who gets hurt:** The user pays, but the cart does not clear. They can get confused about whether the payment went through, and they might pay again.
- **Cause:** clearCart runs after the transaction. If it fails, Stripe retries. On the retry, the unique constraint fails (P2002), so the code returns 200. Stripe stops, and the cart never clears.
- **Order paid and stock decrement are atomic.** Only clearCart is outside.
- **Fake fix:** Just moving clearCart inside the transaction is a fake fix. It uses its own Prisma instance, not the tx one, so it does not roll back.
- **Real fix (A):** Pass the tx prisma as a parameter to clearCart. If it fails, rollback deletes the Stripe event too, so the retry starts clean.
- **Decision:** Document it, don't fix it now. Only 2 real callers (controller and webhook), but I prioritize practicing my hard questions in evaluation week.

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
