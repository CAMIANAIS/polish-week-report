# Day 1: Mon Oct 5

## Top 3 for today

1. Plan the week
2. Define my approach to mentors
3. Explain the overselling bug in my own words

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

- Planning and prioritizing.

## Blockers / questions for my mentor

-

## Mentor update (sent ✅ / ❌)

❌ Not sent.
