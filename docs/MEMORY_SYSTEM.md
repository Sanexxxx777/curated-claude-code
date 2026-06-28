# Memory system — file-based, self-learning

A persistent, file-based memory that survives across sessions. Each memory is one file holding one fact, with light frontmatter; an index file lists one-line pointers to all of them and loads into context each session. This is a pattern you can adopt directly — the principles matter more than the exact layout.

## Layout
- **One fact per file.** A short topic file with frontmatter: `name`, `description` (used to judge relevance on recall), and a `type` (who-the-user-is / feedback-on-how-to-work / project-state / external-reference).
- **An index** (`MEMORY.md`) — one line per memory: `name — hook`. The index is what loads each session; the body of a memory loads only when its topic is relevant. Never put memory content in the index.
- **Link liberally** with `[[name]]` between related memories — even a link to a not-yet-written memory marks something worth writing later.

## Two principles that make it work

### 1. Memory at discovery, not at the finish
Write a critical finding (a bug's root cause, a working workaround, a live incident) to memory **the moment you find it**, not at session end — by then context may be compacted or the session lost. Don't duplicate: update the existing file rather than spawning a near-copy.

### 2. A user correction is the strongest learning signal
When the user corrects a fact / decision / approach, that is not a one-off edit — it's a signal into the harness. Capture it as a `feedback` memory (with the *why*) **in the same turn**, not deferred. "Everyone watches the agent, nobody watches the user" — the most valuable and most missed signal.

## Internal files are optimized for the model, not for humans
Memory / index / project-context / rules are read by the model, not the user → on every touch, optimize for **recall + tokens**: dense, no decorative formatting (keep ⚠️/status markers as semantics), detail pushed to the topic file. Density ≠ losing facts — keep numbers/paths/ids/invariants 1:1, and verify preservation against a backup after any compaction.

## What NOT to save
- What the repo already records (code structure, past fixes, git history, project README).
- What only matters to one conversation.
- If asked to remember something the repo already holds, ask what was non-obvious about it and save *that*.

## Recall caveat
A recalled memory is a point-in-time observation, true when written. If it names a file/function/flag, verify it still exists before acting on it.
