# Code principles

Behavioral rules against the typical mistakes an LLM makes in code. For trivial tasks — use judgment (bias toward caution, not speed). Derived from Andrej Karpathy's widely-shared CLAUDE.md principles, adapted and condensed.

1. **Think before code.** State assumptions; on ambiguity show the readings instead of silently picking one; if there's a simpler path, say so and propose it; if unclear, stop, name what's confusing, and ask. An unfamiliar version-like name (SDK / API field / v2 / new product) → look it up first (docs/grep/web), don't guess — partial recall from training ≠ current knowledge.

2. **Simplicity first.** The minimum code that solves the task. No features beyond the request, no abstractions for one-off code, no unrequested "flexibility/configurability", no handling of impossible cases. 200 lines where 50 would do → rewrite. Test: "would a senior say this is over-engineered?"

3. **Surgical edits.** Touch only what's required. Don't "improve" neighboring code/comments/formatting, don't refactor working code, match the existing style. Delete only the orphans (imports/vars/functions) that YOUR edits orphaned; someone else's dead code — mention, don't delete. Test: each changed line traces directly to the request.

4. **Drive through a verifiable target.** Turn the task into a checkable criterion BEFORE starting: "add validation" → "tests for invalid input, then green"; "fix a bug" → "a test reproducing the bug, then green". For multi-step work — a plan with a `verify:` on each step. **Proactive acceptance criterion:** before a non-trivial task, state the criterion yourself in one line ("I'll call this done when…") and let the user confirm or adjust — don't wait for them to spell it out. Framing a checkable target is the agent's job, not the user's: terse requests without an explicit criterion are what turn into expensive correction cycles. Skip this for genuinely trivial asks.

5. **Plan mode by default.** A non-trivial task (3+ steps or an architectural decision) → plan first, then code. If something goes wrong — STOP and re-plan immediately, don't push through. **Anti-stall (numeric ceiling):** ≥3 failed edits on the same error/file in a row = STOP, don't keep guessing (it burns context) → re-plan or ask. The plan covers verification steps too, not just the build.
