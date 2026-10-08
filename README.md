# Polish Week Report: Camila Mamani

> Ravn cohort, AQP · Oct 5 – 9, 2026
> One week to revisit my work, fix what I would do differently, and show what I learned.

**TL;DR** *(write this last, 3 sentences)*
_I reviewed my four cohort projects as if I were the evaluator. I found and fixed X bugs, the most important being … I learned …_

---

## 1. What I reviewed

| Project | Track | What it is | Repo |
|---|---|---|---|
| T-Shirt Store API | Backend | NestJS + Prisma + Stripe e-commerce API | [link](https://github.com/CAMIANAIS/T-Shirt-Store-API) |
| Task Management | Frontend | React + TanStack Query task board on a GraphQL API | [link](https://github.com/CAMIANAIS/Task_Management_Code_Challenge) |
| Vello Fidelity Review | Design + AI | Design fidelity audit and `ProviderCard` component | [link](https://github.com/CAMIANAIS/vello-fidelity-review) |
| ReNest | PM | Problem frame → PRD → RICE → delivery plan | [link](https://github.com/CAMIANAIS/ReNest) |
| QA Round Robin | QA | Test cases and bug reports | [link](https://github.com/ravn-qa/qa-nerdery-round-robbin-week-trainees/tree/main/trainees/development/submissions/camila-mamani) |

**How I reviewed:** _I read each repo as an evaluator would, ran lint, build and tests, used the app as a user, and listed every place where I could not explain my own code or where the code contradicted my docs._

## 2. What I found

| # | Project | Problem | Why it matters to the user | Severity |
|---|---|---|---|---|
| 1 | API | Two buyers can pay for the last item, and the second is charged with no order | Customer loses money | 🔴 |
| 2 | API | A paid order can be cancelled with no refund | Customer or store loses money | 🔴 |
| 3 | API | Payment links are reusable and identify the user, not the order | Wrong orders, endless webhook retries | 🔴 |
| 4 | Frontend | The "Update" button in the edit modal does nothing | User thinks changes are saved or unsaved | 🟡 |
| 5 | Frontend | Search sends a request on every keystroke | Slow UI, wasted requests | 🟡 |
| … | | | | |

## 3. What I fixed

| PR | Project | Fix | Test that proves it | Status |
|---|---|---|---|---|
| [API #3](https://github.com/CAMIANAIS/T-Shirt-Store-API/pull/3) | API | Prevent overselling the last shirt: conditional update, cancel + refund buyer B (idempotency key) | e2e: two webhooks for the last unit, refund spy called once with B's intent | ⏳ |
| [API #4](https://github.com/CAMIANAIS/T-Shirt-Store-API/pull/4) | API | Reject negative and decimal cart quantities (`@IsInt` + `@IsPositive`) | e2e: `-3` and `1.5` return 400 | 👍 |
| [TM #2](https://github.com/CAMIANAIS/Task_Management_Code_Challenge/pull/2) | Frontend | Lint 7 → 0: remove `any`, move hooks, contexts and constants to their own files | `npm run lint` 0 errors, `npm run build` passes | ⏳ |
| [TM #3](https://github.com/CAMIANAIS/Task_Management_Code_Challenge/pull/3) | Frontend | Debounce search (300 ms) + ✕ clears the box and the filter | Network tab: typing "design" sends only `des` and `design` | ⏳ |
| [ReNest #1](https://github.com/CAMIANAIS/ReNest/pull/1) | PM | Tradeoffs and decisions below 3 PRD features | Review by PM mentor | ⏳ |

_Status: ⏳ open · 👍 approved · ✅ merged_

## 4. Key decisions and tradeoffs

### Overselling: refund vs reservation
- **Options:** …
- **I chose:** …
- **Because:** …
- **Tradeoff:** …

### Edit modal: draft vs live save
- **Options:** …
- **I chose:** …
- **Because:** …
- **Tradeoff:** …

## 5. Before / after

| Before | After |
|---|---|
| _screenshot or GIF_ | _screenshot or GIF_ |

## 6. How I used AI
- **Where it helped:** …
- **Where it was wrong and how I caught it:** …
- **What I now do differently:** e.g. "I write the failing test myself before asking for help."

## 7. What I learned
1. …
2. …
3. …

## 8. What I would still improve
- …

## 9. Daily progress
[Day 1](daily-log/day-1.md) · [Day 2](daily-log/day-2.md) · [Day 3](daily-log/day-3.md) · [Day 4](daily-log/day-4.md) · [Day 5](daily-log/day-5.md) · [Full plan](PLAN.md)
