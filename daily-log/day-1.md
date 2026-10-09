# Day 1: Mon Oct 5

## Top 3 for today

1. Set up this repo and ask my mentor about the evaluation format
2. Fix overselling (T-Shirt Store API)
3. Enforce order status rules (T-Shirt Store API)

## Message to my mentor (send in the morning)

> Hi! For this week I reviewed my 4 projects as if I were the evaluator, and I'm documenting my fixes here: <repo link>. Quick question: for next week's evaluation, should I prepare a demo, a written summary, or will you review my repos and PRs? Thanks!

## Problems explained in my own words

<!-- Overselling timeline: Buyer A pays → Buyer B pays → webhook A → webhook B → ? -->

1. A creates order → check: pass → stock = 1
2. B creates order → check: pass → stock = 1
3. A and B both pay on Stripe
4. Webhook A → decrements → stock = 0
5. Webhook B → decrements → stock = -1
   Result: 1 shirt, 2 buyers paid, stock = -1. Buyer B paid for a shirt that doesn't exist. The store loses money and trust.

## Done

- [x] My AI mentor gave "LGTM" on issue #2 he created respect to 2 skills I created checking kebab-case use on endpoints.

## What I learned

-

## Blockers / questions for my mentor

-

## Mentor update (sent ✅ / ❌)

See "Message to my mentor" above.
