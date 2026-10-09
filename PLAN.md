# Polish Week Plan: Oct 5 – Oct 9, 2026

**Goal:** Fix the most important gaps in my existing work, prove each fix with a PR and a test, and be able to explain every decision in English.

> **Updated Mon Oct 5 (afternoon)** after reading all my mentor feedback (`feedback/raw/`) and a
> full review of my 5 repos (`feedback/raw/review-findings.md`, private). New tasks are marked 🆕.
> Rules for me and Claude are in `CLAUDE.md`.

**Rules for the week**

- No new projects or big features. I fix, deepen and explain what I already built.
- **Feedback first.** Every piece of mentor feedback maps to a task (see the map below).
- One fix per branch and per PR. I write the PR description using `templates/PULL_REQUEST_TEMPLATE.md`.
- Before I fix anything, I explain the problem in my own words in the daily log. AI can teach me and quiz me, but I write the fix myself.
- Every task has a **Done when** line. If it isn't met, the task isn't done.
- **Docs must match the code.** In every repo, I found claims that weren't true (lint "done" but failing, "complete" but not). Every README claim must be true.
- Priorities: 🔴 Must (do it no matter what), 🟡 Should, ⚪ Could (only if time is left).
- When I run out of time, I cut ⚪ tasks first and never skip 🔴 ones.

**Daily routine (every day)**

| Time              | What                                                                                          |
| ----------------- | --------------------------------------------------------------------------------------------- |
| Start (15 min)    | Read today's plan, copy it into `daily-log/day-N.md`, pick the top 3 tasks                    |
| Each task         | Explain the problem, then fix, test, open the PR, and fill in the "Explain it" answer         |
| Each 🔴 PR        | Send it to a mentor the same day: "Does this approach make sense?"                            |
| Speaking (15 min) | Explain today's fixes out loud in 1 min each, and answer 3 hard questions                     |
| End (20 min)      | Finish the daily log, update the PR table in `README.md`, send the 3-line update to my mentor |

## Feedback → task map

| Track    | Feedback (my words)                    | Status                                             | Task               |
| -------- | -------------------------------------- | -------------------------------------------------- | ------------------ |
| Frontend | Delete didn't work                     | ✅ Fixed in `5e2bd27`, but the mentor doesn't know | 1.0 message        |
| Frontend | Not responsive / say it's desktop-only | Partly fixed in `5c5d5db`                          | 3.6 🆕             |
| Frontend | Reusable components, CSS modules       | ✅ Already done                                    | Mention in report  |
| Frontend | Code review issue #1 (13 findings)     | Only 1 fixed, no reply                             | 1.0 reply, 3.1–3.6 |
| Backend  | Kebab-case URLs                        | PR #1 open, not merged                             | 2.9 🆕             |
| Backend  | Secrets in CI                          | ✅ Fixed in `57062ba`                              | —                  |
| Backend  | Skills must live in the repo           | Still symlinks                                     | 2.9 🆕             |
| Backend  | lint-staged; tests on pre-push/CI      | Not done                                           | 2.10 🆕 ⚪         |
| Backend  | Edge cases                             | Many open (1.1, 1.2, 2.0, 2.1, 2.11)               | Day 1–2            |
| AI       | Fix my skills (issue #2)               | Done, but not re-reviewed                          | 1.0 message        |
| Design   | Summary must match the work            | README says "complete", notes don't                | 4.2a 🆕            |
| Design   | Show the project, not the process      | No images in README                                | 4.2a 🆕            |
| Design   | Make a product decision                | None shown                                         | 4.2a 🆕            |
| PM       | Intro, glossary, assumptions, tables   | ✅ Already done                                    | —                  |
| PM       | More users / who are the buyers?       | Only a buyer persona                               | 4.3 🆕             |
| PM       | Use competitors                        | Research stuck in my log                           | 4.3 🆕             |
| QA       | Other scenarios                        | Cross-account and role scenarios missing           | 4.4 🆕             |

---

## Day 1 · Mon Oct 5: Feedback + setup + overselling

_Reality: the morning went to reading feedback and reviewing my repos. That was worth it. 1.2 moves to Day 2._

### 1.0 🔴 Setup + messages (1 h)

- [ ] Push this repo to GitHub. **Check visibility first:** there are no private details in tracked files (`feedback/raw/` is ignored), and Ravn is OK with it being public. If unsure, keep it private and share it with my mentor.
- [ ] Write `feedback/summary.md` in my own words (no names).
- [ ] Send the messages (my own words; structure: what I did → what's next → one question):
  - [ ] Cohort lead/mentor: evaluation format + link to this repo + "PR by PR, or a summary on Friday?"
  - [ ] Reply on Task Management **issue #1**: delete fixed (`5e2bd27`), which findings I'll fix this week and which I'll leave, with reasons.
  - [x] My frontend mentor: delete is fixed, please check again.
  - [x] My AI mentor: comment on **PR #1**, issue #2 is addressed, please re-review.
- [ ] Copy `templates/PULL_REQUEST_TEMPLATE.md` into `.github/pull_request_template.md` in the T-Shirt Store API and Task Management repos.
- [ ] Run the e2e tests locally (needs Testcontainers) and write down the real counts. Known so far: **193 unit passed, 1 todo**.
- **Done when:** the messages are sent and I know my real test counts.

### 1.1 🔴 Two buyers can pay for the last item (3 h)

**The bug:** In `src/webhooks/webhooks.service.ts:64-69`, stock is checked when the order is created (`orders.service.ts:81-96`) but only decremented when the payment webhook arrives. If two people pay for the last unit, the second decrement breaks the database rule that stock can't go below zero. The transaction rolls back, Stripe retries forever, and the customer is charged with no paid order.

- [x] Write the problem in my own words in the daily log. Draw the timeline: Buyer A pays, Buyer B pays, webhook A runs, webhook B runs.
- [ ] Write a **failing** e2e test first: two orders for a variant with stock = 1, both payment webhooks fire, and I check the final state.
- [ ] Fix: replace the plain `decrement` with a conditional update. Use `updateMany` with `where: { id, stock_quantity: { gte: qty } }`, then check `result.count`. If it is 0, the item sold out.
- [ ] Decide what happens when it sold out, and write down why:
  - Option A: mark the order as failed and refund through Stripe.
  - Option B: reserve stock when the order is created and release it if payment expires. (The Stripe workshop described this as "like movie tickets: you have 10 minutes".)
  - Either way, the webhook must return 200 so Stripe stops retrying.
- [ ] The test passes. The PR is called `fix(orders): prevent overselling when two payments race for the last unit`.
- [ ] Send the PR to my backend mentor: "Does this approach make sense?"
- **Explain it:** Why does a check followed by a later decrement fail under concurrency? Why is `updateMany` with a `where` clause atomic? What did I choose, A or B, and what is the tradeoff?
- **Done when:** the PR is open, the test proves the race is handled, and I can draw the timeline from memory.

### End of Day 1

- [ ] Fill in the daily log, update the PR table in the README and send the mentor update.

---

## Day 2 · Tue Oct 6: T-Shirt Store API edge cases + honesty pass

_⚠️ This day is overloaded (~10 h). Before I start, I decide what moves or gets cut (see "If I fall behind")._

### 1.2 🔴 Order status rules (2.5 h), moved from Day 1

**The bug:** In `src/orders/orders.service.ts:328-345`, a client can cancel a **paid** or **processing** order, with no refund and no stock returned. The webhook (`webhooks.service.ts:31-54`) doesn't check the order is still pending, so a cancelled order can later be marked paid. `postStatusHistory` (`:274-292`) returns 409, not 404, when the order doesn't exist.

- [ ] Write a table of allowed status changes in the PR, using my real status values, for example:

  | From      | Allowed to                                 |
  | --------- | ------------------------------------------ |
  | PENDING   | PAID, CANCELLED                            |
  | PAID      | (SHIPPED / REFUNDED, depending on my enum) |
  | CANCELLED | (nothing)                                  |

- [ ] Add one function, for example `assertTransition(from, to)`, that throws a 409 Conflict for any change not in the table. Use it in cancel, in the webhook and in `postStatusHistory`.
- [ ] Return 404 when the order doesn't exist.
- [ ] Unit tests: one test per allowed change and one per forbidden change.
- [ ] PR: `fix(orders): enforce valid status transitions`.
- **Explain it:** Why put the rules in one function instead of an `if` in each place? Why 409 and not 400?
- **Done when:** the PR is open and a paid order can no longer be cancelled.

### 2.0 🆕 🔴 Cart quantity validation (45 min)

**The bug:** cart quantity validation is too weak. Details are in my private notes (`feedback/raw/review-findings.md`) until it's fixed.

- [ ] Failing test first: invalid quantities are rejected with 400.
- [ ] Fix the validation in both DTOs.
- [ ] PR: `fix(carts): validate cart quantities`.
- **Explain it:** Why isn't `@IsNumber` enough? Where else does a bad value travel in my code if it gets past the DTO?

### 2.1 🔴 Payment Links (1.5 h)

**The bug:** In `src/products/products.service.ts:218-256`, the link is reusable and carries the buyer's `userId`, so anyone who pays through it creates an order for that user. In `webhooks.service.ts:88-104`, the check `!session.metadata` is never true, because metadata is `{}`, not missing. So `parseInt` gets `undefined`, returns `NaN` and throws, which causes endless retries.

- [ ] Tie the link to an order id, or make it single-use per user. (Workshop idea: create the DB record first, then put its id in the metadata.)
- [ ] Validate the metadata **fields** in the webhook. If they're invalid, log it and return 200 (don't throw).
- [ ] Add a test that sends invalid metadata and checks that nothing crashes.
- [ ] PR: `fix(payments): bind payment links to an order and validate metadata`.
- **Explain it:** What is the risk when payment metadata identifies the user instead of the order?

### 2.2 🔴 Rate limiting + proxy (45 min)

- [ ] Add `@SkipThrottle()` to the Stripe webhook controller (`webhooks.controller.ts:21`). Stripe must never be rate-limited.
- [ ] Set `trust proxy` in `main.ts` (e.g. `app.set('trust proxy', 1)` with the Express adapter). Otherwise, behind Railway, every client probably shares one IP.
- [ ] PR: `fix(security): skip throttling on webhooks and trust Railway proxy`.
- **Explain it:** What IP does my app see behind a proxy, and why does that break per-user rate limits?

### 2.3 🔴 Demo accounts: keep public, document the known risk (15 min) ✅ decided

- ~~Remove the manager credentials from the README and change that password in production.~~ Not doing: I keep the demo accounts as a known risk.
- [x] Document the risk under the demo accounts table in the API README (`62b4999`).

> The demo accounts are public on purpose because this is the way reviewers can log in using different roles. The risk is bots or any user can for example deactivate products. I accept it because it is for learning purposes only.

### 2.4 🟡 Prisma errors → correct HTTP codes (1 h)

- [ ] In the global exception filter (`all-exceptions.filter.ts:44-50`), map `P2025` (record not found) → 404 and `P2002` (unique constraint) → 409.
- [ ] Test: `signOut` with an unknown token returns 4xx, not 500.
- [ ] PR: `fix(errors): map Prisma known errors to HTTP status codes`.

### 2.5 🔴 Test honesty pass (1.5 h)

- [ ] Remove all 133 `// Assert — your turn` comments.
- [ ] For each spec file, read every assertion and make sure I understand it. Rewrite any assertion that only repeats the implementation (e.g. `orders.service.spec.ts:288` only checks `toHaveBeenCalled()`).
- [ ] Pick **one** test that would catch a real bug and be ready to explain it.
- [ ] PR: `test: clean up scaffolding comments and strengthen assertions`.

### 2.6 🟡 Real migrations + stock constraint (1.5 h), moved up on Oct 5

**Why it moved up:** On Oct 5 I found that neither my test DB (`prisma db push`) nor production has the "stock ≥ 0" CHECK constraint. Prisma can't express CHECK in `schema.prisma`, so the only way to add it is a migration. The `updateMany` fix (1.1) protects me in code; the constraint protects me if a future decrement forgets the `where` (defense in depth).

- [ ] Generate a baseline migration from the current schema (`prisma migrate dev --name init` locally).
- [ ] For the deployed database, mark the baseline as applied with `prisma migrate resolve --applied` instead of re-running it. Read the Prisma "baselining" docs first.
- [ ] Add a second migration with raw SQL for the CHECK constraints in `docs/schemaERD.sql:94,141` (stock ≥ 0 and the others).
- [ ] Switch e2e setup from `db push` to running migrations, so tests have the same rules as production.
- [ ] PR: `chore(db): add baseline migration and stock CHECK constraint`.
- **Explain it:** Why isn't the code fix enough on its own? Why can't `db push` give me the constraint?

### 2.7 🔴 README truth pass (45 min)

- [ ] Use the real test numbers everywhere. Today `README.md:112` and `:179` give two different e2e counts.
- [ ] Update `CLAUDE.md:82`: the session-revocation gap was fixed in `78c793b`.
- [ ] Add a "Known limitations" section (include 2.11 if I don't fix it) and a "What I fixed in Polish Week" section that links to the PRs.

### 2.8 ⚪ If time is left

- [ ] `getOrders` loads the whole status-history table on every status filter (`orders.service.ts:195-199`). Write down how I would push the filter into the database query (even if I don't do it).
- [ ] Wrap order creation in a transaction.

### 2.9 🆕 🔴 Close the AI-module loop (30 min)

- [ ] After my AI mentor re-reviews, merge PR #1 (kebab-case URLs). Today main still has `paymentLink`, `forgotpassword`, `resetpassword`.
- [ ] `.claude/skills/*` are symlinks to `../../.agents/skills/`, which isn't in the repo. Commit the real skill files so anyone who clones gets them.
- **Explain it:** Personal skills vs project skills vs org skills: where does each live, and why?

### 2.10 🆕 ⚪ Faster git hooks (30 min)

- [ ] Pre-commit runs lint + build + tests. Switch it to `lint-staged` (lint changed files only) and move tests to pre-push or CI.
- **Explain it:** Why does this matter on a project with 6,000 tests?

### 2.11 🆕 🟡 More payment edge cases (1 h). Fix or document.

- [ ] **Paying twice:** a payment edge case. Details are in my private notes until it's fixed.
- [ ] Orders paid by payment link have no shipping address (`webhooks.service.ts:106-125`).
- [ ] `clearCart` empties the whole cart, including items added after the order (`webhooks.service.ts:139-141`).
- For each one, I decide: fix it now, or write it in "Known limitations" with why.

### End of Day 2

- [ ] Fill in the daily log, update the README PR table and send the mentor update.

---

## Day 3 · Wed Oct 7: Task Management frontend

### 3.1 🔴 Lint to zero (1 h)

Today there are **7 errors**, but `README.md:110` says linting is done.

- [ ] Type `fetchData` with a generic: `fetchData<T>(query, variables): Promise<T>`. No `any` (`src/FetchData/fetchData.ts:4,18`).
- [ ] Fix the 5 `react-refresh/only-export-components` errors by moving shared constants and types out of component files (`PointEstimate` from `Card.tsx:17`, `statuses` from `Dashboard.tsx:12`, plus `NotificationContext.tsx:28`, `SearchContext.tsx:34`, `TaskColumn.tsx:12`) into `src/types/` and `src/constants/`.
- [ ] Remove the dead mock data in `Task.tsx:18-94` and the empty `handleCancel` (`Card.tsx:78`).
- [ ] PR: `refactor: fix lint errors and move shared types out of components`.
- **Done when:** `npm run lint` shows 0 errors.

### 3.2 🔴 Editing a task: when does it save? (2 h)

**The bug:** In `src/ModalEditTask/ModalEditTask.tsx:248`, the Update button only calls `onClose`, while every dropdown change saves immediately. 🆕 The name saves `onBlur` (`:52`), so clicking Cancel _saves_ the name first, every blur sends a mutation and a toast, and an empty name is allowed (create blocks it, edit doesn't).

- [ ] Choose one model and write down why:
  - **Draft:** keep changes in local state and send one mutation when the user clicks Update. Cancel discards the changes.
  - **Live save:** keep immediate saves, remove the Update button and show "Saved".
- [ ] Block an empty name in edit, like in create.
- [ ] Also fix the `onAssignee(fullName)` parameter, which actually receives a userId (line 15).
- [ ] 🆕 Write down (fix if there's time): the edit modal lives inside the Card (`Card.tsx:100`), so changing status in grid view moves the card and the modal disappears.
- [ ] PR: `fix(edit-task): make Update button commit changes`.
- **Explain it:** Which model did I pick, and what does the user expect when they click Cancel?

### 3.3 🔴 Search and filters (1 h)

- [ ] Debounce search by about 300 ms with a small `useDebounce` hook (`TopNavigationBar.tsx:19`).
- [ ] Add `placeholderData: keepPreviousData` to the tasks query so changing a filter doesn't flash a full-screen "Loading" (`Dashboard.tsx:24`).
- [ ] PR: `perf(search): debounce input and keep previous data while refetching`.
- **Explain it:** How many requests does typing "design" send before and after the fix?

### 3.4 🟡 Accessible modals (1.5 h)

- [ ] Change `role="menu"` to `role="dialog"` with `aria-modal="true"` and `aria-labelledby` (`ModalEditTask:49`, `ModalConfirmation:18`, `ModalEditDelete:21`).
- [ ] Close on Escape, move focus into the modal when it opens and back to the trigger when it closes.
- [ ] Make the search clear icon a `<button>` with an `aria-label` instead of `<img onClick>` (`TopNavigationBar.tsx:21`).
- [ ] Move `NotificationProvider` up to `main.tsx`, clear the toast's `setTimeout` on unmount, and fix the error text that says "SearchProvider" (`NotificationContext.tsx:30`).
- [ ] PR: `fix(a11y): modals as dialogs with keyboard support`.

### 3.5 🔴 Token + README + deploy (1 h)

- [ ] Add `.env.example`.
- [ ] Add a README "Security note": `VITE_*` variables end up in the public JS bundle. In a real product the token would stay on a backend that forwards requests. For this challenge, I accept the risk and explain why.
- [ ] Finish the sentence that ends with "Also" (line 142), fix the checklist (line 128: status filtering was not built), fix the broken GIF path (line 28) and add "How to run lint/tests" plus a folder structure overview.
- [ ] 🆕 Add `vercel.json` so refreshing `/settings` on the live site doesn't give a 404.

### 3.6 🆕 🟡 Responsive: finish it or say it (45 min)

- [ ] Modals have `min-width: 800px` (`ModalEditTask.module.css:23`), so screens from 641 to 800px overflow. The card is fixed at 348px and the top nav has no media query.
- [ ] Either fix these, or say clearly in the README which screen sizes are supported (the mentor said to tell stakeholders).

### End of Day 3

- [ ] Update **issue #1** with which findings are now fixed (link the PRs).
- [ ] Fill in the daily log, update the README PR table and send the mentor update.

---

## Day 4 · Thu Oct 8: Frontend tests + Design + PM + QA

### 4.1 🔴 Frontend tests (2.5 h): not doing this week

> **Why not:** I prioritized the 2 PRs my frontend mentor reviewed: debounce (TM #3) and lint (TM #2).

- [ ] Install `vitest`, `@testing-library/react`, `@testing-library/jest-dom`, `@testing-library/user-event` and `jsdom`. Add `test: { environment: 'jsdom' }` to the Vite config and a `"test"` script.
- [ ] Tests:
  - [ ] `getDueDateStatus`: today, past and future dates
  - [ ] Avatar initials, including when there's no image
  - [ ] Filters → GraphQL variables mapping
  - [ ] One create-task flow with the API mocked
  - [ ] 🆕 Delete sends a valid mutation (the bug a mentor found)
- [ ] Optional: a GitHub Action that runs lint + test on every PR.
- [ ] PR: `test: add Vitest + RTL with core unit and flow tests`.
- **Explain it:** Why test behavior and not implementation? What did I mock and why?

### 4.2a 🆕 🔴 Vello: make the summary match the work (1 h)

This is direct mentor feedback.

- [ ] Fix the honesty gap: `Weekly-Deliverables/README.md:15` says "Status: complete" and `README.md:17` says Wednesday was "stress-tested", but `Day-3-Wednesday/process-log.md:9` says it's a known failure. The headline should say what my notes say.
- [ ] Show the result first: embed real screenshots at the top of the README (today there are zero images, and `image (1).png` is missing).
- [ ] Add a "What AI did vs what I decided" section. Rewrite `Weekly-Deliverables/README.md` in my own voice (line 24 calls me "the Nerd").
- [ ] Add one **product decision**: question one requirement of the ProviderCard and write what I decided and why.
- [ ] Check my Vello docs for a shared link that shouldn't be public (see private notes).

### 4.2b 🟡 Vello: code matches its claims (1.5 h)

- [ ] Make `provider-card.tsx` match its docblock: replace the magic numbers (`:31-34`, `:78-81`, `:97`, `:123`, plus `rating.tsx` gaps) with tokens, **or** change the claim to say which values are exceptions and why.
- [ ] 🆕 The card is a `<button>` containing `<div>` and `<p>` (invalid HTML), and the rating's `aria-label` is on a plain `<span>`. Fix it, since my audit says the a11y work is done.
- [ ] Replace the JS `useState` hover with CSS `:hover`.
- [ ] Make the audit's badge token names match the code (`--green-100/700` vs `--success-tint` / `--text-brand`).
- [ ] Add 3 tests: rating is null, no photo (initials) and keyboard activation.
- [ ] ⚪ Rename misspelled files (`guardaril`, `accesibilityIssue`, `workforFriday`), remove spaces from image names, rename the package (`tanstack_start_ts`), and keep one copy of the docs instead of 3.

### 4.3 🔴 ReNest (PM) (1.5 h), moved up from 🟡: tradeoffs done, rest is a next step

> **Why:** my PM mentor didn't ask for the rest. I asked her where I should put the tradeoffs, and she told me at the end of each epic. That is done in ReNest PR #1.

Already done after feedback ✅: intros, glossary, assumption callouts, tables.

- [ ] Add an "Evidence vs assumptions" section. Be honest about whether Greg is a real person or an assumed persona, and list 3–5 interview questions that would validate him.
- [ ] 🆕 Add the **seller** as a second user. Two of my top 3 risks are about sellers, and my own log says "ReNest has two users".
- [ ] Add acceptance criteria for unhappy paths: failed upload, image limits, editing a listing, accessibility.
- [ ] 🆕 Move my eBay / Facebook Marketplace research from the log into the deliverables (mentor feedback).
- [ ] Explain the RICE scores, especially Impact=3 for #4. Hint: check whether #4 still wins at Impact=2.
- [ ] Use one consistent problem statement (README lines 5 and 7), keep feature IDs separate from ranks, remove leftover template text, and make the docs agree (video length, value proposition).
- [ ] ⚪ Compress the 9.8 MB GIF.

### 4.4 🟡 QA submission (1 h): not doing, QA process ended

> **Why:** the QA submission ended with all my PRs approved on Friday. I asked my QA mentor about the Ravn QA process, and he told me to focus on criticality: sitemap + criticality → high test cases → high test scenarios → smoke suite → rest of test cases / test scenarios → regression suite. **Next step:** add criticality to my sitemap, because the smoke suite depends on it.

My QA work is already strong (almost all mentor reviews approved). Only polish here.

- [ ] Hardcoded dates (`2026-10-07`, `2026-10-08`) break the tests this week. Make them dynamic, or note it.
- [ ] Mark SC-01 and SC-02 as expected failures in the code (they fail on purpose because of D2 and D4).
- [ ] 🆕 Add **one** cross-account scenario: patient B tries to cancel or edit patient A's appointment (TP-13/14 were planned but never written).
- [ ] Can I explain severity vs priority with an example from my own work? (Note: 17 of my 20 TPs are P1. Why is that a problem?)
- [ ] Is there one bug I am proud of finding? Write down why it matters to the user.
- [ ] Check first whether the QA repo still accepts PRs. If not, note the fixes in my report.

### End of Day 4

- [ ] Fill in the daily log, update the README PR table and send the mentor update.

---

## Day 5 · Fri Oct 9: Report, portfolio, demo

### 5.1 🔴 Finish the report (`README.md` of this repo) (2 h)

- [ ] Fill in every section. Each claim links to a PR or commit.
- [ ] Add a "Feedback I received → what I changed" section, using the map at the top of this plan.
- [ ] Add before/after screenshots or a short GIF for at least 2 fixes (the Update button and search debounce are easy to show).
- [ ] Write "How I used AI" honestly: where it helped, where it was wrong, and how I checked it.

### 5.2 🔴 GitHub portfolio (1 h)

- [ ] Give each repo a description, topics and a live link if it has one.
- [ ] Pin the 5 most relevant repos, with `polish-week-report` first.
- [ ] Put a screenshot or GIF at the top of every README. Compress the 17 MB GIF in Task Management's `public/`.
- [ ] Merge all PRs I'm confident in. Leave a comment on any I'm not.

### 5.3 🔴 Demo (2.5 h)

- [ ] Write `demo/script.md` (5 min) following the outline in that file. Show results, not process (design feedback).
- [ ] Rehearse out loud in English 3 times. Record once, listen, and cut what's unclear.
- [ ] Practice every question in `demo/hard-questions.md` without notes (3 per day all week, so this is a final pass).

### 5.4 🔴 Send it (15 min)

- [ ] Send my mentor the report link plus 3 lines: what I fixed, what I learned, what I would do next.
- [ ] Thank the mentors whose feedback I applied, with a link to the fix.

---

## If I fall behind

1. Never skip: 1.0, 1.1, 1.2, 2.0, 2.5, 2.7, 2.9, 3.2, 3.5, 4.2a, 4.3, 5.1, 5.3.
2. Cut first: 2.8, 2.10, 3.4, 4.2b, the ⚪ items in 4.3 and 4.4.
3. Fix or document: 2.11 (write it in "Known limitations" if no time).
4. Tell my mentor on the same day: "I'm behind on X because Y, my new plan is Z." That's ownership too.
