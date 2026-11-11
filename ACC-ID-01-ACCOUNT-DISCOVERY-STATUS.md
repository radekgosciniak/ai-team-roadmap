# ACC-ID-01 — account discovery checkpoint (2026-11-12)

**Implementation repository:** [AI Development Team](https://github.com/radekgosciniak/AI-Development-Team)  
**Merged delivery:** [PR #53 — feat(accounts): add Claude and Codex identity discovery](https://github.com/radekgosciniak/AI-Development-Team/pull/53)  
**Feature commit:** `74e986446ed03080b398cd09c08de354c4d458aa`  
**Merge commit on remote main:** `10e9f6f90d714b5d6140ac9014b80fb322276a81`  
**Scope:** 32 files in PR; local reports and evidence remain outside Git.

## What is implemented and merged

| Increment | Status | Evidence / scope |
|---|---|---|
| F-1 / F-2 | MERGED; accepted with conditions | EXPECTED, identity fingerprint / operator attestation; strict distinction from DECLARED and VERIFIED |
| F-6a Lite | MERGED; native functional PASS | Command Center managed profile preparation, directory qualification, textual login instructions; N08 long-path layout fix |
| F-3 Claude Managed Discovery | MERGED; synthetic native functional PASS | `oauthAccount` from managed-profile state; never reads `.credentials.json`; N07 card overflow fix |
| F-3 Claude System Discovery | MERGED; real system native PASS | Windows default profile recognized as DECLARED without manual account creation or re-login |
| UX-02 | MERGED; native functional PASS | Full DECLARED e-mail in local UI, switch to mask; EXPECTED remains masked; no full e-mail in logs or durable store |
| F-4 Codex System / Managed Discovery | MERGED; real system native PASS / managed synthetic-only | `auth.json` ID-token-derived account metadata, no execution of CLI; UNKNOWN on unsupported auth stores/modes |

**Native acceptance:** Both default Claude and Codex profiles from the current Windows user were recognized as **DECLARED** in Command Center; e-mail display/masking and cards validated in EN/PL. Do **not** interpret this as an authorized Execution path or account verified for a run.

**Tests reported in [PR #53](https://github.com/radekgosciniak/AI-Development-Team/pull/53):** Rust identity 105/105; Node 441 pass / 2 skipped; TypeScript, Vite and native Cargo build PASS; 36/36 card layout cases without overflow. Full Rust suite is **not green**: 365 pass / 36 known Recovery failures / 6 ignored (same baseline). Native managed Codex login remains untested.

## Adopted product decisions

1. **Independent of ORCA:** ORCA is a reference for implementation patterns only. Do not read ORCA's accounts, session state or configuration as an integration dependency.
2. **System default auto-discovery:** Show the current user's default profile for Claude and Codex without manual onboarding. Separate candidates for CLI binaries from accounts.
3. **Managed accounts:** Additional isolated profiles are optional and intended for multiple accounts. Do not claim multi-account browser onboarding is implemented yet.
4. **Proof levels:** EXPECTED = operator assignment; DECLARED = local-file observation only; VERIFIED = future Execution receipt (`Ticket → Receipt → judge`). No automatic MATCH from same e-mail / unmatched workspace.
5. **UI privacy:** full declared e-mail allowed in local UI with a mask toggle; do not persist or log e-mails, tokens or account/workspace identifiers. No credential copying or automatic re-login.
6. **No automatic account launch:** Existing system profile is discovered, **not yet connected to working agent execution**.

## Immediate next work (not started)

### Account Login Manager — priority 1

- Native **Add Claude account / Add Codex account** affordances and managed per-account profile directories.
- Run vendor-supported login flow; operator completes consent/login in browser as appropriate for the CLI. No direct password/token collection, no copying system credentials, and no overwrite of other accounts.
- Show account discovery and sign-in state distinctly, handle cancel/timeout/retry and cleanup without deleting credentials unexpectedly.
- Start with a small functional increment, reuse F-6a and provider patterns, do not port ORCA wholesale.
- Any actual CLI execution for login must have a separately reviewed and authorized boundary; do not silently reuse Discovery permission.

### Account Assignment and Execution — later

- Select managed/system profile for an agent and bind account to a task.
- Integrate with existing Agent Catalog, Tasks, and later launcher and Execution receipt F-9.
- Execution-side verification and a launcher's safety controls are separate work. **NOT IMPLEMENTED** as of this checkpoint.

### Backlog / limitations

- `UI-BUG-OVERFLOW-01` **P2**: app shell permits vertical scrolling below application into blank white background; investigate scroll root, viewport height and layout overflow.
- Narrow viewport (~520px): some full-email labels wrap mid-word and header/overall horizontal scrolling pre-existed; P3 UI issue.
- Managed Codex profile sign-in not validated natively; tests only synthetic.
- Discovery can show stale DECLARED values; `auto`/keyring and unsupported modes result in UNKNOWN. No proof of active sign-in.
- Accepted F-1/F-2 and F-6a minor issues are not silently closed.
- 36 existing Rust Recovery failures remain a separate known issue; **not PASS**.
- Previous AGCAT, CC-INT, P3-1A1, Stage 1A.2 and P7 workstreams retain their separately recorded status; this milestone does not imply their acceptance.

## Git hygiene / local state

PR #53 is **merged and closed remotely**. Updating the **local** `main` is a separate follow-up task `LOCAL-MAIN-SYNC-01`: fetch, confirm a clean local `main`, then fast-forward only to `origin/main`; preserve unrelated worktrees and uncommitted changes. **Local sync is not yet confirmed by this document.**

**Roadmap principle:** ADAPT / NO MIGRATION. Prioritize functioning account workflows over another broad security redesign; preserve essential token/privacy boundaries and the distinct Execution authorization.
