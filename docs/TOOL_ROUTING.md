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

## Don't
- Don't install a paid/mutating tool without the user's command.
- Don't duplicate a built-in with an external tool (web fetch / browser automation / image reading often already exist).
- When the stack changes (a tool added or removed), update this map alongside it — a stale map routes you wrong.
