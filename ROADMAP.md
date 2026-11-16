# Roadmap

Updated from owner handoffs on 2026-11-16. Statuses distinguish published offline functionality, lab preparation, exercised checks, and qualification. No fixed delivery dates are promised before designated targets, expected facts, and the support matrices are known.

## Status and boundaries

BEC 0.7.0, CrumbTrail 0.2.0, and HashBrown 0.2.0 form the historical offline-feature baseline recorded in the [completed offline iteration](docs/current-iteration.md). Subsequent BEC and HashBrown lab helpers and CrumbTrail 0.2.1 are published with passing fresh-wheel checks and green Windows/Linux CI. CrumbTrail's runner, corpus manifest, and docs are complete. The coordinator owns exact revisions, counts, and CI evidence in the [release validation ledger](docs/release-validation.md); the [lab record](docs/lab-iteration.md) preserves the bounded results and open gates.

BEC remains email/account focused. CrumbTrail remains offline/local; remote orchestration is deferred. HashBrown owns acquisition and custody in its own package. Live tenant qualification, exported-EVTX acceptance across supported systems, and actual RAM/physical-disk capture remain open. Offline releases do not close those lab gates.

`BEC` owns email/account compromise. `CrumbTrail` owns Windows event triage. `HashBrown` owns disk/RAM acquisition, integrity, and custody. `Response-Menu` owns planning documents. Every executable project must be independently pip-installable and have its own code, CLI, tests, CI, fixtures, and dependency declarations. None may import or require another project at runtime.

Keep functionality within its owner. Email provider clients stay in BEC; Windows event interpretation stays in CrumbTrail; acquisition and binary transfer stay in HashBrown. Future work consumes the relevant owner handoff. There is no shared engine or continuing synchronization of mixed-purpose implementations.

## Completed offline feature milestones

The three bounded milestones added responder-visible behavior through independent implementations. Their release-time evidence is summarized below; the detailed acceptance contract and limits remain in the [offline iteration record](docs/current-iteration.md). These counts are historical and do not represent the subsequent lab-helper changes.

| Milestone | Single owner | Delivered behavior | Evidence and remaining work |
| --- | --- | --- | --- |
| E3: compare saved email cases | BEC agent | BEC 0.7.0: saved-case comparison with evidence, identity, and coverage context | Implemented, offline tested, and published; 53 tests plus isolated-wheel suite/smoke. Next priority: E2 live tenant qualification |
| W3: inspect logon sessions | Windows triage agent | CrumbTrail 0.2.0: sessions CLI/export/report, provider-validated boundaries, and explicit repeated-ID ambiguity | Implemented, offline/native-fixture tested, and published; 44 tests plus isolated-wheel suite/smoke. Next priority: W2 curated EVTX and responder acceptance |
| A3: preflight acquisition plans | Acquisition agent | HashBrown 0.2.0: `--preflight` for import/collect/sync, capacity/collision/prerequisite checks, and JSON/text results | Implemented, fixture validated, and published; 52 tests plus isolated-wheel extended smoke. Next priority: A2 native capture qualification when the designated VM is available |

CrumbTrail's native selected event IDs are unchanged. New logoff/startup boundaries are handled in imported records, and provider-aware derived event IDs may change on rebuild. Its native inert-fixture success does not establish those boundaries across a real EVTX corpus. HashBrown preflight makes no source/tool/configuration content reads, network/executable calls, or evidence writes; known metadata and declared estimates do not prove capture compatibility or completeness.

Each project releases independently. There is no common package, schema, evidence store, or installation milestone blocking it. The coordinator owns Git, CI, publication, and the authoritative [release ledger](docs/release-validation.md). The roadmap owner records supplied facts and stops; it does not rerun sibling tests or assert new release evidence.

## Active lab priorities and gate status

E2, W2, and A2 remain the next priorities. Preparation and limited exercised evidence do not grant broad qualification. See the [lab iteration packet](docs/lab-iteration.md) for the required handoffs. The coordinator requested tenant and VM designations; no response has supplied them. Do not substitute an available account, host, device, or destination for a designated target.

| Gate | Single owner | Current preparation/exercised evidence | Next action and exit requirement |
| --- | --- | --- | --- |
| E2: live email qualification | BEC agent | Repository-only lab runner complete and published; fresh-wheel checks/CI pass. Live exercise **NOT EXECUTED** | Wait for the designated tenant profile, accounts, window, and provider-native expected events; then verify actual authentication, permissions, retention, pagination, source coverage, and observed evidence against expected facts |
| W2: Windows/Sysmon and responder acceptance | Windows triage agent | Code/runner/corpus work complete for 0.2.1. System and subsecond-window checks pass; Security identities/providers/logon fields agree. Strict 1-microsecond precision **FAILED 8/181**, maximum 1.3 microseconds, with threshold unchanged | W2 stays partial. Exact precision, a declared OS/channel matrix, and responder signoff remain open; retain the failure and documented upstream quantization. No further corpus work this iteration |
| A2: native disk/RAM qualification | Acquisition agent | Repository-only helpers/runbook complete and published; fresh-wheel checks/CI pass; fixture-only preparation verified hash mechanics. Physical disk/RAM capture **NOT EXECUTED** | Wait for designated VM, device, and evidence destination; perform actual capture and human independent-reader/content/extent validation. Qualification stays `not_granted` absent that evidence; record compatibility, read errors, interruption behavior, and throughput |

The final file-only results are bounded evidence, not qualification across every Windows version or event channel. Security-corpus precision remains a recorded failure attributed by the owner to upstream FILETIME-to-float quantization; the threshold was not widened. Original and collector copies were unchanged, while Windows may rewrite only its separate oracle copy. Hashes and successful transfer establish received-byte integrity against a reference, not acquisition completeness. No new connectors, shared runtime, or parser projects are authorized by this checkpoint.

## Responder outcomes first

BEC should help explain an email/account incident using available provider evidence and a reviewable report. CrumbTrail should help a responder understand observable Windows events, the affected host/account context, and what telemetry is missing. HashBrown should make it clear what was requested, what was obtained, whether transferred bytes verified, and what custody actions occurred.

Preserve distinctions between observation, hypothesis, and analyst confirmation. An empty result is not evidence of an uncompromised system when telemetry is absent, inaccessible, expired, or unsupported. Acquisition completeness, transfer integrity, and parser success are different claims.

## Later projects require a value gate

Memory analysis and disk/file timeline parsing belong in later **separate repositories**, each with its own owner and runtime. Before starting either, require a concrete responder question, a usable reference image with known facts, a bounded compatibility matrix, a prototype result that answers the question, and a maintainer able to support the dependency stack.

Volatility 3 is a candidate for memory analysis; its documentation separates analysis from acquisition, and Windows symbol resolution may require prepared offline dependencies. Plaso is a candidate for disk/file timelines; its tools extract and post-process events using their own storage format. These are reuse candidates, not integrations already implemented or requirements imposed on BEC, CrumbTrail, or HashBrown. [Volatility acquisition boundary](https://volatility3.readthedocs.io/en/latest/getting-started-linux-tutorial.html), [symbol handling](https://volatility3.readthedocs.io/en/latest/symbol-tables.html), [Plaso user guide](https://plaso.readthedocs.io/en/latest/sources/user/Users-Guide.html)

Optional file interchange can be proposed only when a real consumer exists. Preserve original evidence, source IDs, timestamps, hashes, and producer versions where applicable, but let each tool keep its own format and standalone use. Any proposed mapping needs its own small examples and compatibility tests. Do not create a mandatory shared schema library, database, case service, or evidence store.

## Capacity and acceptance language

Allow at most three active worker agents plus the coordinator. No recursive delegation. Each worker stays inside its assigned repository, consumes the supplied fact packet, and requests a targeted handoff instead of repeating whole-repository or ecosystem discovery. Start one bulk acquisition or analysis experiment per lab host/worker until measurements justify a different limit. Use repository-local fixtures and explicit storage/time budgets.

Use these labels consistently: **planned** (design only), **implemented** (code exists), **fixture validated** (bounded deterministic tests passed), **lab qualified** (declared live configurations exercised), and **field evaluated** (a specific build reviewed during an authorized real investigation). Record separate labels for separate capabilities. Mock tests never automatically advance a feature to lab qualified.

Fleet management, automated containment, ransomware execution, decryptors, a central incident-response platform, broad connectors, and automatic AI conclusions are outside this roadmap. Background polling, model/API loops, and unattended agent daemons are also excluded.
