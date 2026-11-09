# External repositories — adoption map for AI Development Team

**Status:** architecture reference / planned evaluation, **NOT IMPLEMENTATION AUTHORIZATION**  
**Last updated:** 2026-11-09  
**Owner:** AI Development Team architecture roadmap  
**Canonical public roadmap:** [AI Team Roadmap](https://github.com/radekgosciniak/ai-team-roadmap)  
**Project implementation:** [AI Development Team](https://github.com/radekgosciniak/AI-Development-Team)

This document is a durable record of which externally reviewed projects may be useful, **when** to revisit them and **what must be verified** before implementation. It is a decision register, not a dependency list or permission to copy code.

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
| **AGCAT-1/2** | Trusted catalog, native user approval and agent membership | No Aion integration | **KEEP OWN IMPLEMENTATION** | AGCAT-1 accepted with conditions; AGCAT-2 PR #51 Draft, Windows native acceptance still pending |
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
6. This register does not change state of P3-1A1 Recovery, P6-A, W3.3 or AGCAT-2 native acceptance, and grants **no authorization** for PR merge or agent execution.

## Next concrete decision points

- **Now:** finish AGCAT-2 native smoke qualification (W-1 V3 preflight controller list-construction blocker), keep [PR #51](https://github.com/radekgosciniak/AI-Development-Team/pull/51) as Draft until native acceptance decision.
- **After AGCAT-2:** plan AGCAT-3 Tasks membership, using our approved catalog contract.
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

- **NOW:** finish current AGCAT-2 Windows native qualification; W-1 V3 is blocked in the preflight controller, not an ACL finding; PR #51 Draft/NO MERGE. Preserve P3-1A1 Recovery SUSPENDED/R7 NO-GO, Stage 1A.2 NOT STARTED, Stage 2 BLOCKED, FIX-4 paused and unapproved worktrees/ACL constraints.
- **NEXT (only after relevant gates):** AGCAT-3 contract then authorized provider sessions; P7 Mailbox PoC independent delta review before architecture decision. P7 remains the first executable cross-agent handoff integration, not a new parallel memory broker.
- **LATER:** CLM-2/3 knowledge and preference layers, P8 live UI from real events, optional CLM-4 and Obscura/BRW-OPT. Avoid treating "P3/P4/P5" proposed evaluation windows as approved additions to already named implementation phases.
- **OUT OF SCOPE:** replacing AI Development Team with AionCore/AionUi, installing all second-brain products simultaneously, automatic memory-based authorization, Telegram/voice capture, unneeded Obsidian/cloud sync, silent global \`AGENTS.md\`/\`CLAUDE.md\`/MCP or hooks mutations, and unattended cron-driven execution.
- **DECISION STATUS:** architecture **BACKLOG / EVALUATION ONLY**. No code, dependencies, local files, task statuses, branch merges, agent credentials, jobs or operator approvals are changed by this document.

### Source references and provenance

The eight listed second-brain repositories were inspected through their GitHub metadata and README content on **2026-11-09**. Numbers of stars, features and speed/security claims are upstream self-descriptions, not independently reproduced benchmarks. AionCore/AionUi code-level observations and snapshots are documented in the earlier part of this register and in the local \`reports/AION-ANALYSIS/AION-ARCHITECTURE-SECURITY-ASSESSMENT-2026-11-08.md\`. Obscura was previously evaluated as browser-provider research; Orca's exact browser engine/CDP support has **not** been independently established.

**Authority notice:** This public register is a planning aid. It does not supersede the application implementation repo, its evidence, operator decisions, fail-closed security policies or independent acceptance.
