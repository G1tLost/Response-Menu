# Response Menu

A small planning repository for independent incident-response tools. It records scope, priorities, validation gaps, and handoffs so each tool can become useful on its own.

**This repository contains documentation only.** It has no executable toolkit, shared engine, dispatcher, orchestration service, installer, or required evidence store. Installing one response tool must not install or import another response tool.

## Independent projects

Each tool has its own public repository, package, installation and agent role.

| Repository | Responder outcome | Scope owner |
| --- | --- | --- |
| [BEC](https://github.com/G1tLost/BEC) | Investigate email/account compromise and produce an evidence-linked report | BEC agent |
| [CrumbTrail](https://github.com/G1tLost/CrumbTrail) | Build a Windows event chronology, surface useful observations, and show telemetry gaps | Windows triage agent |
| [HashBrown](https://github.com/G1tLost/HashBrown) | Acquire disk/RAM evidence and record transfer integrity and custody | Acquisition agent |
| [Response Menu](https://github.com/G1tLost/Response-Menu) | Keep priorities, boundaries, and acceptance evidence clear | Roadmap agent |

Each executable project targets its own pip-installable distribution, CLI, code, tests, CI, fixtures, dependencies, and documentation. Runtime dependencies on the other repositories are forbidden. A tool must work from its own checkout or installation without sibling repositories, submodules, or a common parent package.

Users may choose one tool or several. Each owns its local evidence format and reporting appropriate to its scope. Future file-based interchange may be proposed when a concrete workflow warrants it; it is neither implemented nor required by this plan.

## Published offline feature baseline

Historical results for the comparison/session/preflight releases; the lab checkpoint below records subsequent work:

| Tool | Delivered status and local evidence |
| --- | --- |
| BEC 0.7.0 | Published saved-case comparison; 53 tests, fresh-wheel isolated suite, and base-only installed smoke passed |
| CrumbTrail 0.2.0 | Published sessions CLI, `sessions.json`, and report section; 44 tests, fresh-wheel isolated suite/smoke, and native Velociraptor 0.77.3 inert-fixture/raw-verification checks passed |
| HashBrown 0.2.0 | Published preflight; 52 tests passed from a fresh independent wheel under Python `-I`; extended installed smoke passed JSON/text/failure paths with unchanged case bytes/mtimes, and both inert images verified |

These are owner/coordinator-reported historical results, not checks rerun by the roadmap agent. See the coordinator's [revision-specific validation ledger](docs/release-validation.md) for immutable published revisions, commands, and limitations; the latest lab checkpoint follows below.

BEC covers email/account compromise. CrumbTrail supports offline event imports, local exported-EVTX parsing, and session views; remote server orchestration remains deferred. Its native selection of event IDs is unchanged: the new logoff/startup boundary handling is available for imported records. Provider-aware derived event IDs can change when a case is rebuilt. HashBrown owns acquisition/intake, integrity, custody, and preflight, independently of both tools. Future memory/filesystem analysis remains separate work.

**Live RAM driver/device capture and physical-disk acquisition have not been tested.** Fixture transfer and hash verification do not prove that a complete or correct disk/RAM image was acquired.

## Current lab checkpoint

The final owner packets distinguish completed preparation from exercised validation. All three tools are published with passing fresh-wheel checks and green Windows/Linux CI. CrumbTrail 0.2.1 includes its completed runner, corpus manifest, and documentation. Exact revisions and CI evidence belong in the [release ledger](docs/release-validation.md).

| Gate | Preparation and exercised scope | Qualification status |
| --- | --- | --- |
| BEC E2 | Repository-only live-lab runner complete and published; no designated tenant profile, accounts, time window, or provider-native expected events supplied | Live exercise **NOT EXECUTED**; E2 remains open |
| CrumbTrail W2 | Whole-second normalization and subsecond native-window filtering fixed. System/window checks pass; Security identities/providers/logon fields agree, but the unchanged strict 1-microsecond gate **FAILED 8/181**, maximum 1.3 microseconds | W2 partial; exact precision, broader OS/channel matrix, and responder signoff remain open |
| HashBrown A2 | Repository-only helpers/runbook complete and published; fixture preparation verified hash mechanics. No designated VM, device, or evidence destination supplied | Physical disk/RAM capture **NOT EXECUTED**; qualification remains `not_granted` without actual capture and human independent-reader validation |

No private event data, paths, or host identifiers are included. The [lab iteration record](docs/lab-iteration.md) contains the final bounded results and outstanding inputs. No further corpus work is planned in this iteration.

## Next priorities

1. **BEC owner (E2):** retain the prepared runner; execute only after the tenant profile, accounts, window, and native expected events are designated, then compare collected evidence with those expected facts.
2. **CrumbTrail owner (W2):** retain the failed strict-precision result; exact precision, the declared Windows/Sysmon matrix, and responder review remain requirements for a later qualification exercise. The threshold has not been widened.
3. **HashBrown owner (A2):** retain the prepared helpers; execute only after the VM, device, and evidence destination are designated, then record actual capture and human independent-reader validation.

The [completed offline iteration](docs/current-iteration.md) records the comparison/session/preflight releases. The [lab iteration](docs/lab-iteration.md) records completed preparation and bounded file-only checks; requested tenant and VM designations remain unanswered. Those checks provide partial evidence and do not close the broader matrix or precision gates. Later memory/disk parsing still requires its own project and responder-value gate.

The [roadmap](ROADMAP.md) defines deliverables, prerequisites, tests, and one owner per milestone. [Agent handoffs](docs/agent-handoffs.md) define how owners exchange facts without repeating broad discovery. [AGENTS.md](AGENTS.md) and the [roadmap agent brief](agents/roadmap.md) limit work in this repository.

There is no automatic polling, background model/API daemon, or recursive agent delegation. The coordinator dispatches bounded work and integrates results. This repository does not run that process.

## Use the agents

Open the repository for the tool you want to work on in Codex. Its `AGENTS.md`, `agents/` brief and `.codex/agents/` definition describe that owner's scope. This planning repository includes only the `roadmap` agent; the other repositories include `bec_email`, `windows_triage` and `acquisition`, respectively. These are reusable instructions, not always-running processes. Role boundaries are working rules, not an operating-system sandbox. [Codex custom-agent configuration](https://learn.chatgpt.com/docs/agent-configuration/subagents)

For the roadmap agent, use a bounded request such as: “Read the release ledger and update the next milestone for each independent project. Use the supplied owner handoffs; do not inspect or change their source.” Assign an implementation task to the corresponding tool's agent in that tool's own checkout.
