# Detective mindset — reading logs and data

When analyzing logs, events, transactions, or any structured trace of what a system did — don't stop at the first plausible explanation. The obvious read is usually the one someone (a bug, a bad actor, a misconfigured default) wants you to land on.

## Checklist

1. **Numeric coincidences.** The same value recurring across N of M cases, matching timestamps across otherwise-independent events, suspiciously round numbers — these are rarely chance. They're anchors, defaults, or deliberate markers.
2. **Distribution anomalies.** A long tail on latency, a bimodal shape (two populations hiding in one metric), a hard cutoff at a round number — each points to a specific mechanism (a retry storm, two distinct code paths, a rate limit or cap) rather than "normal variance."
3. **Cross-source correlation.** Line up your log against an independent source at the same timestamp (another system's log, an external feed, a second observer). Confirms whether an anomaly is local to you or shared upstream — a very different fix in each case.
4. **Ghost/ephemeral state.** Something existed, then was gone exactly when acted on. Compare "when it was seen" against "when it was acted on" — the gap is where the interesting behavior lives.
5. **Repeat actors.** The same identity/session/IP recurring is a signal, not noise — profile it by time and size before dismissing it as background traffic.
6. **Config vs. code divergence.** The same parameter defined in two places with different values, or a comparison that silently disagrees with the config (e.g. a float compared without rounding) — a classic source of "impossible" behavior.

## Method
- **Quantify, don't gesture.** "7 of 10 cases" beats "often" — a vague claim can't be checked or falsified later.
- **List alternative hypotheses before picking one:** randomness, a config mismatch, a bug, or deliberate behavior. Rule three out before committing to the fourth.
- **Look for the mechanism.** "How is this physically possible" is a stronger question than "what does this look like" — a story without a mechanism is still a guess.
- **Check reproducibility.** Does the same pattern show up earlier in the history, or is this one instance being over-read?

## Don't
- Don't restart/patch and move on without analyzing what actually happened.
- Don't attribute an anomaly to "randomness" without checking the distribution first.
- Don't skip over numbers that look familiar — recurring, "boring" values are often exactly where the pattern is.
- Don't trust a log at face value when an independent source is available to cross-check it.
