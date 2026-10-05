# Polish Week Plan: Oct 5 – Oct 9, 2026

**Goal:** Fix the most important gaps in my existing work, prove each fix with a PR and a test, and be able to explain every decision in English.

**Rules for the week**
- No new projects or big features. I fix, deepen and explain what I already built.
- One fix per branch and per PR. I write the PR description using `templates/PULL_REQUEST_TEMPLATE.md`.
- Before I fix anything, I explain the problem in my own words in the daily log. AI can teach me and quiz me, but I write the fix myself.
- Every task has a **Done when** line. If it isn't met, the task isn't done.
- Priorities: 🔴 Must (do it no matter what), 🟡 Should, ⚪ Could (only if time is left).
- When I run out of time, I cut ⚪ tasks first and never skip 🔴 ones.

**Daily routine (every day)**

| Time | What |
|---|---|
| Start (15 min) | Read today's plan, copy it into `daily-log/day-N.md`, pick the top 3 tasks |
| Each task | Explain the problem, then fix, test, open the PR, and fill in the "Explain it" answer |
| End (20 min) | Finish the daily log, update the PR table in `README.md`, send the 3-line update to my mentor |

---

## Day 1 · Mon Oct 5: Setup + T-Shirt Store API critical bugs

### 1.0 🔴 Setup (45 min)
- [ ] Create the GitHub repo `polish-week-report` and push these files.
- [ ] Ask my mentor what format the evaluation will use (demo, written report or repo review). The message is in `daily-log/day-1.md`.
- [ ] Copy `templates/PULL_REQUEST_TEMPLATE.md` into `.github/pull_request_template.md` in the T-Shirt Store API and Task Management repos.
- [ ] Run the T-Shirt Store API locally and confirm that lint, build, unit tests and e2e tests run. Write down the real test numbers.
- **Done when:** the repo is public, the mentor message is sent, and I know my real test counts.

### 1.1 🔴 Two buyers can pay for the last item (3 h)
**The bug:** In `src/webhooks/webhooks.service.ts`, stock is checked when the order is created but only decremented when the payment webhook arrives. If two people pay for the last unit, the second decrement breaks the database rule that stock can't go below zero. The transaction rolls back, Stripe retries forever, and the customer is charged with no paid order.

- [ ] Write the problem in my own words in the daily log. Draw the timeline: Buyer A pays, Buyer B pays, webhook A runs, webhook B runs.
- [ ] Write a **failing** e2e test first: two orders for a variant with stock = 1, both payment webhooks fire, and I check the final state.
- [ ] Fix: replace the plain `decrement` with a conditional update. Use `updateMany` with `where: { id, stock_quantity: { gte: qty } }`, then check `result.count`. If it is 0, the item sold out.
- [ ] Decide what happens when it sold out, and write down why:
  - Option A: mark the order as failed and refund through Stripe.
  - Option B: reserve stock when the order is created and release it if payment expires.
  - Either way, the webhook must return 200 so Stripe stops retrying.
- [ ] The test passes. The PR is called `fix(orders): prevent overselling when two payments race for the last unit`.
- **Explain it:** Why does a check followed by a later decrement fail under concurrency? Why is `updateMany` with a `where` clause atomic? What did I choose, A or B, and what is the tradeoff?
- **Done when:** the PR is open, the test proves the race is handled, and I can draw the timeline from memory.

### 1.2 🔴 Order status rules (2.5 h)
**The bug:** In `src/orders/orders.service.ts`, a client can cancel a **paid** order, with no refund and no stock returned. The webhook also doesn't check the order is still pending, so a cancelled order can later be marked paid. `postStatusHistory` never checks that the order exists.

- [ ] Write a table of allowed status changes in the PR, using my real status values, for example:

  | From | Allowed to |
  |---|---|
  | PENDING | PAID, CANCELLED |
  | PAID | (SHIPPED / REFUNDED, depending on my enum) |
  | CANCELLED | (nothing) |

- [ ] Add one function, for example `assertTransition(from, to)`, that throws a 409 Conflict for any change not in the table. Use it in cancel, in the webhook and in `postStatusHistory`.
- [ ] Return 404 when the order doesn't exist.
- [ ] Unit tests: one test per allowed change and one per forbidden change.
- [ ] PR: `fix(orders): enforce valid status transitions`.
- **Explain it:** Why put the rules in one function instead of an `if` in each place? Why 409 and not 400?
- **Done when:** the PR is open and a paid order can no longer be cancelled.

### End of Day 1
- [ ] Fill in the daily log, update the PR table in the README and send the mentor update.

---

## Day 2 · Tue Oct 6: T-Shirt Store API cleanup + honesty pass

### 2.1 🔴 Payment Links (1.5 h)
**The bug:** In `src/products/products.service.ts` (around line 240), the link is reusable and carries the buyer's `userId`, so anyone who pays through it creates an order for that user. Missing metadata gives `parseInt` a `NaN`, which throws and causes endless retries.
- [ ] Tie the link to an order id, or make it single-use per user.
- [ ] Validate the metadata in the webhook. If it's invalid, log it and return 200 (don't throw).
- [ ] Add a test that sends invalid metadata and checks that nothing crashes.
- [ ] PR: `fix(payments): bind payment links to an order and validate metadata`.
- **Explain it:** What is the risk when payment metadata identifies the user instead of the order?

### 2.2 🔴 Rate limiting + proxy (45 min)
- [ ] Add `@SkipThrottle()` to the Stripe webhook controller. Stripe must never be rate-limited.
- [ ] Set `trust proxy` in `main.ts` (e.g. `app.set('trust proxy', 1)` with the Express adapter). Otherwise, behind Railway, every client probably shares one IP.
- [ ] PR: `fix(security): skip throttling on webhooks and trust Railway proxy`.
- **Explain it:** What IP does my app see behind a proxy, and why does that break per-user rate limits?

### 2.3 🔴 Remove the public manager login (15 min)
- [ ] Remove the manager credentials from the README and change that password in production.
- [ ] Offer a "request demo access" note instead, or a read-only demo user.

### 2.4 🟡 Prisma errors → correct HTTP codes (1 h)
- [ ] In the global exception filter, map `P2025` (record not found) → 404 and `P2002` (unique constraint) → 409.
- [ ] Test: `signOut` with an unknown token returns 4xx, not 500.
- [ ] PR: `fix(errors): map Prisma known errors to HTTP status codes`.

### 2.5 🔴 Test honesty pass (1.5 h)
- [ ] Remove all 133 `// Assert — your turn` comments.
- [ ] For each spec file, read every assertion and make sure I understand it. Rewrite any assertion that only repeats the implementation.
- [ ] Pick **one** test that would catch a real bug and be ready to explain it.
- [ ] PR: `test: clean up scaffolding comments and strengthen assertions`.

### 2.6 🟡 Real migrations (1 h)
- [ ] Generate a baseline migration from the current schema (`prisma migrate dev --name init` locally).
- [ ] For the deployed database, mark the baseline as applied with `prisma migrate resolve --applied` instead of re-running it. Read the Prisma "baselining" docs first.
- [ ] PR: `chore(db): add baseline migration`.

### 2.7 🔴 README truth pass (45 min)
- [ ] Use the real test numbers from Day 1 everywhere.
- [ ] Update `CLAUDE.md`: the session-revocation gap was fixed in `78c793b`.
- [ ] Add a "Known limitations" section and a "What I fixed in Polish Week" section that links to the PRs.

### 🟡 2.8 If time is left
- [ ] `getOrders` loads every order's latest status into memory. Write down how I would push the filter into the database query (even if I don't do it).
- [ ] Wrap order creation in a transaction.

### End of Day 2
- [ ] Fill in the daily log, update the README PR table and send the mentor update.

---

## Day 3 · Wed Oct 7: Task Management frontend

### 3.1 🔴 Lint to zero (1 h)
- [ ] Type `fetchData` with a generic: `fetchData<T>(query, variables): Promise<T>`. No `any` (`src/FetchData/fetchData.ts:4,18`).
- [ ] Move shared constants and types out of component files (e.g. `PointEstimate` from `Card.tsx`) into `src/types/` and `src/constants/`.
- [ ] Remove the dead mock data in `Task.tsx` and the empty `handleCancel`.
- [ ] PR: `refactor: fix lint errors and move shared types out of components`.
- **Done when:** `npm run lint` shows 0 errors.

### 3.2 🔴 The "Update" button does nothing (1.5 h)
**The bug:** In `src/ModalEditTask/ModalEditTask.tsx`, the Update button only calls `onClose`, while every dropdown change saves immediately.
- [ ] Choose one model and write down why:
  - **Draft:** keep changes in local state and send one mutation when the user clicks Update. Cancel discards the changes.
  - **Live save:** keep immediate saves, remove the Update button and show "Saved".
- [ ] Also fix the `onAssignee(fullName)` parameter, which actually receives a userId (line 15).
- [ ] PR: `fix(edit-task): make Update button commit changes`.
- **Explain it:** Which model did I pick, and what does the user expect when they click Cancel?

### 3.3 🔴 Search and filters (1 h)
- [ ] Debounce search by about 300 ms with a small `useDebounce` hook (`TopNavigationBar.tsx:19`).
- [ ] Add `placeholderData: keepPreviousData` to the tasks query so changing a filter doesn't flash a full-screen "Loading".
- [ ] PR: `perf(search): debounce input and keep previous data while refetching`.
- **Explain it:** How many requests does typing "design" send before and after the fix?

### 3.4 🟡 Accessible modals (1.5 h)
- [ ] Change `role="menu"` to `role="dialog"` with `aria-modal="true"` and `aria-labelledby`.
- [ ] Close on Escape, move focus into the modal when it opens and back to the trigger when it closes.
- [ ] Make the search clear icon a `<button>` with an `aria-label` instead of `<img onClick>`.
- [ ] Move `NotificationProvider` up to `main.tsx` and clear the toast's `setTimeout` on unmount.
- [ ] PR: `fix(a11y): modals as dialogs with keyboard support`.

### 3.5 🔴 Token + README (45 min)
- [ ] Add `.env.example`.
- [ ] Add a README "Security note": `VITE_*` variables end up in the public JS bundle. In a real product the token would stay on a backend that forwards requests. For this challenge, I accept the risk and explain why.
- [ ] Finish the sentence that ends with "Also", fix the checklist (status filtering was not built) and add "How to run lint/tests" plus a folder structure overview.

### End of Day 3
- [ ] Fill in the daily log, update the README PR table and send the mentor update.

---

## Day 4 · Thu Oct 8: Frontend tests + Design + PM + QA

### 4.1 🔴 Frontend tests (2.5 h)
- [ ] Install `vitest`, `@testing-library/react`, `@testing-library/jest-dom`, `@testing-library/user-event` and `jsdom`. Add `test: { environment: 'jsdom' }` to the Vite config and a `"test"` script.
- [ ] Tests:
  - [ ] `getDueDateStatus`: today, past and future dates
  - [ ] Avatar initials, including when there's no image
  - [ ] Filters → GraphQL variables mapping
  - [ ] One create-task flow with the API mocked
- [ ] Optional: a GitHub Action that runs lint + test on every PR.
- [ ] PR: `test: add Vitest + RTL with core unit and flow tests`.
- **Explain it:** Why test behavior and not implementation? What did I mock and why?

### 4.2 🟡 Vello (design) (1.5 h)
- [ ] Make `provider-card.tsx` match its docblock: replace the magic numbers (`top:14`, `right:14`, `width:26`, `py-[2px]`, `pr-[32px]`) with tokens, **or** change the claim to say which values are exceptions and why.
- [ ] Replace the JS `useState` hover with CSS `:hover`.
- [ ] Make the audit's badge token names match the code (`--green-100/700` vs `--success-tint` / `--text-brand`).
- [ ] Add 3 tests: rating is null, no photo (initials) and keyboard activation.
- [ ] Rename misspelled files (`guardaril`, `accesibilityIssue`, `workforFriday`), remove spaces from image names and rename the package (it's still `tanstack_start_ts`).
- [ ] Add a README section "What AI did vs what I decided", rewrite `Weekly-Deliverables/README.md` in my own voice, and add a screenshot at the top.

### 4.3 🟡 ReNest (PM) (1.5 h)
- [ ] Add an "Evidence vs assumptions" section. Be honest about whether Greg is a real person or an assumed persona, and list 3–5 interview questions that would validate him.
- [ ] Explain the RICE scores, especially Impact=3 for #4, and say what data would change the ranking.
- [ ] Add acceptance criteria for unhappy paths: failed upload, image limits, editing a listing, accessibility.
- [ ] Use one consistent problem statement (README lines 5 and 7 currently say it two ways) and keep feature IDs separate from ranks.
- [ ] Compress the 9.8 MB GIF.

### 4.4 🟡 QA submission (1 h)
- [ ] Re-read my `camila-mamani` folder in the QA repo with fresh eyes:
  - [ ] Does every test case have preconditions, steps, expected result and actual result?
  - [ ] Does every bug report have severity, priority, reproduction steps and evidence (a screenshot or video)?
  - [ ] Can I explain severity vs priority with an example from my own work?
  - [ ] Is there one bug I am proud of finding? Write down why it matters to the user.
- [ ] If the feedback I received points to gaps, fix 1–2 of them (in a PR if the repo allows it, or note them in the report).

### End of Day 4
- [ ] Fill in the daily log, update the README PR table and send the mentor update.

---

## Day 5 · Fri Oct 9: Report, portfolio, demo

### 5.1 🔴 Finish the report (`README.md` of this repo) (2 h)
- [ ] Fill in every section. Each claim links to a PR or commit.
- [ ] Add before/after screenshots or a short GIF for at least 2 fixes (the Update button and search debounce are easy to show).
- [ ] Write "How I used AI" honestly: where it helped, where it was wrong, and how I checked it.

### 5.2 🔴 GitHub portfolio (1 h)
- [ ] Give each repo a description, topics and a live link if it has one.
- [ ] Pin the 5 most relevant repos, with `polish-week-report` first.
- [ ] Put a screenshot or GIF at the top of every README. Compress the 17 MB GIF in Task Management's `public/`.
- [ ] Merge all PRs I'm confident in. Leave a comment on any I'm not.

### 5.3 🔴 Demo (2.5 h)
- [ ] Write `demo/script.md` (5 min) following the outline in that file.
- [ ] Rehearse out loud in English 3 times. Record once, listen, and cut what's unclear.
- [ ] Practice every question in `demo/hard-questions.md` without notes.

### 5.4 🔴 Send it (15 min)
- [ ] Send my mentor the report link plus 3 lines: what I fixed, what I learned, what I would do next.

---

## If I fall behind
1. Never skip: 1.1, 1.2, 2.5, 2.7, 3.2, 3.5, 5.1, 5.3.
2. Cut first: 2.6, 2.8, 3.4, 4.2 tests, 4.4 fixes.
3. Tell my mentor on the same day: "I'm behind on X because Y, my new plan is Z." That's ownership too.
