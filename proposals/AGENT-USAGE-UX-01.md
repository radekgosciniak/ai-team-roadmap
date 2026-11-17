# AGENT-USAGE-UX-01 — Unified Claude / Codex Usage & Account Status UX

**Status:** UX reference and evaluation backlog / concept approved; implementation NOT authorized  
**Added:** 2026-11-17  
**Priority:** After Trusted CLI (TC012/G1/TC-3), E7 and account/identity stabilization  
**Reference:** Operator-provided screenshot of newer Orca usage/account UI (2026-11-17). **UX inspiration only**; screenshot does not prove Orca backend architecture, access permissions or official quota source.

## Product intent

Give Command Center an at-a-glance, unified overview of **Claude Code and Codex account usage** and separate details per selected managed account without sacrificing identity verification, least privilege or existing account-binding rules.

## UX proposal

1. **Compact / Detailed modes:** compact summary for daily use; detailed view for account-level window breakdowns and history. Preserve selected mode within approved UI settings if appropriate.
2. **Both providers on one surface:** Claude and Codex usage bars/status, last verified/read timestamp and next reset only when reliably supplied by a provider/authorized adapter. Avoid comparing unlike limits as if they were equivalent.
3. **5h/weekly windows as supported:** clear Used vs Remaining, percent basis and precise reset period, including timezone; no invented percentages or reset times. Show unavailable/unsupported distinctly.
4. **Multi-account chooser:** highlight selected managed identity, mark each account's own usage/read status. `Not checked` means unqueried, *not* zero usage or no quota. Distinguish stale, not connected, failed, unsupported and fresh.
5. **Compact footer/status indicators:** optional Claude/Codex summaries accessible without leaving Runs/Interactive Chat, with timestamp/tooltip and responsive layout.
6. **Usage details/history:** chart historical observations only where legally and technically accessible, with provenance, data retention and clear labels. Distinguish quotas/rate-limits from billed token spend and inferred costs (TOKEN-OPT-01).
7. **Explicit refresh:** per-account/provider on-demand refresh with throttling and error states; never trigger silent login/account switching or credential reads.
8. **Interaction integration:** links to existing managed account details, Account Binding/Identity Probe and future INTERACTIVE-AGENT-CHAT-01. No parallel account registry.
9. **Accessibility:** keyboard navigation, color-independent status, visible labels and screen-reader percentages, compact and detailed Polish/English UI.
10. **Design system:** adopt clear spacing, restrained progress bars, one active-account selection, and information density without copying Orca code/assets.

## Data and security contract (must be validated)

- No assumption that Claude or Codex officially expose all quota/usage windows from local CLI or documented API; assess supported provider contracts before choosing a source. Never infer a per-account usage amount from another account/session.
- UI displays **source, timestamp, freshness, provider/account identity and confidence** for every observation; `Unknown` and `Not checked` do not mean zero. Zero is displayed only when explicitly verified.
- Separate login identity and quota state from Execution eligibility: available limit **does not** bypass EXPECTED/DECLARED/VERIFIED identity, Trusted CLI, native consent or sandbox controls.
- No credential scraping, token copying, bypassing access controls or reliance on undocumented private endpoints without explicit independent authorization.
- Local cache, if approved, must be account-isolated and respect retention/privacy and secrets policy. Do not leak email addresses or usage data across managed profiles.
- Refresh operations and error handling cannot modify selected account, managed profile, process launch, execution permissions, or provider session state.
- UI states required: fresh, stale, not checked, loading, unavailable, unsupported, authentication required, error, rate-limited.

## Evaluation gates (no implementation authorization)

1. **AUX-01 Contract discovery:** verify reliable, policy-compliant Claude Code/Codex usage sources per account and supported versions, windows, reset metadata and rate limits. Record UNKNOWN for unsupported fields.
2. **AUX-02 Prototype:** design non-executing compact/detailed layouts and status bar using synthetic fixture data; review with existing Command Center UX constraints.
3. **AUX-03 Account binding/privacy:** prove displayed usage matches account identity and cannot leak between accounts, providers or worktrees; cover refresh, logout, stale and offline transitions.
4. **AUX-04 Data semantics:** verify correct Used/Remaining labeling, percent math, timezone/reset, absent data, expiry, and cost-vs-quota distinctions. Never claim exact provider data from estimates.
5. **AUX-05 Integration acceptance:** separately authorized integration into Account Management / Identity Probe and Runs/Sessions, with native negative tests and operator approval before rollout.

## Dependencies and relations

- [INTERACTIVE-AGENT-CHAT-01](INTERACTIVE-AGENT-CHAT-01.md) — in-context interactive session header/footer and managed-account picker.
- [TOKEN-OPT-01](TOKEN-OPT-01-agent-experience-token-optimization.md) — token/cache/cost instrumentation is distinct from provider quota windows.
- Existing Accounts / Account Binding / Identity Probe (EXPECTED / DECLARED / VERIFIED); future PILR Direct Task workflows.
- Preserve **Trusted CLI / G1 → TC-3/G2 → E7** as higher-priority execution/security work.

**Decision:** ADAPT UI patterns / NO MIGRATION. Do not install Orca, scrape provider accounts, execute CLIs, change live profiles, or implement any service based solely on this proposal.
