# Worked example — a /vet verdict

A fictional run of `skills/vet` on a made-up candidate, to show the shape of the output. Names are invented.

---

**Candidate:** `AcmeLint MCP` — an MCP server that lints your codebase and posts fixes, found in a Reels post claiming "50k stars, fully local, free".

### Phase 1 — identification
- Type: MCP server.
- Source: `github.com/example/acmelint-mcp`. **Verified, not trusted:** the badge shows 4.1k stars, not 50k (the post inflated it); last commit 8 months ago (semi-stale). Author has one other repo.
- License: MIT.
- Cost: free; but the "auto-fix" mode calls a hosted API (not "fully local" as claimed).
- Install: `npx acmelint-mcp install` — runs a script at install time.

### Phase 2 — security (blocking)
1. Executes code at install (`npx … install`) → read the script: it writes to the MCP config **and** registers a `PostToolUse` hook. Flag.
2. Wallets/keys: none. OK.
3. Exfiltration: "auto-fix" sends file contents to `api.acmelint.example` — **your code leaves the machine**. Flag for any private codebase.
4. Secrets: wants no keys directly, but the hosted call could carry whatever's in the files it reads.
5. Permissions: registers an event hook that runs on every tool use → **excessive agency**.

Two flags (silent exfiltration + auto-hook) → verdict capped at sandbox-or-below.

### Phase 3 — duplication
Overlaps with the local slop-scan + code-review already in the stack, which are read-only and don't send code anywhere.

### Phase 4 — potential
The lint-only (local) mode has marginal value over existing tools; the auto-fix (hosted) mode is the only differentiator and it's the unsafe part. Low ROI.

### Verdict
```
CANDIDATE: AcmeLint MCP (MCP server, MIT, free + hosted auto-fix)
SECURITY: 2 flags — sends code to a hosted API; registers an auto-run PostToolUse hook
DUPLICATES: yes — local slop-scan + code-review (read-only, no exfiltration)
VALUE: low — only the unsafe hosted mode is differentiated
VERDICT: 🔴 REJECT — exfiltration + auto-hook for marginal, duplicated value
STEP: record the reject row; if a fully-local fork without hooks appears, re-vet
```

### Registry row
`| 2026-01-25 | AcmeLint MCP | MCP | 🔴 | code exfiltration to hosted API + auto-run hook, duplicates local tools | reject; re-vet only a local no-hook fork |`
