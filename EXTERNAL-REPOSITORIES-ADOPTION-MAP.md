# External repositories — adoption map for AI Development Team

**Status:** architecture reference / planned evaluation, **NOT IMPLEMENTATION AUTHORIZATION**  
**Last updated:** 2026-11-22 (ECC candidate registered; historical checkpoints preserved)  
**Owner:** AI Development Team architecture roadmap  
**Canonical public roadmap:** [AI Team Roadmap](https://github.com/radekgosciniak/ai-team-roadmap)  
**Project implementation:** [AI Development Team](https://github.com/radekgosciniak/AI-Development-Team)

This document is a durable record of which externally reviewed projects may be useful, **when** to revisit them and **what must be verified** before implementation. It is a decision register, not a dependency list or permission to copy code.


## 2026-11-22 — ECC (Everything Claude Code): high-priority targeted adoption review

- **Upstream:** [affaan-m/ECC](https://github.com/affaan-m/ECC), snapshot [`4adeee6732a12d1540f49a4d50a38ce8c97f9a53`](https://github.com/affaan-m/ECC/tree/4adeee6732a12d1540f49a4d50a38ce8c97f9a53) (2026-11-20); upstream package `ecc-universal` 2.2.3; **MIT**, check notices for any later source-code adoption.
- **Priority:** **HIGH — TARGETED ADOPTION REVIEW / BACKLOG**, **NOT INSTALLED / NOT INTEGRATED / NOT IMPLEMENTATION AUTHORIZATION**. The source is a framework for multi-agent engineering workflows, hooks, rules, skills and review; it is **not** an NTFS transaction or Windows packaging library.
- **P7 / Project Intelligence & Living Roadmap — ADAPT candidate:** [`scripts/worktree-lifecycle.js`](https://github.com/affaan-m/ECC/blob/4adeee6732a12d1540f49a4d50a38ce8c97f9a53/scripts/worktree-lifecycle.js) categorizes worktrees as dirty/merge-ready/conflict/merged/stale/idle and proposes a non-destructive cleanup plan; examine the underlying `scripts/lib/worktree-lifecycle/` before adoption. Map to our isolated Claude implementer / independent Codex reviewer and protect dirty/unmerged worktrees.
- **P7 / GitHub task coordination — ADAPT candidate:** [`scripts/github-coordination.js`](https://github.com/affaan-m/ECC/blob/4adeee6732a12d1540f49a4d50a38ce8c97f9a53/scripts/github-coordination.js) offers claim, sync, validate, publish, review, unblock, decompose; compare against our roadmap-governed/direct-task dual mode, operator approval, GitHub PR state and idempotent handoffs. No auto-merge or unapproved task execution.
- **Quality / independent acceptance — ADAPT candidate:** [`scripts/eval-harness.js`](https://github.com/affaan-m/ECC/blob/4adeee6732a12d1540f49a4d50a38ce8c97f9a53/scripts/eval-harness.js) includes capsule/receipt verification and gate-report conventions. Its CLI explicitly says isolated gate execution is unavailable (`gate.isolation_required`): **do not** interpret this as proof of sandbox isolation. Evaluate read-only evidence capsules and independently verified receipts, not replacement of our Codex review gates.
- **Skills / role templates — SELECTIVE ADOPT candidate:** selectively review `agents/`, `skills/`, `.claude/` and `.codex/` for roles, TDD, security and review practices. Avoid bulk import of hundreds of rules, hooks or agents; determine lifecycle and license per exact file.
- **Security / observability — REVIEW ONLY:** AgentShield, GateGuard, scripts for operator readiness and dashboards may inform future architecture, but hooks and auto-actions can conflict with Trust Broker, Tauri ACL and native consent. Audit real code, side effects, data handling and bypass paths before consideration.
- **S4-B2-P1 / PR #80 FIX-03:** **NO CHANGE**. Existing journal short-write and indeterminate-fsync blockers remain the priority; ECC cannot be used as an unverified fix. No install or dependency is permitted in that PR.
- **S4-B2-UI / P8:** consider dashboard/control-plane UX concepts during the previously mandated AionUi/Orca/OpenWorker comparison; any ECC inspiration requires source-level verification.
- **Acceptance prerequisites:** pinned source+license+transitive deps; Windows 11/Node/Tauri compatibility; security boundaries; dry-run; no automatic shell execution/merges; isolation between implementer and reviewer; evidence and rollback; comparative demonstration of an advantage over already implemented functionality.
- **Decision:** targeted **ADAPT / SELECTIVE ADOPT / REVIEW** only; **NO MIGRATION** and **NO automatic install** (neither `npx ecc-universal setup` nor global/local Claude/Codex plugin without separate authorization). Preserve G-ENV OPEN, G1 BLOCKED, D-3 DEFERRED.

## 2026-11-12 — ACC-ID-01 provider account discovery MERGED

- [PR #53](https://github.com/radekgosciniak/AI-Development-Team/pull/53) merged into remote `main` (merge `10e9f6f90d714b5d6140ac9014b80fb322276a81`).
- Claude and Codex **system default account discovery now work natively** as DECLARED; managed profile discovery is implemented, but managed Codex real-login acceptance remains pending. F-1/F-2, F-6a, F-3, UX-02 and F-4 are included in the merged delivery.
- **ORCA is a pattern reference, not a runtime dependency**: do not scan ORCA profiles or synchronize ORCA account sessions. No Account Login Manager / browser onboarding, account-to-agent Execution binding or VERIFIED receipt has been delivered by this PR.
- **NEXT:** safe local `main` fast-forward (not independently confirmed), then Account Login Manager (separate authorization for actual provider login/CLI launch), Account Assignment and Execution/F-9.
- **BACKLOG:** UI-BUG-OVERFLOW-01 blank white scroll area (P2); narrow 520px UI (P3), 36 pre-existing Rust Recovery failures and accepted account-discovery MINOR items.
- **Canonical checkpoint:** [ACC-ID-01 account discovery status](ACC-ID-01-ACCOUNT-DISCOVERY-STATUS.md). This milestone updates only the account-discovery track; AGCAT/CC-INT/Recovery/Stage 1A.2/P7 statuses below remain governed by their own evidence.

## 2026-11-09 — AGCAT post-merge checkpoint and integration boundary

- **Implementation repository:** [PR #51](https://github.com/radekgosciniak/AI-Development-Team/pull/51) **MERGED**; merge SHA `56c530ca5bcccc79b0a65f22e77e11b125eafcea`; AGCAT-UI-01 head `deeed3b4d5428a65e44b91bf0f0907ad5d01dfb9`. Local `main` was updated to the same merge commit; the active working directory remains on its separate feature branch, with uncommitted changes preserved.
- **Functional evidence:** original AGCAT-2 native smoke **12/12 PASS**; own Win32 dark/light approval dialog, visual catalog editor and independent review; F-01 native retest **T1–T5 PASS**, four previously ignored environment-sensitive Rust tests **4/4 PASS**. These results do **not** constitute full workflow E2E.
- **Known security scope:** high-assurance child-process trace W-3 **BLOCKED**; custody C **NOT CLOSED**. These belong to a separate qualification track and were documented at merge, not silently closed.
- **Functional boundary:** AGCAT-3 **NOT STARTED**. The PR does not integrate verified account selection, agent membership in Tasks, real CLI execution, durable live sessions or automatic agent handoffs.
- **Next checkpoint:** **CC-INT-01** — code-grounded integration map and automated tests on synthetic data covering Accounts → Agent Catalog → Tasks → Execution → Sessions; classify each edge as IMPLEMENTED / PARTIAL / MOCK / MISSING. Follow with separately authorized minimal fixes and AGCAT-3, not unplanned broad migrations. No new work started as of this checkpoint.
- **Operational hygiene:** PR #51 merged without required GitHub Actions checks because repository had zero workflows and `main` lacked branch protection at the time; CI/ruleset hardening is a future separate task. Do not infer CI PASS. Preserve all unrelated worktrees.

## Reviewed source snapshots

| Source | Pinned version considered | Role |
|---|---|---|
| [AionCore](https://github.com/iOfficeAI/AionCore/tree/503a87199eb978790ca77675b39c84b241535bf3) | `503a87199eb978790ca77675b39c84b241535bf3` | Multi-agent orchestration backend reference |
| [AionUi](https://github.com/iOfficeAI/AionUi/tree/e108b6b56b1a6eb99a00904878d0582f657b2908) | `e108b6b56b1a6eb99a00904878d0582f657b2908` | Desktop UI / sessions / team activity reference |
| [Rustbox](https://rustbox.sh/blog/rustbox-vs-judge0-vs-e2b-vs-piston-code-execution-engine) | Article reviewed separately; no pinned source audit in this register | Possible future isolated code-execution research, **not chosen** |

Original local study: `C:\AI-Development-Team\reports\AION-ANALYSIS\AION-ARCHITECTURE-SECURITY-ASSESSMENT-2026-11-08.md`. Detailed evidence remains local; do not publish private environment artifacts.

## Adoption by stage

| Stage | Our need | Reference pattern | Decision | Preconditions / completion evidence |
|---|---|---|---|---|
| **AGCAT-1/2 + AGCAT-UI-01** | Trusted catalog, native operator approval, visual editor and revision/receipt persistence | No Aion integration | **OWN IMPLEMENTATION — MERGED 2026-11-09** | [PR #51](https://github.com/radekgosciniak/AI-Development-Team/pull/51) MERGED (`56c530c`); native functional smoke 12/12 PASS; Win32 F-01 T1–T5 PASS, independent P2 delta review 4/4 verified. High-assurance W-3 BLOCKED and custody C NOT CLOSED in separate track |
| **AGCAT-3** | Select verified agent for Tasks without creating an execution path | Catalog identity/revision / atomic membership check; Aion is conceptual input only | **OWN CONTRACT, NO IMPORT** | Hold catalog lock through Tasks commit; verify identity and revision; fail closed on missing membership; do not turn on `execution_ready` |
| **Stage 1A.2** | Real provider CLI integration and process lifecycle | AionCore ACP/process supervision concepts, session model | **ADAPT AFTER REVIEW** | Independent Windows process tree/Job Object review; explicit operator permissions; CLI capability and cancellation tests; recovery semantics; avoid blind auto-approval |
| **P7 — Real Agent Broker** | Reliable agent-to-agent handoffs and work allocation | AionCore Mailbox, TaskBoard, Team Scheduler, team coordination | **PRIMARY ADAPT CANDIDATE** | First independently review isolated Mailbox PoC (54/54 tests reported, delta review pending). Specify durable state, lease/ack, idempotency, deadlines, crash/restart and sender authentication; implement within our trust model |
| **P7 — Scheduled tasks** | Repeatable schedules without duplicate work | AionCore scheduler/cron and task deduplication | **ADAPT CONCEPTS** | Timezone/DST rules, persistence, missed-run policy, concurrency/idempotency and operator-controlled scheduling; no unattended command execution by default |
| **P8 — Live Execution / team activity** | Visibility of agent runs, handoffs, logs, approvals | AionUi team/session UI; AionCore status/activity events | **INSPIRE / ADAPT** | Real backend events first; avoid mock statuses, preserve existing approved Command Center layout and UX decisions; operator consent and accessible event history |
| **Possible future sandbox execution** | Isolate untrusted code if requirements justify it | Rustbox and comparison with Judge0/Piston/E2B | **RESEARCH ONLY** | Define threat model and OS support first; independent code/security audit, limits and isolation evidence; not a prerequisite for AGCAT or P7 |

## Explicit boundaries

1. **No migration to AionCore/AionUi** and no source-code import without a separate license review, security review and operator decision.
2. **No implicit approval** of ACP/MCP tool calls, agent launches, file edits or Git operations. Aion's approval defaults must not override our native operator gates.
3. **Do not equate file ownership or same-SID attribution with security against a hostile same-SID process.** Windows NTFS, ACL, races, process containment, Job Objects and recovery must be assessed within the relevant stage.
4. Keep **TaskBoard** (work lifecycle) separate from **Mailbox** (messages / handoffs); document at-least-once delivery and make handlers idempotent. Exactly-once must not be claimed without proof.
5. Before starting each stage, re-examine pinned external commits and current upstream changes; record reviewed version, accepted patterns and rejected patterns.
6. This register does not change state of P3-1A1 Recovery, P6-A, W3.3 or AGCAT-2 native acceptance, and grants **no authorization** for further PR merges, automatic agent execution or new implementation work. PR #51 was merged under its separate explicit authorization.

## Next concrete decision points

- **Completed 2026-11-09:** AGCAT-1/2 + AGCAT-UI-01 merged in [PR #51](https://github.com/radekgosciniak/AI-Development-Team/pull/51), merge `56c530ca5bcccc79b0a65f22e77e11b125eafcea` (source head `deeed3b4d5428a65e44b91bf0f0907ad5d01dfb9`). Functional native acceptance and F-01 retest passed; W-3 high-assurance and custody C remain explicitly open and were not formal merge gates.
- **Next planned: CC-INT-01 (NOT STARTED / not yet authorized):** independently trace AI Provider Accounts → Agent Catalog → Tasks → Execution → Sessions; classify each edge IMPLEMENTED / PARTIAL / MOCK / MISSING with source-backed evidence, then run synthetic automated integration tests only within authorized boundaries. The audit must not claim real agent launch, verified CLI-account switching or live sessions without proof.
- **After integration mapping:** plan AGCAT-3 Tasks membership using our catalog contract and shared lock, with its own scope, review and explicit approval. Not started; no production agent execution enabled.
- **Before P7 implementation:** ask an independent reviewer to perform **Mailbox PoC P2 delta review**; then draft a narrow P7 architecture decision comparing AionCore Mailbox/TaskBoard/Scheduler to our requirements, including Windows and security gaps.
- **Before P8 implementation:** decide an event schema for session/run/agent status and evaluate AionUi's interface patterns against our approved Command Center UX.

**Authority:** the public roadmap summarizes status; this document records external-reference usage. Neither one supersedes project security gates or explicit operator approval.


---

## 2026-11-09 — Agent memory / second-brain ecosystem: detailed adoption register

**Decision:** \`RESEARCH / SELECTIVE ADAPT / NO MIGRATION / NO INSTALLATION NOW\`. This is a candidate feature map for future AI Development Team work, **not** approval to implement, install plugins, add execution permissions, merge PRs, change task states or re-prioritize currently blocked work. The current V1.22 roadmap/AGCAT/P3-1A1/P6-A/P7/P8 status remains authoritative. The phase labels **CLM-0 through CLM-4** below are proposed *future architecture workstreams*, not new operational tasks; P3/P4/P5 below are indicative future planning windows, **not** the existing UI-REFRESH P3/P4/P5.

### Evaluated repositories and explicit disposition

| Reference | What it demonstrates | AI Team applicability | Proposed decision / when | Limitations and verification |
|---|---|---|---|---|
| [ai-memory — akitaonrails](https://github.com/akitaonrails/ai-memory) | \`capture → consolidate → recall → handoff\`; typed ownership/claim of handoffs; session history; git-backed Markdown wiki and derived search; Codex/Claude integration | **10/10; first handoff PoC candidate** | **CLM-1, after stable agent lifecycle and broker contracts**, evaluate in isolation ahead of any integration; coordinate with P7 Mailbox rather than inventing a parallel dispatcher | Native Windows described as **experimental** (WSL2 supported); audit hooks, privacy sanitization, exactly-once claim scope, restart and concurrent sessions. A "claimed once" handoff is **not** proof of exactly-once execution |
| [GBrain — garrytan](https://github.com/garrytan/gbrain) | Shared explicit facts with sources, corrections/withdrawal, MCP, keyword+optional semantic search, cited synthesis, typed knowledge graph, optional enrichment | **9/10; advanced shared knowledge candidate** | **CLM-2 / P4 candidate**, compare to ai-memory and a minimal own adapter; choose at most **one** principal memory engine | Bun + PGlite/PostgreSQL, optional API spend; audit cloud data flows, tenancy boundaries, operator control, upgrade paths and source citations. Self-hosted MIT is not a guarantee of isolation |
| [Open Second Brain — itechmeat](https://github.com/itechmeat/open-second-brain) | Deterministic correction→preference lifecycle, confidence/retirement, snapshots, attributed writes, freeze/rollback, memory hygiene, searchable agent transcripts | **8/10; preference and audit patterns** | **CLM-3 / P4–P5, inspiration**, not full-system import | Operator approval before operational/safety policies become binding; prevent feedback poisoning, support rollback and confidence/revalidation |
| [COG Second Brain — huytieu](https://github.com/huytieu/COG-second-brain) | Markdown/Git agent skills and separate read-only verifiers, \`memory-hygiene\` rechecking, self-evolving workflows | **8/10; verification patterns** | **CLM-0/CLM-3, contract inspiration**; do not install its agent packs wholesale | Verify real independent reviewer identity and read-only permissions; author's README has different skill counts by surface/version |
| [Baalda — naveedharri](https://github.com/naveedharri/baalda) | Tauri v2/Rust/React/TS local-first Markdown workspace, filesystem watcher, revisions, CRDT/Yjs sync, MCP with scoped access | **6/10; team-doc collaboration/UI reference** | **CLM-4 / P5 optional**, only if simultaneous human/agent note editing becomes a product requirement | Separate sync/revision protocol from task execution; security, ACL and CRDT merge testing; do not introduce its entire server |
| [Claude Second Brain — jessepinkman9900](https://github.com/jessepinkman9900/claude-second-brain) | Source-ingestion wiki with linked pages/citations, Obsidian, Git, local qmd search | **6/10; wiki import/source schema inspiration** | **CLM-2 reference only**; no automatic global install | Claude-oriented workflow; external tooling/model download and storage; not an orchestration or task-state engine |
| [Agent Second Brain — smixs](https://github.com/smixs/agent-second-brain) | Telegram voice→Obsidian linked notes, recurring jobs and knowledge maintenance | **3/10 for core AI Team** | **OUT OF CORE ROADMAP**; reconsider only if optional voice/Telegram capture is requested | VPS, messenger data/security, persistent-agent and subscription behavior are outside current goals |
| Andrej Karpathy's LLM Wiki idea | LLM-maintained, source-linked wiki rather than unsorted chat snippets | **8/10 conceptually** | **CLM-0 / CLM-2, design principle only** | Supplied Gist URL was truncated (\`gist.github.com/.../442a6bf555914893e9891c11519de94f\`); **original source not verified**. Do not assert implementation, license or exact design details from this reference |
| [Obscura — h4ckf0r0day](https://github.com/h4ckf0r0day/obscura) | Rust headless browser automation, CLI/MCP/CDP, independent rendering approach | **7/10 potential; ~1/10 present priority** | **P4 backlog / optional Browser Provider**, *after* existing Orca/browser capability audit and stable core | Do not replace Orca browser or Playwright/Chromium; rendering/Web API parity, sandbox/credentials, SSRF, private-network access and native Windows tests must pass |
| [AionCore — iOfficeAI](https://github.com/iOfficeAI/AionCore/tree/503a87199eb978790ca77675b39c84b241535bf3) | Mailbox, TaskBoard, scheduler, ACP/process lifecycle, REST/WebSocket concepts | **Primary P7 coordination reference, not a platform replacement** | **Stage 1A.2** selective ACP/process ideas; **P7** Mailbox/TaskBoard/Team Scheduler after delta review; cron conditional | No implicit MCP/ACP approval; verify Windows process containment and permission gates; no unverified claims of auto team spawn or sandbox security |
| [AionUi — iOfficeAI](https://github.com/iOfficeAI/AionUi/tree/e108b6b56b1a6eb99a00904878d0582f657b2908) | Team/session activity, live status, visual session history | **P8 UI reference** | **P8**, only after real backend event/run data exist | No fake statuses; preserve approved Command Center UX and test Windows/Tauri semantics |

**Aion pinned commits independently resolved in their correct repositories on 2026-11-09:** AionCore \`503a87199eb978790ca77675b39c84b241535bf3\`; AionUi \`e108b6b56b1a6eb99a00904878d0582f657b2908\`. The reviewed snapshots are not an endorsement of their security or APIs. The Aion-specific stage table above remains part of this register and takes precedence for Aion scheduling.

### Feature → source → phase → acceptance map

| ID | Desired AI Team capability | Primary references | Candidate timing / gate | Acceptance criteria before implementation |
|---|---|---|---|---|
| **CLM-0** | Define a **context contract** and source-of-truth boundaries | ai-memory, COG, LLM Wiki, existing Aion/P7 plans | **Design only**, after stabilizing current workflow; align with Stage 1A.2 and P7 contract freeze | Existing task/run/approval schema audited first. Document project/task/run/agent scopes, who may read/write, immutable evidence links, decision approval. No new DB/service yet |
| **CLM-1** | **Cross-agent handoff and session recovery** | ai-memory typed handoff; AionCore Mailbox/TaskBoard | After Stage 1A.2 process/session lifecycle works; **P7** broker is the natural integration point | Codex → Claude → Codex can continue on a synthetic task; single owner/claim + durable acknowledgment, dedup/idempotency, lease expiry and recovery, cancellation, replay safety and operator gate verified; no duplicate side effects |
| **CLM-2** | **Project long-term knowledge** with citations and bounded recall | GBrain vs ai-memory; Claude Second Brain/wiki idea | **Future P4 / after P7 and stable P8 event contracts**, isolated evaluation only | Retrieve approved decision plus linked report/commit/hash; show superseded/contradictory and unknown information; rebuild search index from owned source; no global agent-rule overwrite |
| **CLM-3** | **Verified memory hygiene and candidate rules** | Open Second Brain, COG, GBrain corrections | **Future P4/P5, after CLM-2** | Every derived claim has provenance/validity and reviewer status; change log and rollback; no automatic promotion of repeated agent text to policy; approval required for safety/authority changes |
| **CLM-4** | **Multi-user collaborative knowledge UI** | Baalda | **Future P5, optional** | Explicit user demand, tenancy ACLs, revision conflicts, offline merges, export/restore and native Tauri tests; keep task broker as separate authority |
| **BRW-OPT** | Optional **browser provider interface** | Obscura vs current Orca browser and Playwright/Chromium | **P4/backlog, lowest priority**, only after core feature completion | Audit Orca engine/CDP and existing tooling; only add provider if coverage/use-case gap measured; visual UI testing remains Chromium/native; negative tests for SSRF/local addresses, secrets and sandbox escape |

### Concrete handoff contract to specify before coding (illustrative, **not implemented**)

Proposed minimal data: \`handoff_id\`, \`schema_version\`, \`project_id\`, \`task_id\`, \`run_id\`, \`from_agent\`, \`to_agent\`, \`owner\`, \`status\`, \`created_at\`, \`expires_at\`, \`attempt\`, \`goal\`, \`completed_steps\`, \`failed_attempts\`, \`next_steps\`, \`open_questions\`, \`authorization_reference\`, \`evidence[]\` (Git commit, report path, size, SHA-256), \`source_snapshot\` and \`acknowledged_at\`. Invariants: bounded context packet, immutable original evidence, explicit operator approvals, atomic/conditional claim, idempotent executor, durable lifecycle and error/timeout/restart recovery. A SHA-256 digest proves file integrity relative to the recorded digest, **not** author authenticity/signature.

Memory records should distinguish **proposal / observed / verified / approved / superseded / withdrawn** and carry \`source\`, \`recorded_at\`, \`last_verified_at\`, \`project_scope\`, \`confidence\` and optional validity window. Claims made by an agent or retrieved from a previous session are untrusted until verified. A live task status must never be inferred solely from memory. Preserve evidence and never promote memory to a permission token.

### Architecture and security boundary

\`\`\`text
Codex / Claude / additional agents
           |
      Context gateway
   (scope + grants + audit)
         /           \
AUTHORITATIVE       OPTIONAL MEMORY
TASK/RUN STATE      knowledge + handoff summaries
(approvals, locks,    with citations, correction
 live status, retry)  and provenance
         \           /
    Reports / Git / evidence
\`\`\`

- **Orchestrator DB/state is the only authority** for active task/run lifecycle, blocked/paused state, execution rights, approvals, lock ownership, recovery, agent membership and current Git permissions. The optional memory store is advisory. Historical reports are evidence, not live permission.
- Keep **Aion Mailbox** message/ack separate from **TaskBoard** work/lease, and from CLM knowledge. Never have two independent schedulers/dispatchers executing the same job.
- All imported content, external repos, AI-generated summaries, old chat logs and source-linked notes are untrusted input. Sanitize secrets, enforce project/user isolation, deny cross-project reads without grants, prevent prompt injection/tool-call instructions through memory, and audit outbound model/embedding providers.
- Preserve Windows fail-closed model: no automatic CLI launch, file mutation, git write/merge, MCP auto-approval, same-SID security claims, ACL/Job Object bypasses or background cron execution absent independent acceptance.
- Maintain snapshots/backups, export and deterministic rollback for modified memory. Markdown can be human-readable canonical knowledge; any SQLite/FTS/vector index should be rebuildable. Do not silently upload or publish local private reports.
- Third-party code reuse requires **pinned source revision, license check, dependency/SBOM review, independent security review and explicit operator authorization**, even when MIT/Apache-2.0.

### Evaluation plan and go/no-go evidence

1. **Inventory existing functionality before buying/building memory.** Identify what P7 task/handoff/report protocol, AGCAT, Stage 1A.2 provider sessions and P8 events already supply. Document overlap and a single owner for each datum; do not double-store authoritative task state.
2. **Isolated CLM-1 pilot:** synthetic repo and sessions, no secrets, mocked or sandboxed side effects, Codex→Claude→Codex, interruption before/after ack, duplicate delivery, stale claim and restart. Compare a minimal own handoff contract with ai-memory; test native Windows (WSL2 success alone is insufficient).
3. **Isolated CLM-2 comparison:** representative *synthetic* decisions, approved/rejected changes, contradictory and stale reports; compare GBrain, ai-memory and simple Markdown+FTS by retrieval accuracy, provenance/citations, startup latency, disk use, privacy and API cost. Decide *one* memory service/provider or none.
4. **CLM-3 only after reliable recall:** verify rule suggestions from repeated corrections, false positives, approval gates, undo and independent read-only reviewer. Optional timed consolidation stays off until authorized and shown to preserve evidence.
5. **CLM-4 and BRW-OPT:** only if actual collaborative editing/browser automation needs justify their incremental operational complexity.

### Roadmap discipline / non-actions

- **CURRENT (post-merge 2026-11-09):** AGCAT-1/2 and AGCAT-UI-01 are merged in PR #51; native functional acceptance passed, but high-assurance W-3 and custody C remain separately open. Next planned CC-INT-01 audits real module integrations; AGCAT-3 and execution are not started/authorized. Preserve P3-1A1 Recovery SUSPENDED/R7 NO-GO, Stage 1A.2 NOT STARTED, Stage 2 BLOCKED, FIX-4 paused, and unapproved worktree/ACL constraints.
- **NEXT (only after separate authorization):** CC-INT-01 integration audit and automated synthetic-flow tests, then specify AGCAT-3 contract and only later consider authorized provider sessions. P7 Mailbox PoC independent delta review is required before a broker architecture decision; no parallel memory broker or actual agent launch is authorized.
- **LATER:** CLM-2/3 knowledge and preference layers, P8 live UI from real events, optional CLM-4 and Obscura/BRW-OPT. Avoid treating "P3/P4/P5" proposed evaluation windows as approved additions to already named implementation phases.
- **OUT OF SCOPE:** replacing AI Development Team with AionCore/AionUi, installing all second-brain products simultaneously, automatic memory-based authorization, Telegram/voice capture, unneeded Obsidian/cloud sync, silent global \`AGENTS.md\`/\`CLAUDE.md\`/MCP or hooks mutations, and unattended cron-driven execution.
- **DECISION STATUS:** architecture **BACKLOG / EVALUATION ONLY**. No code, dependencies, local files, task statuses, branch merges, agent credentials, jobs or operator approvals are changed by this document.

### Source references and provenance

The eight listed second-brain repositories were inspected through their GitHub metadata and README content on **2026-11-09**. Numbers of stars, features and speed/security claims are upstream self-descriptions, not independently reproduced benchmarks. AionCore/AionUi code-level observations and snapshots are documented in the earlier part of this register and in the local \`reports/AION-ANALYSIS/AION-ARCHITECTURE-SECURITY-ASSESSMENT-2026-11-08.md\`. Obscura was previously evaluated as browser-provider research; Orca's exact browser engine/CDP support has **not** been independently established.

**Authority notice:** This public register is a planning aid. It does not supersede the application implementation repo, its evidence, operator decisions, fail-closed security policies or independent acceptance.


## 2026-11-15 — Open Duet comparative architecture review (ADAPT / NO MIGRATION)

**Status: research and design reference only; NOT implementation authorization.** Open Duet (project shown as `open-duet` in the reviewed demonstration; upstream canonical URL/commit and license require independent confirmation before code reuse) was compared against existing AionCore, AionUi, ai-memory, GBrain, COG Second Brain, Open Second Brain, Orca and PILR references. The current review is documentation/concept-level; **no code-level security audit, runtime tests, pinned upstream snapshot, or independent benchmark was performed**. Do not treat demo animations or README claims as acceptance evidence.

| Need | Existing primary reference | Open Duet contribution | Decision |
|---|---|---|---|
| Reliable delegation, mailbox, task lifecycle | AionCore Mailbox / TaskBoard (P7) | Unified roster/team UX and delegation flows | **ADAPT UX/CONTRACT IDEAS ONLY**; keep one authoritative broker |
| Agent handoff and recovery | ai-memory + P7 | Shared agent context and teamwork patterns | **COMPARE** against CLM-1; no second handoff authority |
| Long-term project memory and provenance | GBrain, COG, Open Second Brain (CLM-2/3) | Shared memory UI and discovery | **REFERENCE**; test citations, correction, scope and rollback before selection |
| Scheduled workflows | AionCore scheduler | Reusable routines and scheduling UX | **ADAPT CONCEPTS**; only one scheduler, explicit operator consent, DST, missed runs, retries and dedup |
| Agent sessions and desktop activity | AionUi, own Tauri/React | Seven-step progressive disclosure and cohesive product navigation | **UX INSPIRATION**; real event data only |
| Accounts and CLI execution | Own Stage 1A.2; Orca as pattern reference | Roster/team entry points | **NO RUNTIME MIGRATION**; preserve account isolation, Windows process controls and approval gates |
| Project governance | PILR-ADR-001 | Routines/team context can consume an approved roadmap snapshot | **INTEGRATE CONCEPTUALLY**; Roadmap Mode optional, Direct Task Mode always available |

### Targeted evaluation gates (no implementation authorized)

1. **OD-01 source verification:** resolve canonical repository, exact commit, license, backend/frontend modules, test coverage and release maturity; distinguish working code from demonstration, experimental or simulated integrations.
2. **OD-02 routines vs scheduler:** synthetic restart, timeout, duplicate delivery, missed-run/DST, retry and approval tests; compare to AionCore. Never create a competing task executor.
3. **OD-03 shared memory vs GBrain/ai-memory:** evaluate citation accuracy, contradiction/supersession, project isolation, recovery, local storage, sensitive data and deterministic export/rollback.
4. **OD-04 delegation vs P7 Mailbox:** verify durable claim/ack, idempotency, crash recovery, cancellation, agent identity and permission boundary; distinguish messaging from authoritative task leases.
5. **OD-05 UX/PILR:** prototype a non-executing view that connects agent roster, routines, project pulse and roadmap context without displacing approved Command Center layouts; retain direct tasks without roadmap.

**Architecture rule:** one authoritative Projects/Tasks/Runs/Sessions state, one broker and one scheduler; memory is advisory and cannot grant permissions or mark tasks accepted. Open Duet is an integration/UX reference, not a replacement for AionCore, ai-memory, GBrain, Orca or the Command Center. Existing security gates, stage order and stabilization priority remain unchanged. External code may be imported only after pinned-source, license, dependency and independent security reviews and separate operator approval.


## 2026-11-15 — Microsoft TokenEconomizer (agent experience / cost optimization)

**Decision: ADAPT / NO MIGRATION / NOT AUTHORIZED FOR INSTALLATION.** [Upstream](https://github.com/microsoft/TokenEconomizer) (Apache-2.0) reports benchmark savings for coding agents, but this has not been verified for Command Center. Potential patterns: (1) project-scoped, evidence-backed micro-skills tied to PILR and ai-memory/GBrain; (2) bounded failure-loop detection in Runs/Sessions with operator notification; (3) advisory-first tool pruning, never silent removal of security-critical capabilities; (4) accurate token/cache/cost telemetry.

**Compatibility caution:** upstream tested Claude Code 2.1.300–2.1.310 and Codex 0.153.1–0.164.0; local Claude 2.1.325 / Codex 0.167.2 are outside the documented ranges. Plugin hooks and edits to `.claude/settings.local.json` / `.codex/config.toml` could affect managed profile isolation, Trusted CLI contracts and permission boundaries. No installation in managed profiles, no model/agent run and no modification of account settings is authorized by this entry.

**Next gates:** TOK-01…TOK-06: pinned source and license → privacy/identity isolation → offline micro-skills → synthetic loop guard → tool-pruning feasibility → separately approved cost/quality pilot. Continue Trusted CLI / G1 / TC-3 / E7 first. See [TOKEN-OPT-01 detailed roadmap](proposals/TOKEN-OPT-01-agent-experience-token-optimization.md).


## 2026-11-16 — Andrew Ng OpenWorker (agent harness, sandbox, approvals)

**Decision: ADAPT / NO MIGRATION / DESIGN REVIEW ONLY.** [Upstream](https://github.com/andrewyng/openworker). Candidate patterns: Harness SDK and session lifecycle (E7 / Interactive Agent Chat), approval/consent (Trusted CLI / E4), Windows sandbox (security gates), scheduling (P7 / PILR) and session UX. Verify source commit, license and actual implementation before relying on upstream claims. No installation, execution, migration, changes to account profiles or implementation authorization; no change to current stage order. Gates OW-01…OW-06 and adoption criteria: [OPENWORKER-01 review](proposals/OPENWORKER-ADOPTION-REVIEW.md).
