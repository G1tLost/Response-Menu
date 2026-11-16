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

## Current status

Owner results received on 2026-11-16:

| Tool | Delivered status and local evidence |
| --- | --- |
| BEC 0.7.0 | Published saved-case comparison; 53 tests, fresh-wheel isolated suite, and base-only installed smoke passed |
| CrumbTrail 0.2.0 | Published sessions CLI, `sessions.json`, and report section; 44 tests, fresh-wheel isolated suite/smoke, and native Velociraptor 0.77.3 inert-fixture/raw-verification checks passed |
| HashBrown 0.2.0 | Published preflight; 52 tests passed from a fresh independent wheel under Python `-I`; extended installed smoke passed JSON/text/failure paths with unchanged case bytes/mtimes, and both inert images verified |

These are owner/coordinator-reported results, not checks rerun by the roadmap agent. See the coordinator's [revision-specific validation ledger](docs/release-validation.md) for immutable published revisions, commands, and limitations. This summary makes no claim about current hosted CI for the new revisions.

BEC covers email/account compromise. CrumbTrail supports offline event imports, local exported-EVTX parsing, and session views; remote server orchestration remains deferred. Its native selection of event IDs is unchanged: the new logoff/startup boundary handling is available for imported records. Provider-aware derived event IDs can change when a case is rebuilt. HashBrown owns acquisition/intake, integrity, custody, and preflight, independently of both tools. Future memory/filesystem analysis remains separate work.

**Live RAM driver/device capture and physical-disk acquisition have not been tested.** Fixture transfer and hash verification do not prove that a complete or correct disk/RAM image was acquired.

## Next priorities

1. **BEC owner (E2):** qualify the existing email and comparison workflow against known activity in authorized tenants once credentials are available.
2. **CrumbTrail owner (W2):** validate exported-EVTX/session behavior against a curated Windows/Sysmon corpus and a responder reviewer, including missing boundaries and repeated identifiers.
3. **HashBrown owner (A2):** qualify preflight and actual disk/RAM acquisition on the designated lab VM, including independently readable content and capture extent.

The [iteration record](docs/current-iteration.md) distinguishes delivered functionality, reported test evidence, and pending work. Live credentials, curated EVTX data, and a designated capture VM remain unavailable; offline success does not close those gates. Later memory/disk parsing still requires its own project and responder-value gate.

The [roadmap](ROADMAP.md) defines deliverables, prerequisites, tests, and one owner per milestone. [Agent handoffs](docs/agent-handoffs.md) define how owners exchange facts without repeating broad discovery. [AGENTS.md](AGENTS.md) and the [roadmap agent brief](agents/roadmap.md) limit work in this repository.

There is no automatic polling, background model/API daemon, or recursive agent delegation. The coordinator dispatches bounded work and integrates results. This repository does not run that process.

## Use the agents

Open the repository for the tool you want to work on in Codex. Its `AGENTS.md`, `agents/` brief and `.codex/agents/` definition describe that owner's scope. This planning repository includes only the `roadmap` agent; the other repositories include `bec_email`, `windows_triage` and `acquisition`, respectively. These are reusable instructions, not always-running processes. Role boundaries are working rules, not an operating-system sandbox. [Codex custom-agent configuration](https://learn.chatgpt.com/docs/agent-configuration/subagents)

For the roadmap agent, use a bounded request such as: “Read the release ledger and update the next milestone for each independent project. Use the supplied owner handoffs; do not inspect or change their source.” Assign an implementation task to the corresponding tool's agent in that tool's own checkout.
