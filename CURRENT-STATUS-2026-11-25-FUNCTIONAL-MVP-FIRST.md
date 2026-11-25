# Functional MVP First — 2026-11-25

Operator approved R2-01–07 option A and deferred R2-O1–O4. SQLite selected for synthetic P7A-2B only; implementation is not authorized yet. REV-02 targeted corrections addendum SHA-256 949551dd444540bfa4054e06eb9f2083ac43683037570b910f653506e216f0ba. Three proposed closures (allocator replay, CancelOnly, refusal precedence) await focused independent confirmation. Exact P7A-1 revision d3e132295d6208f5a02540287eaea7da3d46c1ad; E-01 BLOCKED / NOT RUN, acceptance outstanding.

## Approved decisions
R2-01 rusqlite =0.43.0 bundled, default features disabled; candidate libsqlite3-sys 0.40.0 / SQLite 3.54.1; validate lockfile, MSVC, Rust >=1.89, feature graph, advisory and SBOM during authorized implementation.
R2-02 local transactional boundary; process termination with OS running and trusted intact store only; no whole-store rollback detection or OS/power-loss guarantee.
R2-03 separate durable 64-ID reservations, epoch floors and V1–V10 replay checks.
R2-04 atomic session recovery, versioned /2 events, historical /1 unchanged.
R2-05 deterministic time/lease refusal before reservation; indeterminate COMMIT freezes until reopen.
R2-06 synthetic limits: 64 runs, 72 events/run, 128 KiB/event, 256 ordinary sessions, then CancelOnly with at most 64 atomic session+Cancel records, 1 GiB database. Cancellation remains conditional on healthy storage, available IDs and valid time; no completion in CancelOnly. No production claim.
R2-07 local NTFS test custody, one canonical path; A/B only after P7A-2B authorization, C1/C2 separately authorized, C3 deferred, D independent.
R2-O1–O4 deferred: external checkpoint, import, snapshot/compaction/archive/store succession, lease renewal/paged audit. Remove synthetic lifetime limits before production use.

## Shortest route to functional MVP
1. Confirm only three targeted design corrections; no full architecture audit. Preserve E-01; no retry of denied operation or alternate executor bypass.
2. In parallel, propose P7A-3/P8 synthetic browser workflow UI on isolated merged baseline 0f5cbf4f24f40f2f01394c83f43d88f4664fccdc, using existing React components and mock IPC. Reuse local preview dd68718 as reference, not accepted native evidence. Show task, assigned developer, QA, reviewer, correction, operator acceptance, errors and cancellation. No actual agent calls, native IPC, production bundle changes, or merge. Separate implementation authorization required.
3. When independently authorized E-01 prerequisites exist, complete existing E-01 and operator acceptance; then separately authorize P7A-2B SQLite implementation and P7A-2C acceptance. No new architecture scope.
4. Integrate accepted broker and UI only after dependency and merge-order review; synthetic end-to-end workflow first.
5. Resolve real execution prerequisites and obtain explicit authorization before real Codex developer -> mandatory QA -> Claude reviewer -> operator acceptance. No automatic Git/system changes.
6. Deliver first real workflow, then hardening and product-scale capacity. MVP success is observable workflow completion, not report count.

## Preserved gates
PR83 Draft exact head 1b1eb792c9e9595cf86648204d5a16ba96b01629; CL-P2 NOT ACTIVE; P2 BLOCKED; P0–P9 INCOMPLETE; PV-12 BLOCKED; PV-13 positive partial feedback; SEC-WEBVIEW2-01 OPEN; Origin:null NOT VERIFIED; G-ENV OPEN; G1 BLOCKED; D-3 DEFERRED; native acceptance NOT RUN; TC-0 synthetic-only; Trust Broker custody-only; real agents and FULL NOT AUTHORIZED. No VM, permission changes, or bypass. No application source changes or merge authorized by this roadmap update.

New requirements block only if necessary for current correctness, security or existing mandatory contract. Other enhancements enter backlog. Existing gates are not waived. External adoption remains conceptual under existing register.
