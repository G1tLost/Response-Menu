# Offline evidence iteration: search, handoff, and exact time

Checkpoint: 2026-11-16, based on tool-owner packets supplied by the coordinator. This document records implementation and review facts, not independent reruns by the roadmap agent. The coordinator's [release ledger](release-validation.md) is authoritative for exact published revisions, final wheel/test counts, skipped tests, and observed CI outcomes.

These changes require neither live provider credentials nor a designated capture VM. Each tool retains its own code, fixtures, CLI, dependencies, and format. There is no shared schema, runtime, installer, orchestration service, or mandatory evidence store.

| Milestone and owner | Status from supplied facts | Responder value |
| --- | --- | --- |
| E4 — BEC 0.8.0, BEC agent | Reviewed and published; fresh base-wheel smoke, full suite, and CI passed | Find supplied indicators in existing tenant-bound evidence with source references, without collecting again |
| A4 — HashBrown 0.3.0, Acquisition agent | Reviewed and published; fresh-wheel suite/smoke and CI passed. Two Windows tests were skipped for unavailable symlink creation/FIFO support, not counted as passes | Hand off explicitly selected saved images and verify them after relocation without the original case |
| W2 precision — CrumbTrail 0.3.0, Windows triage agent | Published; independent wheel passed base installed smoke before the optional extra, then the pinned extra and isolated suite; actual native runner checks passed | Preserve exact event time for boundary-sensitive windows, ordering, and session analysis |

The coordinator observed successful Windows and Linux CI jobs for all three published tools. CrumbTrail's first hosted Windows run exposed a path-alias bug in a test mock; the owner corrected the test, retained its growth/early-rejection assertions, and the final run passed. Product code and the independently exercised native timestamp path were unchanged by that test correction. Exact revisions and both CI outcomes are retained in the release ledger.

## BEC: search saved evidence

E4 adds read-only, evidence-linked indicator search within a saved single-tenant case. It complements saved-case comparison: the responder can inspect an indicator in one existing evidence set. The scope stays within email/account investigation, using the case's evidence and provenance rather than new provider collection.

Matches must be read with their field/context, source pointers, integrity state, and coverage limits. No match is not proof of no compromise or complete provider visibility. Retain source evidence and case identity; the search workflow does not grant E2 qualification or require credentials for offline use.

## HashBrown: detached handoff and recipient verification

A4 creates a portable handoff from explicitly selected committed case images, freshly hashing the selected files. The recipient can verify the relocated evidence from the detached handoff without the original source case. Images are not copied or moved automatically.

The handoff preserves partial status and custody limitations. A successful rehash confirms agreement with recorded reference bytes; it does not prove that a physical disk/RAM capture was complete, authenticate every custody claim, or promote acquisition qualification. The original source case is unnecessary for recipient verification, and no BEC or CrumbTrail installation is required.

Windows suite skips for symlink creation and FIFO availability are disclosed in the ledger. They remain skipped platform coverage, not passing evidence. Fresh-wheel smoke and coordinator-observed CI passed.

## CrumbTrail: all eight precision failures closed

CrumbTrail 0.3.0 reads exact BinXML `SystemTime` as integer ticks using its pinned optional `python-evtx==0.8.1` extra. The original native raw results stay immutable and a registered sidecar preserves exact values. Window inclusion, ordering, and session logic operate at 100 ns precision. Portable validation requires neither the original source EVTX nor the optional extraction parser.

| Actual native check | Supplied result |
| --- | --- |
| System file against independent `Get-WinEvent` | 1/1 records, 0 ns timestamp difference |
| Security selected records against independent `Get-WinEvent` | 181/181 records, 0 ns timestamp difference |
| Former precision failures | All 8 closed; original 1,000 ns threshold unchanged |
| Real single-tick inclusive window | 1/1 records |
| Adjacent-instant windows | 0/0 records |
| Evidence preservation | Original source and collector copies unchanged |

The coordinator reproduced System and Security results with the fresh installed wheel's native runner, including full provenance checks. Both checks passed with maximum delta 0 ns. Local exit code 2 correctly reported four missing channels in each case; those coverage gaps were not suppressed. For Security, Windows changed only the disposable oracle copy. The pinned-extra isolated suite also passed; exact counts and commands belong in the release ledger.

The earlier FILETIME-to-float discrepancy and failed 8/181 check are retained in the [historical lab record](lab-iteration.md). They are not the current precision result. No tolerance widening was used. This closes the precision subgate on the exercised files while retaining a separately verifiable record of the former failure.

W2 remains open for the broader Windows/Sysmon OS/channel matrix and responder signoff. Exact comparison on System and Security inputs does not establish every parser/channel combination or authorize remote collection. No private host identifiers or event payloads are included in this record.

## Gates and next decisions

- **E2 — BEC agent:** live tenant validation remains **NOT EXECUTED**. Prepared runner and designation request are unchanged; no new request is made by this offline iteration.
- **W2 — Windows triage agent:** precision subgate passed on exercised inputs. Broader OS/channel coverage, telemetry/session behavior, throughput, and responder signoff remain open.
- **A2 — Acquisition agent:** physical disk/RAM capture remains **NOT EXECUTED**, with qualification `not_granted`. Actual designated capture and human independent-reader/content/extent validation are still required.

The delivered offline work does not depend on those absent live resources and does not close their gates. The coordinator owns release checks, immutable evidence references, publication, and CI. The roadmap owner updates these documents only from supplied facts, checks their consistency and local links, then stops. No new connector, shared infrastructure, or wider parsing project is assigned by this record.
