# Response Menu

![Pixel-art diner with a stepped monitor chart and pixel steam](docs/assets/Response-Menu-Banner.gif)

[View the still banner](docs/assets/Response-Menu-Banner.png)

A small planning repository for independent incident-response tools. It records scope, priorities, validation gaps, and handoffs so each tool can become useful on its own.

**This repository contains documentation only.** It has no executable toolkit, shared engine, dispatcher, orchestration service, installer, or required evidence store. Installing one response tool must not install or import another response tool.

See [setup and prerequisites](docs/setup.md) to read, clone, or validate these planning documents. No suite installation is required.

## Pick a tool

[![BEC: bacon, egg and cheese sandwich](docs/assets/BEC.png)](https://github.com/G1tLost/BEC) [![CrumbTrail: toast](docs/assets/CrumbTrail.png)](https://github.com/G1tLost/CrumbTrail) [![HashBrown: hash brown](docs/assets/HashBrown.png)](https://github.com/G1tLost/HashBrown) [![Flapjacks: pancakes](docs/assets/Flapjacks.png)](https://github.com/FordTruckMonth/Flapjacks)

## Permissions at a glance

Response Menu is documentation only. Reading it or checking its Markdown links does **not** require Microsoft 365, Google Workspace, Active Directory, or endpoint administrator access. There is no shared service account or suite-wide permission grant.

| Tool | Microsoft 365 | Google Workspace | On-premises Active Directory | Local or host access |
| --- | --- | --- | --- | --- |
| [Flapjacks](https://github.com/FordTruckMonth/Flapjacks) | Approved delegated Graph reads and, for applicable checks, matching Exchange/Purview sessions. Identity-only sign-in does not unlock the audit scopes. | Not used. | Not used; Microsoft Entra is separate. | PowerShell and access to the checkout; write only if saving local results. [Authentication and setup](https://github.com/FordTruckMonth/Flapjacks/blob/main/terminal/docs/AUTHENTICATION.md) |
| [BEC](https://github.com/G1tLost/BEC) | Admin-consented Graph **application** permissions for the chosen live collectors, with separate trace provisioning where applicable. | Delegated service account and an authorized reports admin for Admin SDK audit reports; no Gmail mailbox access scope. | No native collector; no AD privileges required. | Read supplied exports and write the local case; no provider credentials for offline work. [Microsoft setup](https://github.com/G1tLost/BEC/blob/main/docs/security.md) · [Workspace setup](https://github.com/G1tLost/BEC/blob/main/docs/workspace.md) |
| [CrumbTrail](https://github.com/G1tLost/CrumbTrail) | No cloud permissions. | Not used. | No directory bind or cloud API permissions. | Access to the input Windows artifacts and local output. Collecting protected source artifacts separately may require channel- or host-specific access. |
| [HashBrown](https://github.com/G1tLost/HashBrown) | No cloud permissions. | Not used. | Not used. | Existing-evidence registration requires local file access. Direct acquisition is a separate workflow with its own platform prerequisites; don't grant host privileges for offline verification. |

Provision only the permissions for the mode you plan to use. The tool-specific README and setup docs are the source of truth for exact scopes, role assignments, licensing, prerequisites, and unsupported platforms. A missing source is a coverage gap, not a passing check.

## Independent projects

Each tool has its own public repository, package, installation and agent role.

| Repository | Responder outcome | Scope owner |
| --- | --- | --- |
| [BEC](https://github.com/G1tLost/BEC) | Investigate email/account compromise and produce an evidence-linked report | BEC agent |
| [Flapjacks](https://github.com/FordTruckMonth/Flapjacks) | Audit Microsoft 365 configuration with evidence-linked observations and explicit coverage gaps | M365 audit agent |
| [CrumbTrail](https://github.com/G1tLost/CrumbTrail) | Build a Windows event chronology, surface useful observations, and show telemetry gaps | Windows triage agent |
| [HashBrown](https://github.com/G1tLost/HashBrown) | Acquire disk/RAM evidence and record transfer integrity and custody | Acquisition agent |
| [Response Menu](https://github.com/G1tLost/Response-Menu) | Keep priorities, boundaries, and acceptance evidence clear | Roadmap agent |

Each executable project owns its distribution, CLI, code, tests, CI, fixtures, dependencies, and documentation. BEC, CrumbTrail and HashBrown target independent Python packages; Flapjacks is PowerShell and does not require Python. Runtime dependencies on the other repositories are forbidden. A tool must work from its own checkout or installation without sibling repositories, submodules, or a common parent package.

Users may choose one tool or several. Each owns its local evidence format and reporting. HashBrown's detached handoff format belongs to HashBrown; it does not introduce a shared schema or require another tool.

## Workflows and validation

Flapjacks is the Microsoft 365 posture auditor. The [workflow menu](docs/tool-workflows.md) separates released baselines from local proposals, defines target-read-only versus local evidence writes, and describes the optional mailbox snapshot v1 handoff. This is a documentation menu, not a launcher. Changes on `main` are not necessarily tagged releases or live qualification. The [release ledger](docs/release-validation.md) remains the source of validation status.

## Recorded published offline baselines

| Tool and owner | Implemented outcome | Supplied evidence and release state |
| --- | --- | --- |
| BEC 0.8.0 — BEC agent | Read-only, evidence-linked indicator search within a saved tenant-bound case | Reviewed and published; fresh base-wheel smoke, full suite, and CI passed |
| HashBrown 0.3.0 — Acquisition agent | Detached handoff creation and verification after relocation, without the source case; selected committed images are freshly hashed, limits retained, and images are not copied automatically | Reviewed and published; fresh-wheel suite/smoke and CI passed, with two explicitly recorded Windows suite skips |
| CrumbTrail 0.3.0 — Windows triage agent | Exact integer SystemTime ticks for 100 ns windows, ordering, and sessions, with immutable native raw evidence and a registered sidecar | Published; independent wheel, base smoke, isolated tests, actual System/Security native checks and Windows/Linux CI passed |

CrumbTrail closed **all eight former precision failures**: System 1/1 and Security 181/181 now differ by **0 ns** from independent `Get-WinEvent` results. The **1,000 ns threshold is unchanged**. Real single-tick inclusive and adjacent-instant window checks also passed. This closes the precision subgate on the exercised files, not the broader Windows/Sysmon matrix.

The [offline evidence iteration](docs/offline-evidence-iteration.md) records scope and limitations. Exact published revisions, test counts, skipped cases, and current CI belong in the coordinator's [release ledger](docs/release-validation.md). No unobserved CI outcome is implied here. No private identifiers or event payloads are included.

## Qualification still open

1. **BEC E2:** live tenant exercise **NOT EXECUTED**. The prepared runner awaits designated tenant/account/window/expected-event inputs. Those inputs are unnecessary for saved-case search.
2. **CrumbTrail W2:** exercised precision subgate passed; broader OS/channel coverage and responder signoff remain open. Portable case validation needs neither the original EVTX source nor the optional extraction parser.
3. **HashBrown A2:** physical disk/RAM capture **NOT EXECUTED**, with qualification `not_granted`. Actual capture and human independent-reader validation remain required. A capture VM is unnecessary for detached verification of already saved images.

Search, handoff verification, and the precision fix are independent offline improvements. They introduce no shared runtime, schema, installer, or evidence store. The [earlier offline release record](docs/current-iteration.md) and [historical lab record](docs/lab-iteration.md) remain available; their old test counts and precision failure are not the current status. Later memory/disk parsing still requires a separate project and responder-value gate.

The [roadmap](ROADMAP.md) defines deliverables, prerequisites, tests, and one owner per milestone. [Agent handoffs](docs/agent-handoffs.md) define how owners exchange facts without repeating broad discovery. [AGENTS.md](AGENTS.md) and the [roadmap agent brief](agents/roadmap.md) limit work in this repository.

There is no automatic polling, background model/API daemon, or recursive agent delegation. The coordinator dispatches bounded work and integrates results. This repository does not run that process.

## Use the agents

Open the repository for the tool you want to work on in Codex. Its `AGENTS.md`, `agents/` brief and `.codex/agents/` definition describe that owner's scope. This planning repository includes only the `roadmap` agent; the other repositories include `bec_email`, `windows_triage` and `acquisition`, respectively. These are reusable instructions, not always-running processes. Role boundaries are working rules, not an operating-system sandbox. [Codex custom-agent configuration](https://learn.chatgpt.com/docs/agent-configuration/subagents)

For the roadmap agent, use a bounded request such as: “Read the release ledger and update the next milestone for each independent project. Use the supplied owner handoffs; do not inspect or change their source.” Assign an implementation task to the corresponding tool's agent in that tool's own checkout.
