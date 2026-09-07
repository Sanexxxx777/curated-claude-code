# Starting a new project — "think before code" checklist

**Trigger:** a new project from scratch (a new repo / app / standalone tool). NOT for large features inside existing code — those are covered by default plan-mode (see `code-principles.md` #5).

**Invariant: not a single line of code until both steps below are done and the plan is explicitly approved.**

## Step 1 — self-check of understanding (in text, BEFORE the plan)
Answer 4 questions honestly, not for show:
1. **What real problem are we solving?** Not "what was asked", but why — the pain behind the request.
2. **Which requirements are already clear?** Firm, not assumed.
3. **Where are the ambiguities?** List the forks where readings could differ → voice them immediately, don't silently pick (see `code-principles.md` #1).
4. **What am I most likely to get wrong if I start coding now?** The most expensive risky assumption.

## Step 1.5 — references BEFORE code
Don't create a single file until you've found 2-3 comparable open-source projects: what problem each solves, its architecture, its stack, whether it's still alive, what to copy and what to avoid. Then the stack and architecture in your plan arrive with a citation instead of from scratch. This saves iterations, not minutes: the expensive rewrites come from a structure chosen blind, and half an hour of reading other people's layouts prevents them.

## Step 1.6 — start from a skeleton, not a blank page
If you have a starter template for this class of project (tooling, lint, test runner, base styles, naming conventions), copy it before writing anything. The value isn't the saved setup time — it's that the section skeleton and the naming law are fixed *before* the first line, so a later redesign stays a redesign instead of becoming a rewrite.

## Step 2 — formal plan
- Put it through plan-mode — the user approves BEFORE the first line of code.
- The plan includes verification steps (`verify:` on each step), not just the build.
- Open decisions that are the user's (stack, storage, scope) — surface them in the plan as questions, recommendation first.

## Don't
- Don't take a meta-instruction / discussion for a request to write code.
- Don't invent a spec if the task is named vaguely — stop and ask.
- Don't skip Step 1 for speed even on a "simple" project (test: would a senior say this is too early to be in code?).
