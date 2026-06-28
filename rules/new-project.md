# Starting a new project — "think before code" checklist

**Trigger:** a new project from scratch (a new repo / app / standalone tool). NOT for large features inside existing code — those are covered by default plan-mode (see `code-principles.md` #5).

**Invariant: not a single line of code until both steps below are done and the plan is explicitly approved.**

## Step 1 — self-check of understanding (in text, BEFORE the plan)
Answer 4 questions honestly, not for show:
1. **What real problem are we solving?** Not "what was asked", but why — the pain behind the request.
2. **Which requirements are already clear?** Firm, not assumed.
3. **Where are the ambiguities?** List the forks where readings could differ → voice them immediately, don't silently pick (see `code-principles.md` #1).
4. **What am I most likely to get wrong if I start coding now?** The most expensive risky assumption.

## Step 2 — formal plan
- Put it through plan-mode — the user approves BEFORE the first line of code.
- The plan includes verification steps (`verify:` on each step), not just the build.
- Open decisions that are the user's (stack, storage, scope) — surface them in the plan as questions, recommendation first.

## Don't
- Don't take a meta-instruction / discussion for a request to write code.
- Don't invent a spec if the task is named vaguely — stop and ask.
- Don't skip Step 1 for speed even on a "simple" project (test: would a senior say this is too early to be in code?).
