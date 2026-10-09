# Day 3

## Top 3 for today

1. Finish the overselling fix (1.1)
2. Cart quantity validation (2.0)
3. Lint to zero on the frontend (3.1)

## Problems explained in my own words

<!-- Write this before fixing. If I can't write it, I don't understand it yet. -->

If stripe sends webhook B two times and there is no indempotencykey, buyer B's money could get as many times as they run webhook, so with refund-${id} I can prevent that

## Done

- [x] Backend PRs #3, #4
- [x] Frontend PRs #2, #3
- [x] Design approach message

## What I learned

-Refund after the transaction + event guard = if refund fails, buyer B never gets the money back.

Today there is a mock+ as , one day with testing real strpe functionality I do need to have on mind right now with AS Typescript is trusting but hen i need to change

Tradeoff for testing

- Mock = fast, stable, but it doesn't prove that Stripe accepts the refund
- Real = proves more, but it's slower and needs the network

In frontend
At first I committed fixes straight to main. This week I switched to one PR per fix, so mentors can see the diff and comment before it lands.

- Problem: the README says lint is done, but there were 7 errors. (Lint was a requirement.)
- What I did: any → unknown / { message: string }. Hooks and context in their own files. Constants in src/constants/.
- Why: with any, the user sees a blank screen instead of TypeScript warning me. Fast refresh: without it, the modal closes and "Buy milk" is lost on every save.
- Checked: npm run build ✅, lint 0 ✅, toast + estimate labels in the browser (API down)
- Bonus fix: "SearchProvider" → "NotificationProvider" in the error text.
- Idea for later: estimate order 8, 4, 1, 2, 0.

## Blockers / questions for my mentor

For designing team:

-Esta semana estoy aplicando los comentarios del feedback de la Design Week.

Mi plan es el siguiente:

1. Que el resumen coincida con el trabajo: modificaré el README para que el título refleje lo que dicen mis notas.
2. Mostrar el resultado primero: colocaré el enlace al sitio en vivo (https://vello-fidelity-review.vercel.app) en la parte superior del README, y el proceso más abajo.
3. Lo que hizo la IA frente a lo que yo decidí: una sección breve, con mis propias palabras.Por ejemplo PM tiene info seccion al final de cada Epic/Feature, aplicaria lo mismo para cada componente que revise con IA  vs design system, poner los tradeoffs y las decisiones que tome en base a ellas al final , como se ve ese flujo en el entregable de un designer en Ravn?
4. Una decisión de producto:  noté que el chip de tiempo de caminata muestra ícono + la palabra "walk" (redundante). Ahora quiero ir más allá: ¿el usuario realmente necesita el tiempo de caminata para decidir si confía y reserva? Voy a decidir si se queda, cambia o se quita, y explicar por qué.
   Mis pregunta
    - Cómo les gustaría ver los cambios:  https://vello-fidelity-review.vercel.app actualizado?, el README actualizado o un video corto?

## Messages to mentors

- Backend mentor: PRs API #3 and #4 sent ✅. He approved #4 and suggested `@IsPositive()` (applied in `cc6f562`). #3 marked ready for review + mention on the PR ✅
- Frontend mentor: PRs TM #2 and #3 sent ✅. He commented on #3: move the ✕ handler out of the JSX (`handleDelete`) and a challenge to make a `useDebounce` hook.
- Frontend mentor (lead): no reply yet.
- Design mentors: plan message sent ✅ (text above). Waiting for reply.
- PM mentor: ReNest #1 link: sent ✅
- General channel daily update: sent ✅
- Peer review: API + frontend repo links: sent ✅

## Mentor update (sent ✅ / ❌)

> **Learned:** T-shirt store sells single shirts, and a customer can buy 1. So today the rule is just "positive" not min(1)
See "Messages to mentors" above.
