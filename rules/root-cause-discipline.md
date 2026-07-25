# Root-cause discipline

Rules for *where* a fix belongs and *what counts as verified*. `code-principles.md` governs how you edit; this governs whether you're editing the right layer at all. The failure these prevent is the most common one an agent makes under time pressure: patching where the symptom is visible instead of where the behavior is owned.

## 1. Fix the owning layer, not the nearest symptom
A bug surfaces in a child component, a helper, a leaf function — that is where it became *visible*, not necessarily where it was *caused*. Look at the parent, the caller, the layer that owns the state or the contract before touching the place that reported it.

Banned patterns, regardless of how small the diff looks:
- A child-side fallback that papers over bad input from its parent.
- Defensive state repair downstream of the thing that corrupted the state.
- A guard, flag, or wrapper whose real job is to hide an error thrown further upstream.

Each one converts a bug into invisible wrong behavior — strictly worse than the crash, because now nothing reports it.

**Heuristic:** a one-file fix for cross-layer behavior is suspect until proven otherwise. If the symptom crosses a boundary (UI ↔ service, producer ↔ consumer, write ↔ read) and the fix doesn't, say out loud why that's correct.

## 2. Minimal ≠ smallest diff
Surgical edits are the default, but "surgical" means *no stray system footprint*, not *fewest characters changed*. When the correct fix and the smallest fix diverge, take the correct one with the smallest system footprint.

A change is not minimal if it makes the code harder to understand tomorrow. A three-line hack that requires a comment explaining why it's there is bigger, in the only unit that matters, than a ten-line fix that needs no explanation.

## 3. Research vertically and horizontally before a non-trivial fix
Before changing anything non-trivial, map two axes — and stop as soon as you've found the owning layer, not one file later:

- **Vertical (the execution path):** caller → handler → service → contract → persistence. Where does the value actually get set, and by whom?
- **Horizontal (the connected surfaces):** sibling code that does the same job elsewhere, *both* sides of a contract (producer and consumer), *both* paths for state (read and write), and the adjacent states (loading, empty, error) that the same change will touch.

This is bounded reconnaissance, not wandering. If you can't say which layer owns the behavior, you are not ready to edit — that's a re-plan, not a reason to start typing.

## 4. Primary vs secondary signals in verification
Not all green is equal. Rank the evidence before reporting anything as done:

- **Primary signal** — the user-visible or runtime behavior of the feature itself. Did the thing actually do the thing, observed directly (a real run, the actual log line, the rendered screen)?
- **Secondary signal** — tests, typecheck, lint, build, a clean compile.

Secondary signals green while the primary is unverified = **"partially validated"**, and it must be reported in exactly those words. Not "done", not "working". Passing tests prove the code compiles and the assertions you thought to write still hold; they don't prove the feature works, and a feature can be silently dead from day one with every test green.

The corollary from the safety guards holds here too: "no errors" is not evidence of working. Go find the positive evidence — the log line, the record written, the state changed — or say plainly that you didn't.

## Don't
- Don't fix at the layer where the stack trace happened to surface if the state was corrupted upstream.
- Don't add a guard whose real function is to make an upstream bug stop being reported.
- Don't equate "the tests pass" with "the feature works" in a status report.
- Don't start editing while you still can't name the owning layer.
