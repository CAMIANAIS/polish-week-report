# My glossary

Words from my own work this week, explained two ways: for an engineer and for a client.

| Word | For an engineer (1 line) | For a client (everyday example) | Where I used it |
| --- | --- | --- | --- |
| Idempotency key | Doing the same operation again gives the same result. | Like clicking "Pay" twice, but you only get charged once. | Overselling refund (API PR #3): without the key, buyer B could get refunded twice if Stripe retries the webhook. |
| Race condition | Two requests see the same stock and both buy it. One didn't get a shirt and already paid. | Two customers click "Buy" for the last shirt at the same second. Both saw stock 1. Buyer B paid but doesn't get a shirt, so we need to refund. | Overselling fix (API PR #3). |
| Conditional update | Update the stock only if stock gte quantity, in one step. Before, both checked before, so in one step nobody can buy between. | The cashier checks if there is still stock at the moment of selling. | Overselling fix (API PR #3): `updateMany` with `gte`. If nothing was updated, buyer B's order is cancelled and refunded. |
| Debounce | My hook doesn't call the API on every keystroke. The search runs after the timeout (300 ms after the user stops typing) with what is in the input. | Like an elevator: it waits until everyone is in, then closes once. | Search bar (Task Management PR #3): `useDebounce` hook. |
| Generic hook | `<T>` makes it reusable for any type of data (text, number…). | Like a generic charger: it can be reused on every phone. | `useDebounce<T>(value, delay)` (Task Management PR #3). |
| Deterministic vs. adaptive | The `CHECK` constraint is deterministic. `CLAUDE.md` is adaptive: someone can ignore it. | A lock on a door is deterministic. A "please don't enter" sign is adaptive. | DB `CHECK (stock_quantity >= 0)`, pre-commit hooks and CI (deterministic) vs. my `CLAUDE.md` rules and skills (adaptive). |
| Author bias | The author sees what they meant, not what they wrote. So mentors and a peer review it (and another Claude session or Codex for the AI's output). | Like checking your own essay: you read what you meant to say. | My PRs reviewed by mentors + peer review. |
