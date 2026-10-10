# Demo script (5 minutes)

Read one line, pause, then read the next. `/` = short pause.

---

## 0:00–0:30 · Who I am + what I did

This week I reviewed my 4 projects as if I were the evaluator, using AI first. I pasted all the transcripts of meetings I had with my mentors, so I have their feedback. I made a proposal of what I would fix, when, and why I chose those. I asked them about the approach and got their acceptance. I worked on PRs that were reviewed by my backend mentor and my frontend mentor. The rest (PM, design) are mostly from feedback.

---

## 0:30–1:00 · How I reviewed

I ran lint. / It was failing on the frontend project, / so I fixed it and sent a PR to my mentor.

I added 1 PR with an e2e test for overselling, / and 2 e2e tests for cart validation.

I compared my docs against my code. / On design, I needed to work on the Wednesday task better.

**Show:** TM PR #3 (frontend mentor thread)

---

## 1:00–2:30 · Main story: overselling

In case there is a race condition between 2 buyers: / a customer paid and got no shirt.

My fix is in a PR, ready for review, / with a conditional update, / which stops overselling.

Buyer B gets a refund, / and an idempotency key stops a double refund.

The decision: refund vs reservation. / I chose refund, / since having just 1 shirt in stock is very uncommon.

I do have a guardrail though: / if there are more than 5 refunds per week, / I'll change the code.

**Show:** API PR #3 (test + fix)

---

## 2:30–3:30 · Merge fix

Two PRs passed alone, / but broke together. / CI caught it.

I used grep / and found one test still calling the old URL.

Then a second failure: / my test expected 400, / but users really get 422.

A green test can lie: / if the test setup is different from production, / it fails.

**Show:** red CI → PR #5 → green CI

---

## 3:30–4:15 · How I used AI

I used concepts I learned in my CCA-F certification preparation.

**How I controlled it:** / permissions, pre-commit, CI, DB CHECK. / These are deterministic.

**How I checked it:** / independent reviewers: / my mentors / and a peer reviewer. / And the times I caught AI-generated code that didn't match my file (design deliverables).

**What I learned:** / my Claude session is not able to see its own mistakes. / So when I want an immediate second opinion, / I use another Claude Desktop session or Codex / to criticize the AI output.

I used this many times, / but especially on QA deliverables, / where I needed a product-minded overview.

---

## 4:15–5:00 · What I learned + next

Feedback made me grow: / mentors and my peer challenged my code, / and I refactored it into reusable pieces.

I used grep from my certification study time / on a real bug.

Next, I would learn decorators deeply, / finish my medium-priority frontend fixes, / and keep learning from PM and design.

**Show:** atlas (10 seconds)

**Then say:** "Any questions?" / and stop talking.

---

## Tips

- Speak slowly. Pausing is better than filler words.
- Show the test, not just the code. Tests are proof.
- Close with "Any questions?" and then stop talking.

## If they ask "Did you test the race?"

There's one e2e test (refunds buyer B when last shirt already sold). It runs buyer A, then buyer B, one after the other. It doesn't run them at the same time (no Promise.all). So it proves the sold-out path, not the race itself.

True answer: "I tested the sold-out path. A parallel test is my next step."
