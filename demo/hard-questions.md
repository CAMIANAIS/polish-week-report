# Hard questions: practice out loud, no notes

Write a short answer under each question, then practice saying it without reading.

## Backend: T-Shirt Store API
1. Walk me through what happens when two users pay for the last unit at the same time.
2. If a Stripe event fails partway through, what does Stripe do next, and what state is left behind?
3. Why keep order status in a history table instead of a column? What does that cost `getOrders`?
4. The role comes from the JWT. What happens if a manager is demoted while their token is still valid?
5. Pick one unit test that would catch a real bug. Why that one?
6. Why store money as BigInt cents?

## Frontend: Task Management
7. Walk me through, step by step, what happens between typing in search and seeing the cards update. How many requests?
8. Who can see your `VITE_API_TOKEN`? How would you fix that in a real product?
9. Why `invalidateQueries` instead of optimistic updates? What does the user see while it refetches?
10. Explain the `Avatar` component's broken-image check.

## Design: Vello
11. Your docblock says every value is a token. Why was `top:14` there?
12. Card padding: 15px matches the reference, 20px matches the token scale. Defend your choice.
13. What does the Lovable scaffold do that you can't explain?

## PM: ReNest
14. Is Greg real? What would change in your MVP if he isn't?
15. #4 wins on Impact=3. What evidence supports 3 rather than 2?
16. What would make you kill #4 after launch?

## QA
17. Severity vs priority: give an example from your own bug reports.
18. Which bug are you proudest of finding, and why does it matter to the user?

## General
19. Which parts did AI write, and how did you verify them?
20. What would you do differently if you started the cohort again?
21. What feedback did you get, and what did you change because of it?

## Peer review: webhook questions (line numbers on `main`)
22. How does the handler make sure the same Stripe event isn't processed twice? (`webhooks.service.ts:22-29`, `schemaERD.sql:303`)
    > **My answer:** `stripe_event_id` is the primary key. The second time, the insert fails with P2002, the catch returns 200, so Stripe stops retrying.
23. Why does `payment_intent.succeeded` check for `metadata.orderId` before doing anything? (`webhooks.service.ts:31-39`)
24. Why is `clearCart` called outside the transaction, and what happens if it fails? (`webhooks.service.ts:134-152`)
25. What happens if the stock is already 0 when the payment succeeds? (`webhooks.service.ts:64-69`, `schemaERD.sql:141`) On `main` vs. in PR #3?
