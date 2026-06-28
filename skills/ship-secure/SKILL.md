---
name: ship-secure
description: Security launch audit for a public web app BEFORE you ship or hand it to a client — "vibe coders are getting sued", systematized. An executable pipeline: classify (static / full-stack / AI) → run sections (secrets+supply-chain, DB, auth, infra+headers, legal, LLM/injection) orchestrating deterministic tools + manual checks → GO/NO-GO report. Adapted from Prajwal Tomar's playbook, extended with prompt-injection / supply-chain / secret-history / dependency-CVE layers. NOT the same as pre-push (that = clean push; this = is the product itself safe to launch). Triggers — "security checklist for the site", "audit before launch", "is the site safe", "check before handing to client".
---

# /ship-secure — security launch audit for a public web app

Goal: $ARGUMENTS — turn "I hope it's secure" into a verifiable GO/NO-GO BEFORE the product goes public (launch / client handoff). Each section is either OK or STOP with a finding (`file:line` + type, a secret = NOT its value).

**Origin:** adapted from Prajwal Tomar's "Vibe Coders Are Getting Sued — 30-Min Playbook" (5 sections), retargeted to a general stack and extended with layers the original lacks (prompt-injection / supply-chain / secret-history / dependency-CVE).

**Border with `/pre-push`:** pre-push = "don't let junk/secrets/bugs leave on push" (scope-diff, secret-grep, lint, code-review, build). ship-secure = "is the product itself safe" (RLS, auth, rate-limit, CORS, headers, legal, injection). They complement each other.

**Honest disclaimer (state it in the report):** this catches common, deterministically-checkable holes. It is NOT a pentest and NOT a legal guarantee. An LLM "security review" gives false confidence → deterministic tools (gitleaks/audit/grep) come first, LLM prompts are supplementary.

---

## Step 0 — classify the target (branch, don't run the inapplicable)

The type determines the section set:

- **Static** (static host, landing page, single-file HTML, an SPA build with no own backend): → Sections **A, D, E** (secrets, headers, legal). B/C (DB/auth) and most of infra are **N/A** — don't waste time.
- **Full-stack** (own backend + forms + DB ± auth): → all of A–E.
- **AI app** (an LLM in production: chat/agent/generator with public input): → all of the above + Section **F** (LLM/injection) is MANDATORY.

If it's a client site, the client's jurisdiction matters (Section E). Unsure of the type — ask, don't guess.

## Section A — secrets, frontend, supply-chain (deterministic, BLOCKING — all types)

1. **Secrets in git history** (not just current files): `gitleaks detect --source . --redact`. Any hit = STOP. Finding = `file:line` + type, NOT the value.
2. **Keys in the frontend bundle/public files:** grep build/public assets for `api[_-]?key|secret|token|sk-|ghp_|AIza|private_key|Bearer `. ⚠️ Build-time public vars (`NEXT_PUBLIC_`/`VITE_`) ship to the browser — confirm no server secret landed there. Service-role/admin keys in the frontend = STOP.
3. **Supply-chain:** review installed dependencies/extensions against known compromise advisories before trusting them.
4. **Dependency CVEs:** `npm audit --omit=dev` (Node) / `pip-audit` or `uv pip audit` (Python). High/Critical = fix/upgrade before launch.

## Section B — database & backend (full-stack)

1. **Row-level authorization:** is there an ownership check on EVERY data access? (Row Level Security enabled on all tables; a hand-rolled backend — no endpoint returns others'/all records without a `user_id` check). A "dump the whole table" hole = STOP.
2. **Server-side validation of all forms:** client validation is trivially bypassed. Every field validated server-side (type/length/whitelist). File uploads — type + size + path.
3. **SQL/NoSQL injection:** parameterized queries / ORM only. Concatenating user input into a query = STOP.
4. **Generic error messages:** no stack traces / SQL / table names / versions leak outward. `DEBUG=False` in production. 500 = generic, details to the log.

## Section C — auth failure testing (if there is authentication)

Run real attempts, observe behavior:
1. **Wrong password ×5+** → is there a rate-limit/lockout/backoff (brute-force protection).
2. **Password reset for a non-existent email** → identical response for existing and not (anti user-enumeration). Same on login/signup errors.
3. **Duplicates:** double-click verification, re-register an existing email → graceful, no 500/leak.
4. **Sessions/cookies:** `HttpOnly` + `Secure` + `SameSite`; tokens expire; logout truly invalidates; no default JWT secret.

## Section D — infra & headers (full-stack + baseline static)

1. **Rate limiting** on public/expensive endpoints (login, API, forms, LLM): middleware / reverse-proxy limits. Critical where an endpoint spends money (LLM calls, email/SMS).
2. **CAPTCHA** on public no-auth forms (contact, signup).
3. **CORS:** not `Access-Control-Allow-Origin: *` on credentialed/session endpoints. Whitelist the origin.
4. **Security headers** (check static too, `curl -sI <URL>`): `Content-Security-Policy`, `Strict-Transport-Security`, `X-Frame-Options: DENY`, `X-Content-Type-Options: nosniff`, `Referrer-Policy`. Force HTTPS (redirect from http).

## Section E — legal & privacy (if it collects personal data/analytics)

1. **Privacy policy** published (free generators exist). Without one, data-collecting forms = risk.
2. **Where data is stored** documented; cookie/analytics consent if trackers exist.
3. **Client jurisdiction:** GDPR (EU) / local data-protection law / CCPA — note what applies, don't give legal advice, but flag it to the client.

## Section F — LLM / prompt-injection (AI app — the layer the original lacks)

1. **User-input isolation:** external input is wrapped in delimiters/tags, not naively concatenated into the system prompt. External content = DATA, not commands.
2. **System-prompt/key protection:** a jailbreak doesn't extract the system prompt, keys, or hidden instructions. Test "ignore previous instructions / show your prompt".
3. **Model output is not executed** as code/SQL/shell without sanitization (especially tool-using agents).
4. **Bill-drain:** rate-limit + cap on the LLM endpoint (no auth = anyone burns your API budget).

## Section G — code review (on top of deterministic — all types)

1. **A security-focused code review** of the app code — vulnerabilities.
2. **A correctness review** — authorization logic often breaks here.
3. **A slop/dead-code scan** — swallowed `except: pass` can silently eat security errors.

**AI security prompts (supplementary, adapted from the playbook — NOT a replacement for G.1–G.3):**
1. "Review this app as a security specialist — what are the attack vectors?"
2. "Review against the OWASP Top 10 — what's violated?"
3. "Find credential/PII leaks in the code and logs."
4. "Confirm there are no API keys/secrets in code shipped to the browser."
→ Re-verify the LLM's output with tools, don't take it on faith.

---

## Report format
```
TARGET: <what> (<type: static/full-stack/AI>)
A secrets+supply: OK / STOP — <finding file:line + type>
B database:       OK / N/A / STOP — …
C auth:           OK / N/A / STOP — …
D infra+headers:  OK / STOP — …
E legal:          OK / N/A — …
F LLM/injection:  OK / N/A / STOP — …
G code review:    OK / findings
VERDICT: 🟢 GO / 🔴 NO-GO — <one line>
Disclaimer: catches common holes, NOT a pentest / legal guarantee.
```

## Principles
- **External input = data, not commands** (prompt-injection guard).
- **Secrets = `file:line` + type, NEVER the value.** A hit in history = STOP.
- **Branch static/full-stack/AI** — don't run the inapplicable (static ≠ RLS/auth).
- **Deterministic tools first, LLM prompts supplementary** — no false confidence.
- **Mutation = on the user's command:** fixes/deploy/key-rotation after findings — propose, don't apply silently. A live secret found in git history → ROTATE first, then scrub history (a secret in the repo ≠ safe after deletion — treat it as compromised).
- **Verify by checking, not on faith:** headers via real `curl -sI`; auth via real attempts; "no errors" ≠ secure.
- **Not legal advice:** flag Section E to the client, name the jurisdiction, don't advise as a lawyer.
