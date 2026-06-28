# Tool registry — template

The registry is the fast index of every tool you've evaluated with `skills/vet`. One row per candidate, appended on every verdict — **including rejects**, so you never evaluate the same thing twice (even when it returns under a new name).

Copy this table into your own private notes (e.g. `REGISTRY.md` outside any public repo) and append to it.

| Date | Candidate | Type | Verdict | Why (one line) | Where it landed / return trigger |
|------|-----------|------|---------|----------------|----------------------------------|
| 2026-01-15 | example-formatter | library | 🟢 | TLS-fingerprint bypass for scrapers behind blocks | pip --user, used in the scraper project |
| 2026-01-18 | example-loop-runner | skill | 🧪 sandbox | Useful design coach but ships an external runner outside our guards | test on a throwaway repo first; cherry-pick the rubric |
| 2026-01-20 | example-mega-framework | framework | 🔴 | Giant: 500+ blocks, auto-running hooks, settings rewrite — conflicts + excessive agency | cherry-pick a single block via /vet, never install whole |
| 2026-01-22 | example-signing-mcp | MCP | 🔴 | Transaction signing pointed at real wallets — red line | never; revisit only with an empty test wallet |

## Verdict legend
- **🟢 adopt** — safe, useful, low effort. Note where it landed.
- **🧪 sandbox** — potential but risk/uncertainty. Note what to test on a throwaway env.
- **⏸️ defer** — useful but not now. Note the trigger that brings it back.
- **🔴 reject** — unsafe / duplicates without benefit / not your use case. Note the reason.

## Why rejects matter
A reject row with a reason saves you from re-evaluating the same tool in three months when a new post hypes it. Record the *why* tersely; that one line is the whole value.
