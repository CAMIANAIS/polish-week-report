# Day 8: Friday, Oct 16

## Top 3 for today
1.
2.
3.

## Problems explained in my own words
<!-- Write this before fixing. If I can't write it, I don't understand it yet. -->

### clearCart clears too much (bug B)
- **Who gets hurt:** A customer pays, adds a hoodie, and the hoodie disappears.
- **Cause:** clearCart runs when Stripe sends the webhook, after the order. It deletes the whole cart, including the hoodie.
- **Options:** (1) subtract the ordered quantity: more code to write. (2) clear the cart when the order is created: empty cart if payment fails.
- **Decision:** subtract the quantity, because an empty cart hurts more.

### Plan for both clearCart bugs (A = not cleared, Q24 in day-7; B = clears too much)
- **One PR, 2 commits:** they touch the same clearCart. Kevin can review each commit, and I can revert one without losing the other.
- **Test A:** given clearCart throws → the order stays pending, the event is not saved, and the webhook returns 500, so Stripe retries.
- **Test B:** the order has 1 shirt. Cart 1 shirt + 1 hoodie → 1 hoodie stays. Cart 3 shirts → 2 stay.
- **Branch:** `fix/clear-cart`. Tests first, they must fail before the fix.

## Done
- [x] Finished SOC 2 training
- [x] ReNest: 2 "why" messages on the photo upload screen ("Photos from 5 angles can sell faster", "Showing the damage means buyers don't walk away"). No date message, because a message can't stop fake dates.
- [x] Aldía page + repo: "75% (3 of 4)", plate explanation, clearer data labels → `31312f5`
- [x] clearCart: plan for 2 bugs (above)
- [x] Daily message posted

## What I learned
- **3 pillars of observability:** logs = what happened, traces = where and how long, metrics = how often.
- **Order to debug:** metrics first (is it real, for everyone?), then traces (which step is slow?), then logs (why?).
- **Collect vs. see:** OpenTelemetry collects the data. Datadog, New Relic and Sentry let you see it.
- **NestJS Observe** (the tool Kevin showed me, [demo](https://www.observe-demo.nestjs.com)): free tier = metrics + traces + errors. Logs are paid. My API has `@nestjs/core` 11.2.1, so it works (needs 11.1.4+).
- **Bug A in production** would show on the errors screen: the 500 and the stack trace. The trace shows where it failed: clearCart.

## Blockers / questions for my mentor
-

## Next
- Write the failing tests for clearCart, then the fix
- Install NestJS Observe on my API in a new branch (secret in .env only)
- Keep studying for Claude Certified Architect – Professional
