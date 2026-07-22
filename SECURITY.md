# Security Policy

## Reporting a Vulnerability

If a skill, rule, or agent in this repo describes a behavior that's unsafe, bypasses a stated guard, or could be abused (prompt injection, trigger abuse, excessive agency), please open an [issue](https://github.com/Sanexxxx777/curated-claude-code/issues), or for sensitive reports contact **Aleksandr Shulgin** directly via [@Aleksandr_NFA](https://t.me/Aleksandr_NFA) rather than filing publicly.

## Scope

This is a **configuration harness for an AI coding agent** — Markdown skills, rules, and agent definitions meant to be copied into `~/.claude/`. It does not run a service and does not store secrets: there are no API keys, tokens, or credentials committed here, and none should ever be added.

When you adapt this harness for your own setup, you are responsible for keeping your own private data (infrastructure details, credentials, project specifics) out of anything you commit back or share publicly — the templates here are written generically on purpose.

## Disclosure

This is a personal open-source project with no fixed SLA, but reports are reviewed as soon as possible.
