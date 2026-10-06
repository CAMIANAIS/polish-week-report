# Day 2

## Top 3 for today

1.
2.
3.

## Problems explained in my own words

<!-- Write this before fixing. If I can't write it, I don't understand it yet. -->

## Done

- [ ] Task: … → PR [#](link)

## What I learned

- Fixing harness is better than loosening the assertion
  > In my own words: I need my test runs same as the real app, so I have many rules considered for error status in my backend same I need to consider on tests. Before my test passes, but it was not testing the real app.

## Blockers / questions for my mentor

-

## Messages to mentors

### Kevin (backend): overselling, sent ✅

> Hi Kevin, this week is all about polishing the work before the evaluation. I'm incorporating the feedback and reviewing my projects.
> I ran into an issue: Buyer B can pay for a T-shirt that doesn't exist because the stock check and the stock deduction happen at different times.
> My plan: I'm going to add a conditional update. Buyer A's update returns 1, Buyer B's returns 0. Buyer A gets the T-shirt, Buyer B's order fails, we issue a refund, and the webhook returns a 200 status so Stripe doesn't retry.
> I chose this option because stock only changes after payment, and refunds are rare (it only happens with the last T-shirt). If it happens more than five times a month, I'll switch to a timed checkout or hold the funds and only charge them upon shipping.
> Does this approach make sense? I'll send you the PR today.

**Reply:** yes, the approach is OK. New feedback: use a database transaction whenever a request writes more than once. (Full reply in `feedback/raw/backend.md`.)

### Steven (frontend lead): reply on Task Management issue #1, posted ✅

Link: https://github.com/CAMIANAIS/Task_Management_Code_Challenge/issues/1#issuecomment-6019999558

> Hi @stevenheffner thanks for the review!
> Update:
> Done: #1 Delte bug fixed in (https://github.com/CAMIANAIS/Task_Management_Code_Challenge/commit/5e2bd27538c11c86c3c805ed3523c60dbbe1ea1f)
> Blocked: the cohort Task Management API is down (404). Is there a new URL? Then you can check the fix live.
> Work I do this week: #2 (lint failing, it's a requirement) and #5 (Slow loading, users feel it first).
> Not this week: #3. It's a bigger change, and I'm fixing 4 projects this week. I chose what users feel first. If I finish asap I'll do it.

### Steven (frontend lead): Slack, sent ✅ 10:58

> Good morning Steven!
> I replied on issue #1: https://github.com/CAMIANAIS/Task_Management_Code_Challenge/issues/1#issuecomment-6019999558
> Done: delete is fixed. But the cohort API is down, so it can't be tested live. Is there a new URL?
> This week: I'll fix lint (#2) and slow loading (#5). I'm leaving #3 for now, because I'm fixing 5 projects and chose what users feel first when they are surfacing the app.
> Does my plan make sense? Would you like me to follow up with Francisco or Brayan? Thanks a lot! Have a nice day:disco_raven:

### Daniela (PM): Slack follow-up on ReNest, sent ✅ 11:20

> Good morning Daniela :grin:! Following up on my question from before. This week I'm polishing my projects before the final evaluation :elmo_fire: : https://github.com/CAMIANAIS/ReNest
> [internal Ravn Outline link removed]
> What I did: I chose "mark as sold + pickup confirmed" to count transactions. I'll review it with real data after the first month.
> What I want to improve:
> 1. Use my competitor research (e.g. Facebook Marketplace counts conversations)
> 2. Show the options I considered for counting transactions, and why I chose mine
>
> My question: where is the best place to explain tradeoffs in a PM doc: in the PRD, or in a separate "decisions" section?
> Do you have 20 minutes this week ? Thank you!

**Reply:** "it depends." If a tradeoff belongs to one feature or epic, put it in the PRD as an info section at the end of that epic. She also liked my clickable prototype. (Full reply in `feedback/raw/pm.md`.)

### Cohort Slack daily, posted ✅

> Good morning team! Here is my daily! :grin:
>
> Yesterday: Read all my mentor feedback (frontend, backend, AI, design, PM, QA) and reviewed my repos. Made a plan for Polish Week. Pedro gave LGTM on my AI module review (issue #2).
> Today: Explained the bug found and sent my approach to Kevin, he already gave me feedback about this approach. Next: write a failing test, then the fix and send him the PR. Also replying to the frontend review: delete is fixed, and this week I'll fix lint and slow error loading.
> Blockers: The cohort Task Management API is down (404 "Application not found"), so my deployed app can't load anything.
> Questions: Is there a new URL for the Task Management API? @steven  Thank you for checking.:pepenote:

## Mentor update (sent ✅ / ❌)

> **Done:** …
> **Learned:** …
> **Next / blocked:** …
