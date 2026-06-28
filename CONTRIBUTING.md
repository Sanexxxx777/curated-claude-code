# Contributing

This repo stays small on purpose. A new skill/rule earns its place; it isn't added because it exists.

## The bar (every PR)
1. **Passes its own `/vet`.** Security is a blocking gate. Read the [vet pipeline](skills/vet/SKILL.md) and apply it to your own contribution: relevance → security → duplication → real ROI → verdict.
2. **No auto-hooks.** No event-driven hooks that auto-run on tool events. Read-only or confirmation-gated only. See [`rules/safety-guards.md`](rules/safety-guards.md).
3. **No secrets, no private data.** No keys/tokens, no personal or infrastructure details, no real hostnames/wallets. Findings reference `file:line` + type, never values.
4. **Surgical.** Touch only what your change requires. Don't reformat or refactor neighboring content.
5. **Attribution.** If a skill adapts someone else's idea, credit it (as the existing skills do).

## What gets rejected
- "Here are 40 more skills." Curation is the point; volume isn't.
- Anything that mutates state without confirmation.
- Anything that ships code/data to an external service without it being obvious and consented.
- Duplicates of an existing skill without a clear, named difference.

## How to propose
Open an issue describing the gap first (what real task isn't covered). A skill that solves a concrete, recurring task beats a clever one that solves nothing in particular.
