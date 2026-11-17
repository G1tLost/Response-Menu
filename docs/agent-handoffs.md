# Agent handoffs

Handoffs transfer the facts needed for one bounded task. They do not transfer ownership of another repository or authorize broad discovery. The coordinator dispatches work manually, with at most three active worker agents plus the coordinator and no recursive delegation.

## Ownership map

| Owner | Repository and exclusive subject | Useful handoff facts |
| --- | --- | --- |
| BEC agent | BEC: email/account compromise | Provider/input coverage, email CLI/package behavior, report evidence links, standalone checks, live API limitations |
| M365 audit agent | Flapjacks: Microsoft 365 configuration audit | Published/draft revision distinction, stable control IDs, permission/licensing gaps, snapshot export class and exact target binding, PowerShell checks |
| Windows triage agent | CrumbTrail: Windows event triage | Supported event inputs, chronology/findings behavior, source locators, time handling, coverage gaps, standalone checks |
| Acquisition agent | HashBrown: disk/RAM acquisition and evidence handling | Source/transport support, resume/hash/custody behavior, partial states, standalone checks, real acquisition qualification limits |
| Roadmap agent | Response-Menu: planning | Milestones, dependencies, acceptance language, owner assignment, unresolved planning decisions |
| Coordinator | Publishing and integration administration | Authoritative revision/status ledger, repository destinations, explicit task assignments, owner-supplied acceptance results |

Each executable repository owns its own code, fixtures, tests, CI, CLI, dependencies, and evidence files. Neither this map nor a handoff creates a shared runtime or mandatory file contract.

## Optional mailbox snapshot handoff

The [mailbox snapshot v1 proposal](tool-workflows.md#optional-mailbox-snapshot-v1) is an optional Flapjacks producer / BEC consumer contract, not a shared package. The coordinator supplies the same schema and synthetic fixture to both owners. Each owner keeps its implementation and tests in its own repository. Exchange fixture hash, exact revisions, invocation, result and coverage dispositions before claiming interoperability. A schema agreement alone is not a passing integration test. Do not copy unlicensed implementation code between projects.

The strict tranche consumes existing offline files and creates only explicitly selected local outputs. It does not grant tenant authentication, remote collection, search-job creation or remediation. Local evidence writes are not target writes, but must still preserve originals.

## Task packet

Before starting, provide the owner with:

1. The responder outcome and exact repository/path ownership.
2. Relevant baseline facts and their source; avoid a whole-history dump.
3. Required deliverables and acceptance checks.
4. Explicit exclusions and dependencies on another owner's answer.
5. The expected completion packet and any resource constraints.

If a missing fact blocks work, send one focused request naming the question and why it matters. Continue independent work where possible. Do not start a duplicate provider survey, acquisition investigation, or repository scan while waiting. The coordinator decides any reassignment.

## Completion packet

Use this compact form:

```text
Owner / repository:
Revision or working-tree state:
Owned files changed:
Delivered behavior or documents:
Validation: command, environment, outcome, saved evidence location:
Known limits / untested paths:
Next owner action or narrow missing fact:
```

For tests, distinguish checks run by the current owner from inherited results. Keep fixtures and validation records in the relevant repository; public examples must be sanitized and suitable for redistribution. Do not include credentials or real case material in a public handoff.

## Minimum evidence for claims

- **Standalone installation:** record a clean environment, install command, CLI exercise, and absence of sibling-repository imports/dependencies.
- **Fixture validated:** identify the local fixture, expected facts, command, revision, and actual result.
- **Live provider qualified:** identify the authorized test configuration, expected activity, observed coverage, and limits.
- **Native acquisition qualified:** record actual device/driver use, tool and OS versions, expected acquisition extent, independent image readability/content checks, and separate transfer/integrity outcomes.
- **Documentation complete:** list documents reread, local links checked, consistency checks, and unresolved assumptions.

The earlier combined engine's 71-test result is not standalone acceptance evidence. A fixture-server transfer cannot qualify live RAM or physical-disk acquisition. When a required check cannot run, record it as untested or blocked and leave that gate open.

After sending the completion packet, stop the task. Do not keep polling, launch another agent, or create a background model/API process. Follow-up work needs an explicit coordinator assignment.
