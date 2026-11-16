# Changelog

All notable changes to this harness. The rule for this file mirrors the repo's own change discipline: one entry per change, with the *why*, not a diff dump.

## [Unreleased]
### Added
- **README: companion tool `reply-ending`.** Tells whether an agent's turn ended by asking you for something (a question, a decision, a command only you can run) or only reported, with a Stop hook that notifies only in the first case. Most asks in real sessions have no question mark, so "ends with ?" found 3 of 42 in a blind sample and this found 35.
- **README: companion tool `variant-diff`.** Screenshots N design variants and flags pairs that are one page in other colours. Measured on recolours other people made (dark modes, Bootstrap themes), and its README says what it misses: a skeleton reused with new text and pictures.

## [0.3.0] — 2026-11-16
Things learned from a pass over 133 trending repositories, each read by its code rather than its README. Most of them were platforms we would never install; a few held one idea worth keeping.

### Added
- **`skills/ship-secure` Section J: CI/CD pipelines.** A workflow runs with the repo's token and secrets, and on a public repo anyone can open the issue or pull request whose title ends up in a `run:` step. The section covers expression injection, `pull_request_target` and `workflow_run` with untrusted code, token permissions, pinning third-party actions by SHA, secrets in fork-triggered runs, self-hosted runners, cache poisoning and release jobs, and starts from two deterministic analyzers (zizmor, actionlint).
- **`skills/ship-secure` B5-B6: races and business logic.** A burst of identical requests against a balance or a coupon, and the flows no scanner sees: a refund after the thing was consumed, a step called out of order, a price or account id taken from the request body.
- **`skills/ship-secure` F5-F7: indirect injection.** Text the model reads (pages, documents, tool output) and not only what the user types; exfiltration through rendered Markdown images; least-privilege tools for agents and quarantine for memory written from untrusted sources.
- **`skills/goal`: resume means re-verify.** After a compaction, a session break or a fork, the loop re-reads its state file and re-checks the live state before acting, and a goal the user stopped never restarts on its own. Taken from the guard list of a goal-loop driver in an open-source agent harness.
- **README: companion tools.** Links to the standalone tools built alongside this harness, two of them new: `agent-census` (what an agent was offered versus what it used) and `ref-rot` (paths in agent instruction files that no longer exist).

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
