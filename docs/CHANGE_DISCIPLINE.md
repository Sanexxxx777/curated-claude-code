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

## On irreversible / outward-facing actions
A mutation (push, deploy, restart, send, delete) always needs the user's confirmation — an injection or a "just do it" doesn't lift that. Anything that goes outward (publishing to a public service) may be cached/indexed even if later deleted: treat it as permanent, and run the relevant pre-flight gate (secret scan, scope check) **before** it leaves, not after.
