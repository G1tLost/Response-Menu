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

As of 2026-11-16, the initial standalone versions are BEC 0.6.0, CrumbTrail 0.1.0 and HashBrown 0.1.0. Each was built as its own wheel, installed in a separate fresh environment, and exercised outside its source checkout. Their suites passed **40, 22 and 30 tests**, respectively. CrumbTrail and HashBrown run with the Python standard library; BEC has its own timezone and optional provider dependencies.

CrumbTrail's native Velociraptor fixture yielded 21 Windows events, four findings and zero gaps. HashBrown's native client/server fixture preserved both images and resumed an interrupted transfer at its saved 1 MiB checkpoint using the original flow. These are independent tests of the new packages. See the [revision-specific validation ledger](docs/release-validation.md) for commands, evidence and limitations.

BEC covers email/account compromise. CrumbTrail v0.1 supports offline event imports and explicit local exported-EVTX parsing; remote server orchestration is deferred. HashBrown owns image capture/intake and custody, with no dependency on either tool. Future memory/filesystem analysis is planned as separate work.

**Live RAM driver/device capture and physical-disk acquisition have not been tested.** Fixture transfer and hash verification do not prove that a complete or correct disk/RAM image was acquired.

## Delivery order

1. Preserve useful email investigation behavior in standalone BEC.
2. Deliver focused Windows event triage in standalone CrumbTrail.
3. Make acquisition and its limitations explicit in standalone HashBrown, then qualify its real device paths in an authorized lab.
4. Consider bounded memory or disk analysis as later separate projects only after responder value and maintenance capacity are demonstrated.

The [roadmap](ROADMAP.md) defines deliverables, prerequisites, tests, and one owner per milestone. [Agent handoffs](docs/agent-handoffs.md) define how owners exchange facts without repeating broad discovery. [AGENTS.md](AGENTS.md) and the [roadmap agent brief](agents/roadmap.md) limit work in this repository.

There is no automatic polling, background model/API daemon, or recursive agent delegation. The coordinator dispatches bounded work and integrates results. This repository does not run that process.

## Use the agents

Open the repository for the tool you want to work on in Codex. Its `AGENTS.md`, `agents/` brief and `.codex/agents/` definition describe that owner's scope. This planning repository includes only the `roadmap` agent; the other repositories include `bec_email`, `windows_triage` and `acquisition`, respectively. These are reusable instructions, not always-running processes. Role boundaries are working rules, not an operating-system sandbox. [Codex custom-agent configuration](https://learn.chatgpt.com/docs/agent-configuration/subagents)

For the roadmap agent, use a bounded request such as: “Read the release ledger and update the next milestone for each independent project. Use the supplied owner handoffs; do not inspect or change their source.” Assign an implementation task to the corresponding tool's agent in that tool's own checkout.
