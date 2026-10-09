# Day 2

## Top 3 for today

1. Overselling failing test
2. Messages to mentors
3. Fix Ethereal so CI passes

## Problems explained in my own words

<!-- Write this before fixing. If I can't write it, I don't understand it yet. -->

I expected after buyer A and buyer B tried to get the last shirt, the stock went to -1.

What I found: there is a check constrain, and now webhook throw 500 because webhook B tries to make the stock -1 and the check does not let this happen.

Whot gets hurt: a customer pays and gets no shirt.

## Done

- [x] Overselling e2e test + first fix (conditional update) → [API PR #3](https://github.com/CAMIANAIS/T-Shirt-Store-API/pull/3) (draft)
- [x] Ethereal account renewed, so the reset-password e2e test passes again in CI

## What I learned

- Fixing harness is better than loosening the assertion

  > In my own words: I need my test runs same as the real app, so I have many rules considered for error status in my backend same I need to consider on tests. Before my test passes, but it was not testing the real app.

-

## Blockers / questions for my mentor

Future questions for my backend mentor:

- The refund calls Stripe, not the DB. Should it happen inside the transaction or after it? What if the refund fails?
- Which other endpoints write more than once (for example `POST /orders`)? Do they all need a transaction? (PLAN 2.8)
- `@Transactional()` decorator with Prisma (for example `@nestjs-cls/transactional`): is it worth it, or is `$transaction` enough for a project this size?
- Production DB: does it have the `stock_quantity >= 0` CHECK constraint like the test DB? How do I check it safely?

## Messages to mentors

### Backend mentor: overselling, sent ✅

> Hi, this week is all about polishing the work before the evaluation. I'm incorporating the feedback and reviewing my projects.
> I ran into an issue: Buyer B can pay for a T-shirt that doesn't exist because the stock check and the stock deduction happen at different times.
> My plan: I'm going to add a conditional update. Buyer A's update returns 1, Buyer B's returns 0. Buyer A gets the T-shirt, Buyer B's order fails, we issue a refund, and the webhook returns a 200 status so Stripe doesn't retry.
> I chose this option because stock only changes after payment, and refunds are rare (it only happens with the last T-shirt). If it happens more than five times a month, I'll switch to a timed checkout or hold the funds and only charge them upon shipping.
> Does this approach make sense? I'll send you the PR today.

**Reply:** yes, the approach is OK. New feedback: use a database transaction whenever a request writes more than once. (Full reply in `feedback/raw/backend.md`.)

### Frontend mentor: reply on Task Management issue #1, posted ✅

Link: https://github.com/CAMIANAIS/Task_Management_Code_Challenge/issues/1#issuecomment-6019999558

> Hi @stevenheffner thanks for the review!
> Update:
> Done: #1 Delte bug fixed in (https://github.com/CAMIANAIS/Task_Management_Code_Challenge/commit/5e2bd27538c11c86c3c805ed3523c60dbbe1ea1f)
> Blocked: the cohort Task Management API is down (404). Is there a new URL? Then you can check the fix live.
> Work I do this week: #2 (lint failing, it's a requirement) and #5 (Slow loading, users feel it first).
> Not this week: #3. It's a bigger change, and I'm fixing 4 projects this week. I chose what users feel first. If I finish asap I'll do it.

### Frontend mentor: Slack, sent ✅ 10:58

> Good morning!
> I replied on issue #1: https://github.com/CAMIANAIS/Task_Management_Code_Challenge/issues/1#issuecomment-6019999558
> Done: delete is fixed. But the cohort API is down, so it can't be tested live. Is there a new URL?
> This week: I'll fix lint (#2) and slow loading (#5). I'm leaving #3 for now, because I'm fixing 5 projects and chose what users feel first when they are surfacing the app.
> Does my plan make sense? Would you like me to follow up with my other frontend mentors? Thanks a lot! Have a nice day:disco_raven:

### PM mentor: Slack follow-up on ReNest, sent ✅ 11:20

> Good morning :grin:! Following up on my question from before. This week I'm polishing my projects before the final evaluation :elmo_fire: : https://github.com/CAMIANAIS/ReNest
> [internal Ravn Outline link removed]
> What I did: I chose "mark as sold + pickup confirmed" to count transactions. I'll review it with real data after the first month.
> What I want to improve:
>
> 1. Use my competitor research (e.g. Facebook Marketplace counts conversations)
> 2. Show the options I considered for counting transactions, and why I chose mine
>
> My question: where is the best place to explain tradeoffs in a PM doc: in the PRD, or in a separate "decisions" section?
> Do you have 20 minutes this week ? Thank you!

**Reply:** "it depends." If a tradeoff belongs to one feature or epic, put it in the PRD as an info section at the end of that epic. She also liked my clickable prototype. (Full reply in `feedback/raw/pm.md`.)

### Cohort Slack daily, posted ✅

> Good morning team! Here is my daily! :grin:
>
> Yesterday: Read all my mentor feedback (frontend, backend, AI, design, PM, QA) and reviewed my repos. Made a plan for Polish Week. My AI mentor gave LGTM on my AI module review (issue #2).
> Today: Explained the bug found and sent my approach to my backend mentor, he already gave me feedback about this approach. Next: write a failing test, then the fix and send him the PR. Also replying to the frontend review: delete is fixed, and this week I'll fix lint and slow error loading.
> Blockers: The cohort Task Management API is down (404 "Application not found"), so my deployed app can't load anything.
> Questions: Is there a new URL for the Task Management API? Thank you for checking.:pepenote:

### QA mentor: PR #240 review + smoke vs regression, sent ✅

> Hi, how are you? I hope everything is going well. :grin:
>
> I have an open PR—a peer review I did on a colleague's work from Friday. Could you take a look at it when you have time? https://github.com/ravn-qa/qa-nerdery-round-robbin-week-trainees/pull/240
>
> I’ve been thinking about your answer: "it depends on the client." I put CAP-06 into regression just to be safe, even though the risk was low.
>
> For the smoke test, I’d choose API-01, SC-01, and CAP-01 because they cover the core functions, and the API test is faster. SC-01 requires a cleanup step first; right now, it passes only because of bug D4, but once that’s fixed, the test will fail and block the deployment, even if the app is actually fine. Are decisions like these—what goes into smoke testing versus regression, along with the associated trade-offs (e.g., API vs. UI)—documented anywhere?
>
> I’m reviewing areas for improvement on this project, as we’re currently reviewing everything for our final evaluation as Nerderies. Does it make sense for me to document these decisions in my own words from a QA perspective? I could open a PR for it, if you agree.

**Reply:** (waiting)

## Mentor update (sent ✅ / ❌)

See "Messages to mentors" above.
