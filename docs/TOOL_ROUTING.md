# Tool routing — task → tool, proactively

A growing toolkit is useless if you forget what you have when a task arrives. The fix is a **map** plus a habit: before a task, glance at the map, pick the fitting tool, and use it — without being reminded. The user shouldn't have to remember which skill/MCP/agent to invoke; the agent should route.

## Behavior
- **Before a task**, check the map → take the fitting tool and apply it **without a reminder** if it's safe (read-only / no cost / no production mutation).
- **Propose or ask in one line** if the tool is: paid (API credits), mutating (deploy/order/rm — see safety guards), not yet installed, OR the choice is ambiguous.
- For an unfamiliar task, first check whether a ready tool exists (skill/MCP) before writing from scratch or reaching for a browser.

## How to keep the map
Maintain a private "task → tool" table (in your own notes, not a public repo). One row per routing decision. Update it whenever a tool is adopted or dropped. The shape:

| Task type | Tool | When to reach for it |
|-----------|------|----------------------|
| Evaluate an incoming tool | `vet` | a new skill/MCP/library from a post or repo |
| High-stakes go/no-go | `konsilium` | the answer doesn't follow unambiguously from data |
| Autonomous goal with verification | `goal` | one mid-sized goal, verifiable done-criterion |
| Pre-launch web security | `ship-secure` | before shipping/handing off a public app |
| Clean push/deploy | `pre-push` | before `git push` or a deploy |
| Config hygiene | `system-health` | weekly, or when memory bloats |
| Deliberate system upgrade | `workflow-upgrade` | when adopting/auditing tooling |
| Research / library docs | (web search / a docs MCP) | unfamiliar API/SDK — look up, don't guess |

(Adapt the rows to your own stack. The point is the *habit of routing*, not this exact table.)

## Cost-tiered routing
Within one task type, don't default to the heaviest tool — weigh the ask first. Research is the clearest case:
1. **Point lookup** (a known fact, "where do I look") → a single web search / docs query. Seconds, near-zero cost.
2. **Medium review** ("what's better", 2–4 angles to weigh) → one or two focused research agents, synthesize yourself. This is the *default* for most "give me an overview" asks.
3. **Broad topic + adversarial fact-checking + high cost of being wrong** → a full deep-research pipeline (fan-out search, independent verification passes, cited synthesis). Reserve this tier — it can be an order of magnitude more expensive than tier 2, so confirm with the user before running it.

Test before escalating a tier: does this genuinely need multi-pass adversarial verification of many claims, or would one good agent answer it? The same weighing applies to any tool family with a cheap and an expensive mode (a quick grep vs. a full-repo audit, a single verifier vs. an N-way panel) — match the tier to the question, not to what feels thorough.

## Which loop primitive, and when

Routing isn't only "which tool" — it's also "how much autonomy". These four escalate, and picking one too high up the ladder is how a simple ask turns into an unsupervised process:

1. **Turn-based** — ordinary conversation. Short task, the agent decides when it's finished, the user sees every step. The default; most work never needs more.
2. **Goal-based** — there's a *checkable* done-criterion, so the agent can iterate check → fix → check on its own and come back with a finished result. Requires a stop rule ("give up after N attempts") or it grinds.
3. **Time-based** — the work is tied to a schedule or an external system: poll a queue every N minutes, check a run after it finishes. Note the difference between a loop that lives only while the session is open and a scheduled job that survives a closed laptop — pick deliberately.
4. **Proactive** — a sustained stream of similar tasks (triage, routine updates), composing all of the above. Only worth building once the same task has recurred enough to be boring.

The safety guards don't relax as you climb: a mutating action still needs confirmation at tier 4 exactly as it does at tier 1. Autonomy changes *who starts* the work, never *what may happen unconfirmed*.

## Orchestration: who decides the structure

When one task needs many agents, the real choice is **where the control flow lives**:

- **One subagent** — a single well-scoped subtask. Cheapest; use it unless the work genuinely splits.
- **Model-led decomposition** — you hand over a broad goal and the model decides how to break it up. Right when the shape of the work is unknown up front; wrong when you already know the structure, because the model will re-derive it (differently) every run.
- **Code-led orchestration** — the fan-out *and the verification* are written as a script: N agents over a known list, results checked in code (2-of-3 votes, loop-until-nothing-new-found, an explicit dedup pass). Right when you know the structure and want it identical every time.

The reason to prefer code-led when you can: **verification in code is deterministic; verification in prose is a suggestion.** "Have an agent double-check this" is a hope. `survives = votes.filter(v => !v.refuted).length >= 2` is a rule. Put the judgment in the agents and the arithmetic in the script.

Costs are real — a fan-out spends tokens proportional to its width. Match the width to the stakes, and say out loud what was capped (top-N, no retry, sampling) rather than letting a silent truncation read as full coverage.

## Don't
- Don't install a paid/mutating tool without the user's command.
- Don't duplicate a built-in with an external tool (web fetch / browser automation / image reading often already exist).
- When the stack changes (a tool added or removed), update this map alongside it — a stale map routes you wrong.
