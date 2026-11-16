# Historical lab iteration: preparation and exercised scope

**Historical snapshot for CrumbTrail 0.2.1.** The eight precision failures recorded below were closed in CrumbTrail 0.3.0: System 1/1 and Security 181/181 now have 0 ns differences, with the 1,000 ns threshold unchanged. See the [current offline evidence iteration](offline-evidence-iteration.md) for that result and the new BEC/HashBrown features. E2 and physical A2 remain unexecuted; the broader W2 matrix remains open.

Final bounded checkpoint: 2026-11-16, based only on owner packets supplied by the coordinator. The roadmap agent did not inspect sibling source, run tests, access tenants, inspect private event files, or operate capture targets. Exact published revisions and CI belong in the coordinator-owned [release ledger](release-validation.md).

BEC's published runner changes passed 64 fresh-wheel tests and base installed smoke; HashBrown's published helper changes passed 65 fresh-wheel tests/smoke. Published CrumbTrail 0.2.1 passed 56 fresh-wheel tests/smoke, and its runner, corpus manifest, and docs are complete. Windows/Linux CI is green for all three published tools. The coordinator records exact published revisions and CI evidence separately. These software checks do not close the qualification gates below.

The prior [offline feature iteration](current-iteration.md) delivered independent comparison, session, and preflight features. The current work evaluates those tools without introducing a shared package, installer, evidence store, runner, or runtime. Each owner keeps its lab code and fixtures in its own repository.

## Gate status

| Gate and owner | Prepared | Exercised | Qualification |
| --- | --- | --- | --- |
| E2 — BEC agent | Repository-only lab runner complete and published for the BEC 0.7.0 line | Live tenant run **NOT EXECUTED** | Open; designated profile, accounts, window, and provider-native expected events are missing |
| W2 — Windows triage agent | CrumbTrail 0.2.1 normalization/window fixes, runner, corpus manifest, and docs complete | System/window checks pass; Security identities/providers/logon fields agree, but strict 1-microsecond precision **FAILED 8/181**, maximum 1.3 microseconds | Partial; exact precision, broader OS/channel matrix, and responder signoff remain open |
| A2 — Acquisition agent | Repository-only helpers and runbook complete and published for HashBrown 0.2.0; fixture-only preparation checked hash mechanics | Physical-disk and RAM capture **NOT EXECUTED** | `not_granted`; designated VM/device/destination and human capture/independent-reader validation are absent |

No private host identifiers, event payloads, file paths, tenant identifiers, or evidence destinations are recorded in this public plan. The existence of a private file-only test does not authorize publication of its data or indicate that a live endpoint was collected by this iteration.

## E2: wait for designated email-lab inputs

**Owner: BEC agent.** The runner is preparation, not evidence of a successful provider connection or collection. The coordinator has requested the actual targets; none have been supplied.

Before execution, obtain a designated tenant profile, test account scope, collection window, and provider-native expected events suitable for comparison. Record references to sensitive configuration in the owner's private lab material rather than copying credentials or account details into this repository. Do not infer a target from ambient configuration.

Once those inputs are available, exercise the bounded runner against the designated scope. The acceptance packet must distinguish authentication and permissions, provider availability/retention/pagination, expected versus observed events, coverage gaps, and report/comparison behavior. Missing expected events require investigation or an explicit limitation; a successful process exit alone cannot close E2.

Until that exercise is performed, report E2 as **NOT EXECUTED**, even if all offline runner tests pass. No new provider connector is part of this milestone.

## W2: historical precision failure, closed in 0.3.0

**Owner: Windows triage agent.** CrumbTrail 0.2.1 fixes whole-second timestamp normalization by preferring the native timestamp and fixes subsecond native-window filtering. The final file-only results are:

| Comparison | Result |
| --- | --- |
| Real private System file against `Get-WinEvent` | 1/1 records agree |
| Fractional 150 ms window | 1/1 records agree |
| Empty window | 0/0 records agree |
| Security corpus selection | 789 native records; 181 selected; identities, providers, and logon fields all agree |
| Security timestamp precision | Strict 1-microsecond gate **FAILED for 8/181** selected records; maximum discrepancy 1.3 microseconds |

The coordinator also independently exercised the fresh installed wheel against the real System file with a fractional window: 1/1 records, zero mismatches, maximum 300 ns timestamp discrepancy, four expected gaps, and unchanged inputs. That successful bounded check does not override the Security-corpus failure.

The owner documented upstream FILETIME-to-float quantization behind the remaining precision discrepancy. The threshold was **not widened**, and the failing result is retained. System/window successes do not turn the Security precision gate into a pass.

Original and collector copies were unchanged. Windows may rewrite only its separately created oracle copy; that copy is distinct from preserved source evidence. No private identifiers or event payloads are included here. Runner, corpus manifest, and documentation are complete, with no further corpus work in this iteration.

At this 0.2.1 checkpoint, W2 was partial with exact precision, the declared Windows/Sysmon OS/channel matrix, and responder signoff open. The subsequent 0.3.0 precision closure is recorded in the [current iteration](offline-evidence-iteration.md); the broader matrix and review remain open. Keep private-file results separate from public reproducibility and matrix-wide qualification. Publication of 0.2.1 did not authorize remote collection or grant W2.

## A2: prepare without granting capture qualification

**Owner: Acquisition agent.** The published helpers and runbook prepare metadata, check prerequisites, and demonstrate hash mechanics with inert fixtures. They do not establish physical-device coverage, RAM-driver compatibility, image readability, or complete capture.

Before capture, obtain the explicitly designated VM, device/source, and evidence destination. The coordinator's request is unanswered; do not use another available device or destination. Physical-disk and RAM acquisition remain **NOT EXECUTED**.

Once designated resources are available, the acceptance packet must record the actual capture path, tool/driver/OS context, expected extent, read errors or other limitations, resource use, byte-integrity results, and independent image readability/content checks. A human must review the capture and independent-reader evidence. The qualification result remains `not_granted` whenever that capture/reader validation is absent; automated helper success or matching hashes cannot grant it.

## Handoff and next decision

Each owner returns only its own changed paths, final checks, exercised inputs by non-sensitive description, known limitations, and gate disposition. Distinguish **prepared**, **exercised on a stated input**, and **qualified for a declared matrix**. These are evidence boundaries, not interchangeable completion labels.

This historical iteration ended with preparation and bounded file-only exercises complete, E2 and physical A2 unexecuted, and the W2 precision failure preserved. Subsequent work closed that precision failure as documented in the [current iteration](offline-evidence-iteration.md). Current publication/CI references belong to the coordinator ledger; this snapshot does not describe the latest gate status.
