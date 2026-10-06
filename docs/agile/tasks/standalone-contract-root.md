---
uuid: "6f9fe530-b303-477d-84b6-11adacce86ba"
title: "Make Osmos contract binding independent of its checkout and working directory"
type: "task"
status: "incoming"
priority: "P1"
points: "3"
labels: "osmos, ingestion, standalone, contracts, planning"
created_at: "2026-10-06"
---

# Standalone Osmos contract binding

## Context

Osmos personal and upstream main are both `0c33018e27f861536822086afcdab666a60add44`. Foresight's existing [E1.08 extraction card](https://github.com/riatzukiza/foresight/blob/96a6dca24cb7a14b041bdd6e3e7922c568238da9/docs/agile/kanban/e1-08-osmos-ingestion-service.md), canonical identity `e1-08-osmos-ingestion-service`, remains eight points and native Rheos `blocked`. This is one proposed three-point prerequisite slice; it does not narrow, close or change that umbrella, its bootstrap dependency, or its history. The reference is in the body because it belongs to a different repository/board; no cross-project parent or hard dependency is invented.

Current source finds contracts through cwd and its parent; root changes can reuse another root's cached tenant/source result. Startup logs and continues when no root is found. The Dockerfile also copies an absent, ignored `resources/` directory. Source reproduction and the packaging inventory are in the [design note](../../design/standalone-contract-root.md).

## Outcome

An independently packaged Osmos revision can bind an explicit, documented contract bundle without donor checkout discovery or cwd-dependent selection. Missing or invalid required bundle inputs refuse readiness before database, executor, watcher, network or job effects. Contract selection and cached results remain isolated by the admitted root. The build context and runtime input contract account for every genuinely required resource instead of assuming the absent donor directory exists.

## Scope

- Define and review explicit root/bundle binding, preserving the existing defaults → tenant → source → job-override precedence and supported ingestion APIs.
- Separate portable admission/selection decisions from filesystem/environment adapters; use the existing EDN loader and service schemas, not another parser or contract engine.
- Remove implicit cwd/parent discovery in production; specify compatibility and startup migration for existing `CONTRACTS_DIR` users.
- Validate the configured root and required bundle inventory before startup effects; distinguish an intentionally declared contractless mode, if owners approve one, from an accidental missing bundle. Contractless operation is not the current proposed default.
- Bind cache identity to the admitted root as well as tenant/source, retaining explicit invalidation and override behavior. No silent reuse across roots.
- Document and test one chosen standalone bundle delivery and build/runtime resource contract. Inventory actual inputs before altering Docker/resource declarations; no placeholder directory merely to satisfy `COPY`.

## Non-goals

No live database, service deployment, Docker daemon change, credentials, receiver configuration, scheduler redesign, ingestion/retry/dedup algorithm change, translation-model authority change, Clio/Receipt River engine copy, or Rheos configuration/state change. Knoxx/Services consumer migration, integration smoke, image promotion and donor retirement remain in the eight-point umbrella. This PR implements none of the proposed changes.

## Acceptance criteria

- [ ] The reviewed input contract names bundle ownership, revision/inventory and delivery. The same explicit admitted bundle yields the same contract from two unrelated cwd values; a tempting sibling `contracts/` never supplies an implicit fallback.
- [ ] Unset/blank/absent/not-directory/unreadable configured roots and missing/malformed required bundle files fail with bounded, secret-free diagnostics before `db/init!`, job reset, executor initialization, job enqueue, scheduler, watcher, translation worker or HTTP readiness. Any approved contractless mode is explicit and cannot qualify contract-backed extraction.
- [ ] A→B→A loads with the same tenant/source IDs cannot reuse another root's contract. Existing merge precedence, explicit invalidation, job overrides and secret resolution remain compatible; resolved secret values never enter new proofs or diagnostics.
- [ ] The isolated source/build context accounts for actual required classpath resources and runtime contract files. Missing required inputs fail before build/startup; irrelevant donor resources are not copied. A resulting package's declared input contract remains reproducible without parent paths.
- [ ] Portable decision laws run on JVM Clojure and ClojureScript/NBB where practical; host checks remain in outer adapters. Actual JVM loader/startup-seam tests and package verification cover the effect boundary, not only the pure decision.
- [ ] Planning review settles the binding/migration, contractless-mode, resource-delivery and estimate decisions, then Rheos admits implementation readiness. No current source probe, card parsing, document or empty-board result supplies that qualification.

## Verification

Start implementation only after planning convergence and lawful Rheos-ready admission. Red fixtures cover cwd/sibling contamination, root-key cache collision, invalid required inputs and missing build-context resources. Green uses the existing loader/host boundaries and preserves merge/override behavior. Run the existing service-free unit alias with private caches and explicit test bundles; run focused portable tests on both hosts. Prove the actual pre-effects startup seam with bounded spies, then the selected artifact assembly/build-context boundary. Record each command, source revision, actual counts and unavailable prerequisites. No live DB/network/service test is claimed by this planning slice.

## Risks

Three points is a proposed estimate for this one binding/packaging seam, not an accepted budget for the full extraction. If reviewed bundle delivery or startup separation exceeds that scope, perform a lawful breakdown before implementation. Default-relative path removal needs an explicit migration; owner-supplied contracts cannot be guessed or recovered from a neighbouring checkout. Source admission does not authorize deployment. The native board currently uses an unconfigured default task path; readback can establish this incoming artifact's visibility, not planning convergence or ready gates.
