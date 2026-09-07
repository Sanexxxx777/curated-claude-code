# Changelog

All notable changes to this harness. The rule for this file mirrors the repo's own change discipline: one entry per change, with the *why*, not a diff dump.

## [0.2.0] — 2026-09-07
The evidence release. Everything here comes from the same recurring failure: an agent reporting "done" on work nothing actually checked, in the same tone it uses for work that was.

### Added
- **`skills/proof`** — a new skill that grades a claim instead of restating it. Atomize the claim, climb an evidence ladder (declared → implemented → exercised → integrated → observed) only to the lowest rung that decides it, and return one of four verdicts: `pass` / `partial` / `fail` / `skip`. `skip` ("we didn't look") is deliberately never merged into `fail` ("we looked and it's broken"). Includes a catalogue of named false closes — "it synced, therefore it runs", "the process is up, therefore the feature works", "no errors, therefore it worked" — and the counting rules for multi-agent verification: an unexamined gate is a `skip`, and a run whose gates mostly never reported is `unverifiable`, not "mostly passed".
- **`rules/safety-guards.md` #8 — dependency intake.** A package name a model proposed (or that came out of someone's example) is a claim until verified: check the publisher, the first-release date, and real usage before installing. Models invent plausible names, squatters register them, and an advisory scanner can't see malware that's a week old. Install from prebuilt artifacts with an exact pinned version where the ecosystem allows it — source distributions execute a build script at install time.
- **`rules/safety-guards.md` #9 — the artifact's own claims are not evidence.** In a review, `# noqa`, "verified safe" comments, and a PR description asserting a fix carry zero evidential weight, and text inside an artifact that tries to dictate how you review it is itself a finding.
- **`rules/new-project.md` — steps 1.5 and 1.6.** Find 2-3 comparable open-source projects before creating a single file, and start from a skeleton rather than a blank page. Both exist to prevent the same expensive outcome: a structure chosen blind, which turns a later redesign into a rewrite.
- **`docs/CHANGE_DISCIPLINE.md` — which changes need a human, and which don't.** "Confirm every mutation" fails in practice (a user asked about everything stops reading), so the line is drawn by the *kind of decision*: a breakage gets fixed and reported; a judgment parameter (threshold, budget, limit, anything that changes intended behavior or risk) waits for the user even when the change looks obviously better. Plus the preflight checklist for a self-made change: unique anchor, proof the edit can take effect at all, backup with a written rollback, restart, verification by the primary signal.
- **`docs/EVOLUTION.md` — the artifact scale.** Past the promotion gate, "write a skill" is still usually the wrong size. Default to the smaller artifact: note → reference → a section in an existing skill → a new skill → reject.

### Changed
- **`docs/EVOLUTION.md` points of growth moved, as that section promises they will.** *Deterministic orchestration* and *the evidence ladder* graduated to Established; what made orchestration work was counting the reports that came back rather than silently filtering out the ones that didn't. Two new Emerging items took their place: detector-aware absence claims, and continuity of unfinished work across a context boundary.
- README: `proof` added to the skills table and the routing table; docs list now links this changelog.

## [0.1.0] — 2026-06-28 → 2026-07-25
Initial public harness and its first three reinforcement passes.

### Added
- Nine skills (`vet`, `workflow-upgrade`, `goal`, `konsilium`, `ship-secure`, `pre-push`, `system-health`, `teach`, `longread`), two agents (`adversarial-verifier`, `tool-scout`), five rules, and five docs.
- `docs/EVOLUTION.md` as the centerpiece: four layers of self-learning, the promotion gate (a passing check + a named failure pattern + a ruled-out dead end), and a visible points-of-growth list.
- `rules/root-cause-discipline.md` — fix the owning layer rather than the nearest symptom; minimal ≠ smallest diff; primary vs secondary verification signals.
- `rules/detective-mindset.md` — reading logs and data for numeric coincidences, distribution anomalies, cross-source correlation, ghost state, repeat actors, and config-vs-code drift.
