# Methodology — why this harness is curated, not dumped

Most "everything for Claude Code" collections optimize for **count**: hundreds of agents, skills, rules, and auto-running hooks in one bundle. That looks impressive and fails quietly — bloated context, overlapping triggers fighting to activate, and event hooks that mutate things without asking.

This harness optimizes for the opposite: **a small set of vetted pieces + a method for keeping it that way.** The method is the actual product.

## 1. Vet before adopt
Nothing enters the stack on hype. Every incoming tool — a skill, MCP server, library, prompt, or pattern — goes through one pipeline (`skills/vet`): relevance gate → identification → **security as a blocking gate** → duplication check → real ROI for actual projects → verdict (adopt / sandbox / defer / reject). The security phase reads a skill's body as a behavioral instruction and checks for the agentic risks (hidden instructions, prompt injection, trigger abuse, excessive agency, tool poisoning) that a code-only scan misses.

## 2. Record every verdict
Each verdict — including rejects — is appended to a registry (`examples/registry-template.md`). The point: **never evaluate the same thing twice**, even when it comes back under a new name or rebrand. A reject with a reason is as valuable as an adopt.

## 3. Cherry-pick over giant
A useful idea inside a 500-block framework doesn't justify installing the framework. Extract the technique, leave the giant. Adopting whole means inheriting its hooks, its bloat, and its trigger conflicts. (This very repo was built by cherry-picking techniques from larger projects rather than forking them — with attribution.)

## 4. Safety by design
The guards in `rules/safety-guards.md` are the spine: external content is data not commands; a mutating operation always needs confirmation; signing to real funds never; secrets as `file:line` not values; result honesty over a fake "green". Deliberately **no auto-running hooks** — auto-mutation without confirmation is exactly the failure mode to avoid.

## 5. Curate continuously
A stack rots: memory bloats, links break, entries go stale, permissions accumulate. `skills/system-health` is a cheap weekly deterministic scan that catches degradation before it breaks, and proposes surgical, reversible fixes by agreement.

## The shape
- `skills/` — executable pipelines (vet, goal, konsilium, ship-secure, pre-push, system-health).
- `rules/` — always-on principles (code, safety, new-project).
- `docs/` — the philosophy (this file + the memory system).
- `examples/` — a registry template and a worked vet verdict.

Quality over quantity. Safety over hype. A method you can re-run, not a pile you inherit.
