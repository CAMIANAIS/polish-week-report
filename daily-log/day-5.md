# Day 5

## Top 3 for today
1.
2.
3.

## Problems explained in my own words
<!-- Write this before fixing. If I can't write it, I don't understand it yet. -->

## Gaps I closed today
<!-- My own words. Question → what I understand now → next step. -->

### Refund inside or after the transaction?

Inside, because if the refund fails, the rollback erases "cancelled" and Stripe retries the webhook. If the refund is after, buyer B never gets the money back. The idempotency key stops a double refund when Stripe retries.

### Why did 1.5 return 201 when the column is Int?

Before my fix, 1.5 passed and got into the database (201). Now `@IsInt()` rejects 1.5 at the door and says: only whole shirts. Not verified yet: who changed 1.5 into a whole number. Maybe Postgres rounded it. Next step: check the saved value.

## Done
- I recorded a demo video of what I did this week polishing my projects.
- I finished the report with all findings, decisions, AI section, lessons, next steps and certifications plan.
- I merged and commented on the issues from my peer's feedback ([API PR #10](https://github.com/CAMIANAIS/T-Shirt-Store-API/pull/10) closes #7 and #9; replied to #6 and #8).
- I started closing my gaps: `Int` vs `@IsInt()`, and refund inside the transaction.

## What I learned
- Check before agreeing, even with human reviewers. My peer said `.env.example` didn't exist, but it was in the repo.

## Blockers / questions for my mentor
-

## Mentor update (sent ✅ / ❌)

Good morning team!
My daily report right here.

Yesterday
- Backend: merged my PR, my CI did not pass and I fixed it, with valuable lessons after that.
- Frontend: mentor's challenge done: generic `useDebounce` hook + `handleDelete` refactor, and I replied on the PR.
- Report section: How I used AI during this time.
- Peer review: reviewed my peer's API + frontend, 4 issues opened (refund bug, open GraphQL proxy, mobile search, skeleton flash). He also opened issues on my code.
- Speech rehearsal with my peer: I got great feedback.

Today
- Report README filled: fixes, decisions and tradeoffs.
- Peer review: I started to work on the 4 issues my peer opened on my API. With PR #10 I fixed 2 of them; #6 and #8 are a next step.
- Close all gaps of my learning with my own words on my artifact.
- Next today: polish my daily logs, make the report public and share it here.

Any blockers or questions?
