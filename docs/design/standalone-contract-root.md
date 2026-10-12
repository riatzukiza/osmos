# Osmos standalone contract-root planning

Status: proposed; no implementation or deployment. Source/fork base: `0c33018e27f861536822086afcdab666a60add44`. Owning proposed story: [`6f9fe530-b303-477d-84b6-11adacce86ba`](../agile/tasks/standalone-contract-root.md), incoming, proposed three points.

## Existing authority and complete outcome

Osmos owns the extracted JVM `kms-ingestion.*` service. Its existing EDN loader, merge precedence and ingestion schemas stay authoritative locally. Filesystem/environment checks belong to outer adapters; portable selection/admission decisions belong in `.cljc` when practical. This proposal adds no alternative parser, event ledger, board validator, workflow, source pin or provider.

The Foresight E1.08 eight-point umbrella continues to require standalone service/image operation, explicit configuration/contracts/resources, service-free unit tests, documented live integration dependencies, Knoxx compatibility and coordinated Services references. Its canonical identity, blocked state, dependency and history are untouched. This smaller story covers the contract/resource binding prerequisite, not the entire image/service/consumer qualification.

## Current facts and read-only probe

| Source at base | Observed behavior | Qualification limit |
| --- | --- | --- |
| `contracts/loader.clj:96–110` | Root selection is test binding, `CONTRACTS_DIR`, cwd/contracts, then cwd/../contracts. | A neighbouring checkout can supply production input without explicit binding. |
| `contracts/loader.clj:194–214` | Cache key is tenant/source; lookup precedes root resolution. | Root B can return root A's already cached contract. |
| `contracts/loader.clj:73–78,198–216` | Missing file returns nil; a missing explicit root can produce nil. | Optional per-source layers and required deployment inputs must be distinguished. |
| `server.clj:356–378,441–468` | Missing root logs/skips source creation; database init and job reset run first, and later workers/server still start. | No fail-closed pre-effects bundle admission exists here. |
| `Dockerfile:12`, `deps.edn:1`, `.gitignore:1` | Docker copies resources; classpath declares resources; resources is ignored and absent from the clean 53-file extraction. | Source absence is confirmed; no Docker build failure was executed or image claim made. |
| `db.clj:init!` | Current schema setup is inline SQL; source search found no `io/resource` or Migratus runtime invocation. | Do not invent a required migration bundle or conclude no future resource is needed from the dependency name. |

Paths in the first four rows are under `src/kms_ingestion/`. The private Babashka1.12.207 probe loads the actual, unchanged loader with only its standard-library dependencies. It uses synthetic files under the owned temporary directory, unsets `CONTRACTS_DIR`, and restores its own process `user.dir`/cache in `finally`. It opens no database, service, provider or watcher.

Observed sequence, preserved losslessly in verification artifacts:

1. A synthetic cwd with only parent/contracts selects that parent and loads marker `parent`.
2. Explicit root A loads `root-a` for tenant acme/source docs.
3. Explicit root B without invalidation still returns `root-a`, despite the selected root naming B.
4. Invalidation permits B to return `root-b`.
5. A missing explicit root is selected as a string and load returns nil.

These are source behavior observations, not implementation red/green qualification or a claim of an actual parallel-worker effect. Existing loader tests invalidate between fixtures, so they do not prove root-key cache isolation. Historical filename-slug attempts to read two Sol cards refused; corrected explicit UUID reads succeeded and remain separate evidence.

## Proposed binding and startup contract

The proposed default requires an explicitly configured bundle root. Review whether `CONTRACTS_DIR` must be absolute, or whether a relative value can be resolved once against a documented fixed owner input; it must never depend on arbitrary process cwd. The test binding remains an isolated seam. Do not silently substitute cwd/parent directories when configuration is absent, invalid or inaccessible.

Admit the root, required inventory and declared owner/revision before database construction or other startup effects. An input contract distinguishes required bundle files from the loader's optional defaults layers: not every absent tenant/source file becomes a startup error. Unknown, unreadable, malformed or incomplete required inputs fail with secret-free diagnostics. If owners need deliberate contractless operation, review an explicit opt-in mode and its reduced guarantees; accidental nil is never that mode or standalone extraction proof.

Cache selection must incorporate admitted root identity. Preserve per-job override non-caching, explicit invalidation, merge order and secret-resolution timing unless a reviewed compatibility decision changes them. Root changes must not reuse a previous root's tenant/source result. Do not log or persist resolved credentials in admission evidence.

Review and select one reproducible bundle delivery mechanism, with ownership and required inventory. Possible choices are a separately mounted versioned bundle or a declared packaged non-secret bundle; neither is activated here. Account for genuinely required classpath resources in the same documented build contract. If the extraction uses no classpath resources, justify removal of the ineffective declaration/COPY rather than manufacturing an empty directory; if it requires them, include the actual owned files and test their absence. Do not copy donor configuration or secrets to make a cold build appear successful.

## Laws and fixture contract

Portable laws decide admission from shaped root/bundle facts and describe complete failure outcomes; host adapters supply canonical path/directory/readability/inventory facts. The pure layer cannot inspect Node/JVM filesystem objects. Existing EDN parsing and service schema validation remain at their proper owner seams.

Meaningful red fixtures must demonstrate sibling/cwd discovery cannot satisfy explicit binding, invalid/missing required inputs prevent all startup effects, and A→B→A selects each root's own contract without test-wide invalidation hiding the collision. Preserve defaults/tenant/source/job-override precedence, overrides remaining uncached, and secret resolver behavior using synthetic markers only.

Green must prove actual loader behavior and the pre-effects startup seam, not only a boolean law. Run portable decisions on both JVM and NBB/CLJS where feasible, JVM loader tests, service-free existing unit tests with private caches, and selected artifact assembly/build-context checks without live services. Full cold service/image/integration/deployment qualification remains the umbrella's later work; missing dependencies or unexecuted checks stay explicit.

## Planning and execution holds

Osmos has no checked-in AGENTS, local skill, Rheos configuration, board history, CI workflow or receipt consumer at this base. Global instructions and canonical PR-flow therefore control process. Native Rheos1.1.1 read-board at the empty extraction reports zero tasks and emits ENOENT for default `docs/agile/tasks`; this is absence evidence, not board validation. The story uses that native default path as first-class Markdown input without adding configuration or another engine.

A later read-task/read-board can prove artifact visibility and its incoming status, but cannot establish reviewed ready admission. Planning decisions and estimate need native review/convergence and lawful Rheos-ready transition before implementation. If the three-point estimate is insufficient, review and break down the scope rather than weakening startup/resource acceptance. Automatic merge stays off; no manual reviewer request is part of this preparation.

New process receipts use the global append-only envelope, with additive repo context. No local Osmos consumer exists; any public Receipt River verification used for these new rows is identified as a chosen preparation check, not an inherited repository gate or authority over historical Sol/Axxium rows.
