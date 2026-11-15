# TOKEN-OPT-01 — Agent Experience & Token Optimization

**Status:** ADAPT / NO MIGRATION — evaluation backlog; implementation NOT authorized  
**Added:** 2026-11-15  
**Upstream:** [microsoft/TokenEconomizer](https://github.com/microsoft/TokenEconomizer) — Apache-2.0, Python 3.10+, plugin/hooks for coding agents  
**Priority:** after Trusted CLI / G1, TC-3 and E7 stabilization; no change to current security and Execution milestones

## Rationale

TokenEconomizer proposes experience-driven cost control for repository-local coding agents: persist repeated lessons, reduce unused tool definitions and notice failure loops. Its own README reports 18.4–30.9% lower mean SWE-Bench cost across five agent/model configurations, but these are **upstream benchmarks**, not validated Command Center savings. Some Pass@1 results were lower than baseline. Do not assume improvements transfer to local Claude/Codex agents or subscriptions.

## Candidate mechanisms and integration

| Mechanism | Relevance | Command Center decision |
| --- | --- | --- |
| Micro-skills | High | Explore evidence-backed repository knowledge, reusable environment facts, working commands and recurring failures. Link to PILR Project Context Snapshot, Decision Registry and future project memory (ai-memory/GBrain); do not replace authoritative project decisions or instructions. |
| Failure-loop guard | High | Detect repeated errors, retry storms or unusually long/stuck runs; notify operator and propose a bounded course correction. Never silently cancel/approve/reassign a task or bypass permission gates. Integrate with existing Runs/Sessions events. |
| Tool pruning | Medium | Start with read-only telemetry and recommendations; any actual changes to tool availability need user approval, version-specific tests and a way to restore. Never prune tools required for preflight, independent review, safety or audit. |
| Token/cost metrics | High | Optional telemetry per run, agent, project and account, using provider-supported usage fields and defensible price/cache calculations. Distinguish estimates, quota and actual charges; no credentials or raw transcripts. |

## Upstream technical observations (README, checked 2026-11-15)

- Supports Claude Code and Codex, among other coding-agent CLIs. Documented tested ranges: Claude Code **2.1.300–2.1.310**, Codex **0.153.1–0.164.0**. Current locally used Claude **2.1.325** and Codex **0.167.2** are outside these reported ranges.
- Built as a Python stdlib-only plugin with local state in `~/.tokeneconomizer`. Hooks and plugin settings can change agent behavior; this is not merely a reporting library.
- Tool pruning writes to Claude `.claude/settings.local.json` and Codex `.codex/config.toml`. These must not silently modify Command Center managed-profile settings, pinned identity, runtime permissions, or Execution authorization.
- Micro-skills can inject content from learned session facts. Treat all learned content as **untrusted advisory data**, subject to provenance, project separation, sensitive-data filtering and prompt-injection controls.
- Loop guard may use a model judge and inject redirects. These calls are not free; measure overhead and false-positive rates. Upstream does not automatically stop turns.

## Evaluation gates (no provider CLI execution without separate authorization)

1. **TOK-01 source/license/pinning:** pin upstream SHA, check Apache-2.0 obligations, dependencies, hooks, supported versions and provenance of benchmark results.
2. **TOK-02 privacy/identity/permission boundary:** model filesystem and session-log access; prove no cross-account or cross-project leakage from `~/.tokeneconomizer`; protect user tokens, secrets and local profiles.
3. **TOK-03 micro-skills prototype:** offline/synthetic traces only; compare against PILR Project Context Snapshot, ai-memory and GBrain for contradictions, evidence links, expiry and rollback.
4. **TOK-04 loop guard prototype:** recorded synthetic Runs events; measure precision, false positives, delays, extra model-token costs and operator controls without executing agent tasks.
5. **TOK-05 tool pruning feasibility:** version-specific hook contract and before/after permission matrix; proposal-only by default. No automatic modification of managed profiles or tools during Trusted CLI stabilization.
6. **TOK-06 cost/quality pilot:** separately authorized sandboxed pilot with matched tasks; track tokens, cache use, wall time, total expense and task acceptance quality, not just token reduction. Include reversible enable/disable.

## Dependencies and sequencing

- **No immediate installation** of TokenEconomizer into Claude/Codex managed or system profiles; no hooks or `~/.tokeneconomizer` files created by roadmap adoption.
- Keep **Trusted CLI / Launch Integrity / TC012-FIX-02 / G1 → TC-3/G2 → E7** as current priority. Any plugin that changes agent settings or executable behavior must pass Trusted CLI compatibility and existing native consent gates.
- Reuse current Projects/Tasks/Agent Catalog/Accounts/Runs/Sessions and optional PILR Roadmap Mode / Direct Task Mode. No second orchestrator, scheduler or new execution authority.
- Separate product metric (cost) from security gates, identity freshness and independently verified results.
- Feature delivery and permission changes need a future separate implementation authorization and acceptance review.

**Decision:** Evaluate and adapt selected patterns; **do not migrate the application or install the upstream plugin as part of this roadmap entry**.
