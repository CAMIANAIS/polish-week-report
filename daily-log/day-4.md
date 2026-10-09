# Day 4

## Top 3 for today

1. Close frontend (my frontend mentor's 2 comments)
2. Structure demo day+report
3. Peer review with my peer

## Problems explained in my own words

<!-- Write this before fixing. If I can't write it, I don't understand it yet. -->

## Done

- [x] Demo accounts: kept public, risk documented in the API README → [`62b4999`](https://github.com/CAMIANAIS/T-Shirt-Store-API/commit/62b4999)
- [x] `handleDelete` refactor + generic `useDebounce` hook → [TM PR #3](https://github.com/CAMIANAIS/Task_Management_Code_Challenge/pull/3)
- [x] Main CI red after merging API #1 + #4 → fixed → [API PR #5](https://github.com/CAMIANAIS/T-Shirt-Store-API/pull/5)
  > The demo accounts are public on purpose because this is the way reviewers can log in using different roles. The risk is bots or any user can for example deactivate products. I accept it because it is for learning purposes only.

## What I learned

- If I am going to use debounce many times make it a hook reusable generic so I can call it when I need it
- Also keep return clean and easy to read , every event called on click needs to go out of it because it could keep growing.
- Ask review on my PR's heleped me to grow and challenge me in ways I was not able to see myself.
- the smoke suite is the high-criticality tests.That is why high cricality test cases first .The right flow in Ravn is sitemap + criticidad → test cases high --> test scenarios high → smoke suite → rest of test cases/ test scenarios --> regression suite
- Tests are only as true as their setup.A green test can still be wrong. Yesterday the test setup didn't match production, so the test checked 400, a status real users never get. Merging PR #1 made the test setup match production, and that showed the mistake

## Blockers / questions for my mentor

-

## Mentor update (sent ✅ / ❌)

**Done:**
To my frontend mentor
Reply: "This handleDelete could keep growing, but my return is clean and easy to read."
Commit: https://github.com/CAMIANAIS/Task_Management_Code_Challenge/pull/3/commits/e0de5ce

Daily
Gooood afternoon team! My daily right here
Yesterday

- Backend: PR reviewed and approved, I already applied mentor's suggestion.
- Frontend: PR's reviewed by my mentor.
- PM: tradeoffs + decisions under features already on PR.
- Peer review: I sent my repos to my peer so we could help improve each other.

Today

- Frontend: My mentor challenged me and I accepted the challenge, already commit my changes on my PR regarding this.
- Re-plan of the rest of the days: what's done / next step / cut
- Peer review: review my peer's API + frontend
  -I found All Hands Q4 session very helpful for all the advices they shared to us about where Ravn is going and how we can help.
  Any blocker right now

**Replies on the PR:**

- handleDelete thread: "This handleDelete could keep growing, but my return is clean and easy to read. e0de5ce"
- Debounce thread: "I added a useDebounce hook, generic and reusable. Thanks for challenging me! 2e0bdee"

