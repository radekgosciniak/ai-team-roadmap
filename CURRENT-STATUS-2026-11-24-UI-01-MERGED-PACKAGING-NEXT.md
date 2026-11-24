# AI Development Team — S4-B2-UI-01 MERGED; packaging next

**2026-11-24** · Living Roadmap / Trusted CLI / synthetic terminal

## Confirmed checkpoint

- [PR #82](https://github.com/radekgosciniak/AI-Development-Team/pull/82) **MERGED** into `main`; reviewed head `705fefb0f38776b05a4e75a2665336ddae03fb9d`, merge commit `0f5cbf4f24f40f2f01394c83f43d88f4664fccdc`.
- Scope: React synthetic terminal UI in Tasks and Agents, incremental text output, bounded reads, per-session unsent drafts, controlled synthetic echo input, and explicit incomplete-output/uncertain-delivery warnings; only `tc4-synthetic`. Standard release bundle remains isolated from terminal features.
- Independent Claude acceptance of FIX-08: **READY FOR MERGE WITH FOLLOW-UP GATES; D1 PASS, D2 PASS**. FAST 264/264, frontend INTEGRATION 985 PASS / 0 FAIL / 11 SKIP, ACL-1 2/2, TypeScript PASS, Edge React StrictMode with mock IPC PASS. These results do **not** qualify the latest changes as native WebView2 + ConPTY tested.
- Previous [PR #81](https://github.com/radekgosciniak/AI-Development-Team/pull/81) native P2 smoke: 73 PASS / 0 FAIL / 1 NOT VERIFIED, before later UI changes. Do not reuse as native acceptance of FIX-08.

## NEXT — S4-B2-UI-PACKAGING-01

Validate that synthetic packaging **always** uses freshly built `vite build --mode tc4-synthetic` assets, rather than accidentally embedding a stale standard Vite dist. Preserve P1 embedded SHA-256 pinning, immutable versions, ACTIVE/HISTORY/PENDING journals, ACL-1, TC-0 and production isolation. Implement only minimal build/packaging glue with isolated test evidence; require independent review before merge. No real-agent execution. Use separate worktree, preserve other agent resources. FULL tests not authorized.

Suggested acceptance:
1. Rebuild synthetic assets in a clean controlled staging directory immediately before the package/embed step; fail closed on stale/missing/mismatched mode artifacts.
2. Validate actual package provenance of embedded frontend assets and their hash; do not infer from filenames or environment variables alone.
3. Negative controls: intentionally stale standard bundle, stale synthetic bundle, missing dist, mode mismatch, modified assets after hash generation, concurrent build attempts.
4. Verify standard build remains free of `terminal_session`, TC4 grants and terminal UI; validate synthetic package retains its gated terminal surface.
5. Test in isolated profile/data directories only; do not touch shared real-profile state.

## Open gates and limitations

- **S4-B2-UI-PACKAGING-01 OPEN / NEXT**. Not closed by PR #82.
- **SEC-WEBVIEW2-01 OPEN**: production WebView2 may honor `WEBVIEW2_ADDITIONAL_BROWSER_ARGUMENTS`; unauthorized CDP execution under main permissions remains an independent security risk before real agents.
- **Origin: null NOT VERIFIED**; previous native smoke timed out three times. No implicit PASS.
- **G-ENV OPEN; G1 BLOCKED; D-3 DEFERRED.** Trust Broker remains custody-only, TC-0 synthetic-only, no real Claude/Codex execution.
- **Latest native UI smoke NOT RUN** (no safely isolated shared-profile alternative); FULL NOT RUN.
- **FIX-08 low-priority follow-ups**: P3-1 denial not visible in input bar after returning to a session; P3-2 unchanged unconfirmed draft disappears after switch/remount while warning and send restriction remain; P3-3 draft remains in volatile store memory as long as RUNNING. No secret persistence claim.
- No backend idempotency key or IPC cancellation; permanently unresolved IPC can exhaust slots.

## Local sync caveat (2026-11-24)

Operator's existing `C:\AI-Development-Team` checkout has two modified tracked files and numerous untracked reports/docs that overlap Git paths. `git switch main` and `git pull --ff-only` **aborted safely**. `origin/main` has been fetched to `0f5cbf4`, but the current checkout remained at `1ff433b`. Do not reset, clean, stash or overwrite user work. A separate read-only/detached `git worktree add --detach C:\AI-Development-Team-main-sync origin/main` is a safe working copy; verify worktree listing before attempting to move a locally checked-out `main` branch.

## References

- App merge: https://github.com/radekgosciniak/AI-Development-Team/commit/0f5cbf4f24f40f2f01394c83f43d88f4664fccdc
- FIX-08 acceptance report (operator local): `C:\AI-Development-Team\reports\TRUSTED-CLI\S4-B2-UI-01-FIX-08-FINAL-ACCEPTANCE-CLAUDE.md`
- FIX-08 implementation report (operator local): `C:\AI-Development-Team\reports\TRUSTED-CLI\S4-B2-UI-01-FIX-08-RESULT-CODEX.md`
- Previous historical checkpoint: [P2 merged / UI design next](CURRENT-STATUS-2026-11-23-P2-MERGED-UI-DESIGN-NEXT.md).
