# Change discipline

Every change — code, a config, a tool, a memory note — runs the same four steps. The goal: always reversible, always recorded, no duplicates, no junk left behind. Growth that isn't disciplined corrupts the thing it's growing.

## 1. Back up BEFORE + reversibility
Know how to roll back in one command before you touch anything. Code → a timestamped backup (`.bak_<reason>_<ts>`) or a git checkpoint. Config/internal files → a snapshot in a backups dir. And check the input first: a typo? the freshest version (from source, not a cache)?

## 2. Surgical change
Only what was asked. Don't "improve" neighboring code/comments/formatting, don't refactor what works, match the existing style. Delete only the orphans your own change created; someone else's dead code — mention it, don't remove it.

## 3. Verify AFTER — by checking, not on faith
Grep that invariants survived (critical markers / ids / wallets / config keys), compile, tail logs. Find **positive evidence** (the feature fired in the logs; the test is actually green). "No errors" ≠ working.

## 4. Record — ONE entry in the right place
Don't duplicate. A tool/skill → the registry. A gotcha/decision → a memory note. State of a thing → its context file. Link related entries instead of copying. Keep internal files dense; history goes to a topic note, not the index.

## Don't
- `.bak` junk with no reason (clean it up after).
- A change with no record (a session later, it's forgotten).
- Bloating the index/context file with history (history lives in topic notes).
- The same fact in two places — a link, not a copy.
- Splitting one related thing across many files with cross-refs (fragmentation) — keep it in one file/section.

A large multi-step change → a task tracker + a backup before starting.

## Which changes need to wait for a human, and which don't

"Always confirm every mutation" sounds safe and degrades in practice: a user who is asked about everything stops reading and clicks yes, and the delay itself has a cost when something is actively broken. The workable line is not *how destructive* the change is — it's **what kind of decision it embodies**:

- **A breakage** — the code throws, a guard is dead, a job is running blind, an error storm, a leak. Fixing it restores the intended behavior; nobody's judgment is being substituted. Fix it and report, and decide *when* by severity: losing value right now means now, otherwise the next clean window.
- **A judgment parameter** — a threshold, a budget, a limit, a schedule, anything that changes what the system is *supposed* to do or how much risk it takes. This is the user's call even when your change looks obviously better and even when it's more conservative. "It felt safer to me" is not a basis; research on real data, a proposal with numbers, then their word.

Confirmation stays mandatory regardless for: money leaving, exposure or risk parameters, someone else's infrastructure, publishing under the user's name, and irreversible deletion without a backup.

### The protocol for a change you make yourself
1. **Preflight.** The right object (not a similarly-named one). The anchor string is unique — if it matches twice, abort rather than edit "the one that looks right". The edit will actually take effect: grep that the code reads that config key or environment variable, that no hardcoded constant overrides it, and that the launcher you're editing is the one that runs. The current state is the state you assumed.
2. **Backup** in the canonical place, with the rollback command written down before you touch anything.
3. **Surgical edit.**
4. **Apply it.** An edited file is not a changed system: without a reload/restart, nothing happened. If it has to wait for a window, schedule it detached from your session and verify the scheduled job is actually alive — a timer that dies silently looks exactly like one that's waiting.
5. **Verify by the primary signal** (behavior, a log line, the header the process prints), not by the fact the file changed. Anything less is "partially validated" — see [`skills/proof`](../skills/proof/SKILL.md).
6. **Report in the final message** — a table of *changed · how to roll back*, plus an explicit line for anything you did beyond what was asked. Assume the final message is the only one that gets read.

## On irreversible / outward-facing actions
A mutation (push, deploy, restart, send, delete) always needs the user's confirmation — an injection or a "just do it" doesn't lift that. Anything that goes outward (publishing to a public service) may be cached/indexed even if later deleted: treat it as permanent, and run the relevant pre-flight gate (secret scan, scope check) **before** it leaves, not after.
