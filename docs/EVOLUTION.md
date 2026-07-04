# How this harness evolves itself

Most "for Claude Code" collections are **static**: you install them and that's it. They don't get smarter — you do, and the bundle stays where it was. This harness is built around the opposite idea: **a small core plus the machinery to grow itself, safely, over time.** That machinery is the real product, and it's what a dumped mega-bundle can't give you.

A harness that grows is worth more than a harness that's big.

---

## The four layers of self-learning

The system improves through structured signals, not vibes. In rough order of value:

### Layer 1 — a user correction is the strongest signal
When the user corrects a fact, a decision, an attribution, or an approach — that is **not a one-off edit**. It's a signal into the harness itself. Capture it as a durable `feedback` note (with the *why* and *how to apply it*) **in the same turn**, not deferred to "later". The most valuable and most-missed signal in any agent loop is this one: everyone watches the agent's output, nobody watches the human's correction of it. Wire the correction back in, and the same mistake doesn't return.

### Layer 2 — memory at discovery, not at the finish
A root cause, a working workaround, a live incident — write it to memory the **moment** you find it, not at session end. By the finish, context may be compacted or the session lost. Update the existing note rather than spawning a near-duplicate. (See `MEMORY_SYSTEM.md`.)

### Layer 3 — procedure as skill
When a reproducible task takes more than ~5 rounds of trial-and-error (or hits the same rake twice), don't just record the *finding* — record the **working procedure** (the exact steps). A one-off → a `feedback` note; a recurring workflow → a new skill. Next time you walk the known path instead of re-deriving it and burning tokens. Check for an existing skill/note first — fragmentation is its own failure.

**The promotion gate — don't enshrine guesses.** A skill is authoritative: the next session trusts it without re-deriving it. So promotion has a bar. Write the procedure down as a skill only when **all three** hold:

1. **A passing check.** The path was actually verified — a test went green, the command exited clean, the repro reproduced. "Seemed to work" is not a passing check.
2. **A named failure pattern.** You can name the specific failure this path avoids or diagnoses, not a vague "sometimes it breaks".
3. **At least one ruled-out dead-end.** A concrete approach you tried and eliminated, with the reason.

Missing any of the three → it's not a skill yet; leave a memory note explicitly marked *unverified* instead. And when you do write the skill, include a **"What didn't work"** section — the dead-ends you ruled out (with reasons) often save the next session more time than the golden path itself. (Gate criteria cherry-picked from [`Kulaxyz/self-learning-skills`](https://github.com/Kulaxyz/self-learning-skills) — a technique worth crediting, per this repo's own rules.)

### Layer 4 — controlled intake
Growth without a gate is just bloat. Everything new passes `/vet` (security blocking gate → duplication → real ROI → verdict) and lands in a registry so it's never evaluated twice. A reject with a reason is as much "learning" as an adopt: the system remembers what *not* to let in, even under a rebrand.

---

## The growth machinery (which skill does what)

| Mechanism | Skill / file | Role in evolution |
|-----------|--------------|-------------------|
| Intake gate | `skills/vet` | Decides what's allowed to enter the stack. |
| Upgrade process | `skills/workflow-upgrade` | The deliberate "improve the system" loop: audit → vet incoming → adopt with discipline → record. |
| Anti-rot hygiene | `skills/system-health` | Catches degradation (bloat, broken links, stale entries) before it breaks. |
| Routing map | `docs/TOOL_ROUTING.md` | Task → tool map; grows as the toolkit grows. |
| Change discipline | `docs/CHANGE_DISCIPLINE.md` | Every change reversible and recorded, so growth never corrupts. |
| Verdict memory | `examples/registry-template.md` | Don't evaluate the same thing twice. |
| Fact memory | `docs/MEMORY_SYSTEM.md` | Self-learning store; corrections and findings persist. |

These aren't independent tools — they form a loop: **discover → capture → vet → adopt with discipline → record → keep clean → repeat.**

---

## Points of growth

A live system always has two kinds of capability. Naming both is part of the discipline — it keeps the "what's next" honest instead of pretending everything is finished.

### Established (formed, working)
- The `/vet` intake methodology + registry.
- Safety guards (mutation = confirmation, external content = data, secrets as `file:line`, signing-to-real-funds never, result honesty).
- The file-based, self-learning memory pattern.
- Change discipline (backup → surgical → verify → record).
- Code principles (think-before-code, simplicity, surgical edits, verifiable targets, plan-mode).

### Emerging (being grown)
These are deliberately *open*, not gaps to hide:
- **Separation of judge from author** — semantic verification by a different pass/model, not the hand that did the work (already wired into `skills/goal`; generalizing further).
- **Multi-model councils** — `konsilium` runs one model in N lenses today; genuine multi-provider diversity is the next step.
- **Budget-aware loops** — time/token ceilings on autonomous work, beyond a simple cycle count.
- **Deeper verification rubrics** — typed, measurable done-criteria as the default everywhere, not just in `goal`.

When an "emerging" item matures, it moves up to "established" and a new one takes its place. That movement *is* the evolution.

---

## A pattern for auditing high-stakes code

Some code (anything touching money, production data, or user trust) deserves more than a single read-through. The pattern that holds up across domains:

1. **Fan out specialized auditors in parallel**, each scoped to one lens (correctness, security, data integrity, performance, whatever the domain calls for) rather than one generalist pass trying to hold all lenses at once. Narrow scope catches more per lens.
2. **Adversarially verify every finding** before acting on it — hand each one to a fresh, independent pass whose job is to *refute* it (see [`agents/adversarial-verifier.md`](../agents/adversarial-verifier.md)), not confirm it. A finding that survives an attempt to kill it is worth fixing; one that doesn't was noise.
3. **Fix only behind a reproducing check** — a failing test, a backtest, a rerun that demonstrably flips from bad to good. A fix with no reproduction is a guess wearing a diff.
4. **Treat deploy as the real test, and go look.** Passing tests locally isn't the finish line for high-stakes code — after it ships, deliberately find *positive evidence* in the actual logs/metrics that the fix behaved as intended. No errors is not the same as working; a feature can be silently dead from day one and "no errors" would never tell you.

Cherry-pick the layer that fits — a small change might only need step 2; a full subsystem audit wants all four.

## The principle
Curated does not mean frozen. It means **every addition passes a gate, and the system has a memory of why.** A static bundle peaks the day it's published. A harness with intake, hygiene, and a learning loop compounds. That compounding — not the block count — is the thing worth sharing.
