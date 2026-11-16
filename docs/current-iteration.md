# Completed iteration: offline responder workflows

This is the historical comparison/session/preflight release record; its counts and commits describe that release snapshot. Final subsequent lab preparation, bounded exercised results, and the still-open E2/W2/A2 gates are tracked in the [lab iteration](lab-iteration.md). Use the coordinator ledger for the latest revision of each tool.

Assigned and updated 2026-11-16 from owner packets supplied by the coordinator. BEC, CrumbTrail, and HashBrown have implemented, fixture-validated, and published their bounded improvements. This record summarizes supplied evidence; the roadmap agent did not inspect sibling code or rerun its tests.

| Milestone / version | Owner-reported evidence | Publication state |
| --- | --- | --- |
| E3 / BEC 0.7.0 | 53 tests; fresh-wheel isolated suite and base-only installed smoke passed | Published at `3c7dc77a51e16ef17c84fa6b5d283243202cf2fc` |
| W3 / CrumbTrail 0.2.0 | 44 tests; fresh-wheel isolated suite/smoke; actual Velociraptor 0.77.3 inert lab and raw verification passed | Published at `690b37a14185b7fe564e4795dc94eebe792afe9d` |
| A3 / HashBrown 0.2.0 | 52 tests passed from a fresh independent wheel under Python `-I`; extended installed smoke passed JSON/text/failure paths with unchanged case bytes/mtimes; both inert images verified | Published at `9b6e19e8b3e544f70cf74d5ec33655d34940da06` |

The coordinator's [release ledger](release-validation.md) owns immutable published references, exact commands, and release decisions. At this historical release handoff, CI was successful for all three listed revisions, including HashBrown run `43826907895`. These results are historical evidence rather than substitutes for acceptance of subsequent changes.

No live credentials, curated exported-EVTX corpus, or designated capture VM are available. No sibling imports, shared runtime/install/store, remote orchestration, broad new connectors, or automatic agent/API loops were added to this scope. The regression contracts below remain useful after implementation; scenario-level evidence belongs in each owner's validation record and the coordinator ledger.

## E3: saved-case comparison — BEC agent

Status: **implemented, offline tested, and published in BEC 0.7.0**. The saved-case comparison helps a responder identify evidence newly observed since an earlier collection. It compares recorded observations rather than proving that account configuration was created or deleted between runs.

The owner delivered `bec compare BEFORE_CASE_DIR AFTER_CASE_DIR [--format json|html] [--output FILE]`, with canonical raw-record differences, explicit tenant/customer identity, source/coverage differences, and unchanged input cases. Provider-specific state-diff engines and live collection remain excluded. The owner reports 53 tests and successful fresh-wheel isolated/base-only smoke checks.

Acceptance/regression contract:

1. Identical observations in differently named/reordered cases produce no new observations; local case IDs and input ordering do not fabricate a change.
2. A later case containing added sign-in, rule, and OAuth observations reports the expected additions with account/source context and evidence references.
3. Repeated rows do not inflate the result. Conflicting content under the same usable source identity is exposed, not silently overwritten. The owner documents a conservative fallback when stable identity is absent.
4. Non-overlapping windows, unavailable sources, and partial coverage produce explicit comparison limitations. Missing rows remain observations absent from that input, not inferred configuration removal.
5. Missing or altered evidence fails integrity validation visibly. Running comparison leaves both input cases unchanged; any output goes to an explicitly separate destination.

Next priority: E2, known activity in authorized tenants to validate actual source coverage and provider behavior. Credentials are unavailable. Offline comparison tests do not establish live authentication, permissions, retention, or collection completeness.

## W3: logon-session timelines — Windows triage agent

Status: **implemented, offline/native-fixture tested, and published in CrumbTrail 0.2.0**. The owner delivered a `sessions` CLI, `sessions.json`, and a report section. Sessions retain explicit source context and repeated-identifier ambiguity. Provider-validated 4634 logoff and 4608 startup records provide boundaries when present in imported data.

The owner reports 44 tests, fresh-wheel isolated suite/smoke, and an actual Velociraptor 0.77.3 inert lab with raw verification. The native selected event IDs did not change, so new boundary records are supported through imports only. Provider-aware derived event IDs may change on rebuild; consumers should retain raw provenance rather than assume derived IDs are stable across versions. Remote collection remains excluded.

Acceptance/regression contract:

1. Two logon sessions on one host remain separate; reused logon identifiers on two hosts never merge.
2. Known boundary/boot information prevents merging reused identifiers across distinct contexts. When available data cannot disambiguate reuse, expose the ambiguity instead of presenting a confident combined session.
3. Events missing session identifiers remain accessible as unlinked data. Intake rejects missing or mismatched host identity instead of assigning a host. Missing logon/logoff boundaries remain unknown.
4. Shuffled inputs and equivalent timestamp offsets yield stable chronological output with a documented tie-break rule; existing event counts and evidence references remain reconcilable.
5. A benign account/name match without an explicit session link does not attach the event. Every session record links back to its source event. Raw evidence remains unchanged; session exports and report additions are clearly derived outputs.

Next priority: W2, curated EVTX and responder acceptance for real Windows/Sysmon field behavior, identifier reuse, telemetry availability, usability, and throughput. The required corpus is unavailable. Native fixture parsing does not substitute for that corpus or establish native collection of the newly supported boundaries.

## A3: acquisition/transfer preflight — Acquisition agent

Status: **implemented, fixture validated, and published in HashBrown 0.2.0**. The fresh independent wheel passed 52 tests under Python `-I` and extended installed smoke checks for JSON/text/failure paths without changing case bytes or mtimes. Both inert images verified. Imports resolved from site-packages with no BEC or CrumbTrail installed. Preflight helps a responder catch known plan problems without certifying that capture will work or be complete.

The owner reports `--preflight` on import/collect/sync with JSON/text output and pass/warning/fail results (exit codes 0/2/1). Checks cover declared-byte estimates, regular local paths and ancestors, capacity, collisions, and prerequisites. Sync preflight may inspect metadata for an explicitly supplied original source; actual sync cannot redirect its source through that option. Preflight makes no source/tool/configuration content reads, network/executable calls, or evidence writes. Unknown extents and prerequisites remain limitations rather than guaranteed readiness.

Acceptance/regression contract:

1. An inert source with known size produces the expected storage calculation; insufficient capacity and invalid limits produce explicit blocking results. Test resource boundaries using controlled values rather than filling a real disk.
2. A conflicting destination and a source/destination resolving to the same file are rejected. Existing source and evidence files are never overwritten.
3. Missing required tool/configuration paths are reported. File existence or version metadata is not represented as proof of driver/device compatibility.
4. Unknown RAM/device size or unavailable source metadata remains unknown; the result explains which requirement cannot be evaluated and what the operator must supply or verify.
5. Every preflight path submits zero collection jobs, loads zero drivers, starts no capture, and leaves source files and existing case/custody state unchanged. Printed or explicitly requested preflight output is separate from acquired evidence.

Next priority: A2 needs the designated VM to compare preflight estimates with actual physical-disk/RAM acquisition, failure conditions, and independently readable known content. That VM is unavailable; real capture remains unqualified and no live collection was performed for this milestone. Transfer hashes alone cannot close A2.

## Remaining work and ownership

The offline improvements do not finish the overall roadmap. Prioritize E2, W2, and A2 when the required tenant access, corpus, and VM are supplied. Each gate has one tool owner and no cross-project runtime dependency. No new connector/parser initiative starts automatically while those resources are missing.

Each owner keeps meaningful fixtures, tests, and wheel checks in its own repository. The coordinator reviews handoffs, owns hosted CI/release decisions and immutable ledger entries, and pushes all repositories. The roadmap agent only updates these planning documents from supplied facts, then stops. Documentation checks do not substitute for tool verification, current CI, or live qualification.
