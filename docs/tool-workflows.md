# Response Menu workflows

**Local review design, 2026-11-16. Not a runtime launcher or a new release.** Use the [release ledger](release-validation.md) for exact reviewed baselines and acceptance evidence. Each tool owns its code, CLI, dependencies, evidence and installation; none requires a sibling checkout. These runbooks select work and handoffs, not executable menu commands.

## Choose the question, not the whole suite

| Tool / evidence owner | Question | Accepted input in this tranche | Output / local writes | Target effects and limits |
| --- | --- | --- | --- | --- |
| BEC / BEC agent | What email/account activity is supported by the evidence? | Existing approved provider exports and saved tenant-bound cases; optional mailbox snapshot import under review | New local case, evidence-linked timeline/report or search output, with preserved original bytes | Offline only here. No message-body fetching, URL following, arbitrary GAM execution or live Purview search-job creation |
| Flapjacks / M365 audit agent | What M365 configuration was observed, and what could not be assessed? | Synthetic fixtures or already collected configuration snapshots; published and draft paths kept distinct | Local evidence/report and optional target-bound snapshot export under review | No live sign-in/collection in this tranche; no grants, remediation, rule changes, enrollment or configuration writes |
| CrumbTrail / Windows triage agent | What do supplied Windows events show, with what gaps? | Approved staged JSON/JSONL or explicitly supplied exported EVTX using the tool's supported local parser | Local case, preserved source records, chronology, findings and report | No ambient live-log default, remote hunt, agent deployment or endpoint acquisition in this tranche |
| HashBrown / Acquisition agent | Do existing evidence bytes match a reference, and what custody was recorded? | Already acquired images; generic artifact registration is new work under review | Explicit local registry/custody output and detached handoff manifest | No live capture, RAM driver, provisioning or endpoint operations. Case verification can write local journals/logs; detached verification is a distinct workflow |
| Response Menu / Roadmap agent | Which independent tool and acceptance gate fits the task? | Sanitized owner handoffs and pinned release facts | Planning documents and local validation records | No runtime, shared store, dispatcher, installer or live-service calls |

Tool-owned instructions: [BEC reviewed README](https://github.com/G1tLost/BEC/blob/07c4593215d11db0e8d2ebc57e2797e000c85e12/README.md), [Flapjacks published coverage](https://github.com/FordTruckMonth/m365-bec-investigator/blob/3490b92126d020794b8951dfa7ae0275cad94703/docs/COVERAGE.md), [Flapjacks draft terminal](https://github.com/FordTruckMonth/m365-bec-investigator/blob/eea3134e5969c2ac37343815b8bb9de43dafd865/terminal/README.md), [CrumbTrail reviewed README](https://github.com/G1tLost/CrumbTrail/blob/e1317275714c472b516db2a4576e20d59ea4f65c/README.md), [HashBrown reviewed README](https://github.com/G1tLost/HashBrown/blob/859c46f30767bd468f40de76a717c11e9b92bc52/README.md).

## Target-read-only is not zero filesystem writes

Preserve source bytes and work from permitted copies. Local evidence directories, derived reports, hashes and custody records are intentional writes, each with explicit destinations and overwrite behavior. A tenant read can still require authentication/consent and incur provider-side activity; “read-only” never authorizes it implicitly. The strict offline tranche excludes server-side search-job creation, remote capture, driver/agent staging and all remediation. Do not follow URLs or execute commands embedded in evidence. Never transfer private evidence to public repositories or production PendingUpdates.

## Bounded runbooks

### Phishing / account investigation

1. Establish tenant, exact account, incident window, available sources and known gaps from approved exports. Keep Entra distinct from on-premises AD DS.
2. Use BEC's documented offline import, verification and report paths. Label observed activity separately from interpretation.
3. If a compatible Flapjacks snapshot is supplied, verify tenant/mailbox binding and preserve its acquisition time, byte hash, evidence class and source/record pointer. Compatibility remains under review until the producer/consumer fixture passes.
4. Show present mailbox rules separately from historical create/update/delete events. Today's absence does not establish deletion or historic absence; today's presence does not prove presence during the incident. Denied/partial coverage is not an empty successful result.
5. Write derived output to an explicit local destination. Optional HashBrown registration must not become a prerequisite for BEC use.

### M365 configuration audit

1. Select Flapjacks' reviewed implementation explicitly. Published main's mailbox investigation and the draft tenant Audit / narrower Pull Logs are different scopes, not interchangeable releases.
2. For each audit control, report a stable ID, observed value, reference policy/version where applicable, evidence pointer and pass/gap/not-applicable/not-assessed disposition. Keep permission, licensing and collection gaps separate from configuration findings. Do not score unknown as compliant.
3. Use only synthetic/existing data for this tranche. A later live lab needs designated tenant/scope and separate authorization; no automatic grants or corrective actions.
4. Export optional mailbox state only with exact target binding and truthful evidence class. Preserve historical collector behavior until replacement parity is verified. Draft Pull Logs is not a full substitute for published historical audit collection.

### Endpoint-event triage

1. Supply explicit exported files and supported host/source context. Retain complete original EVTX separately from derived output.
2. Run the tool's offline validation and chronology/session interpretation. Test boundary events, reused IDs and ambiguous roles before drawing account/process associations.
3. Report missing channels, source precision and coverage; parser success and a clean timeline do not establish a clean endpoint. Broader W2 remains open.
4. Optionally hand off existing source and derived artifacts for custody, retaining classification and parent references. Do not initiate collection from the menu.

### Existing-evidence handoff

1. Identify existing evidence, scope, producer, original/derived class, acquisition bounds and partial status. Generic registration is under review; the published HashBrown baseline is image-centric.
2. Use the tool-owned supported workflow to hash selected files and create a detached manifest. Record any local case/journal writes separately.
3. Transfer evidence through an approved channel and verify at the recipient against the manifest, optionally with an independent reference hash. A manifest does not automatically copy images.
4. Retain partial states and failures. Matching hashes demonstrate agreement with reference bytes, not authenticity, complete acquisition or chain-of-custody trust by themselves.

## Optional mailbox snapshot v1

**Contract under local review, not a mandatory suite schema or shipped compatibility promise.** Flapjacks produces a JSON file; BEC consumes it independently. No shared runtime library, database or sibling import. Required envelope:

```json
{
  "schema": "response-menu.mailbox-snapshot.v1",
  "tenant_id": "00000000-0000-0000-0000-000000000001",
  "mailbox_upn": "analyst@example.test",
  "acquired_at": "2026-11-16T00:00:00Z",
  "synthetic": true,
  "evidence_class": "reserialized-source-objects",
  "sources": {
    "inbox_rules": {"status": "collected", "records": []},
    "forwarding": {"status": "denied", "records": []},
    "delegates": {"status": "unavailable", "records": []}
  }
}
```

- `tenant_id`: GUID, matched to the destination case; `mailbox_upn`: lowercase exact UPN, not a broad search or display-name match.
- `acquired_at`: ISO UTC acquisition time, distinct from historical event time; `synthetic`: boolean, carried through outputs.
- `evidence_class`: exactly `reserialized-source-objects`. Preserve source objects honestly; do not call these original HTTP response bytes.
- Each required source (`inbox_rules`, `forwarding`, `delegates`) has a `records` array and `status` of `collected`, `partial`, `denied` or `unavailable`. Do not coerce inaccessible or missing coverage to successfully collected empty data.
- BEC ingest computes and preserves the hash of supplied file bytes plus the source and record pointer. For example, `sources.inbox_rules.records[0]` identifies the first inbox-rule record. Original vs derived classification and transformation context must survive reporting.
- Reject mismatched tenant/mailbox and unbound live data. Synthetic test data must remain unmistakably synthetic. No self-reported “safe” marker permits live collection.
- Record field vocabulary and producer/consumer compatibility must be proven with the same bounded synthetic fixture before advancement. Do not infer a wider provider event schema from this envelope.

Acceptance packet: exact producer/consumer revisions, fixture byte hash, commands, record pointers, preserved target/acquisition facts and actual outcomes. Include hidden/still-present, delegate-created and subsequently deleted rule scenarios, denied/partial coverage, wrong tenant/mailbox, malformed input, tampering and snapshot-time/event-time differences. Failed and not-run checks stay visible. BEC and Flapjacks do not need each other for their primary standalone workflows. Resolve reuse/licensing separately before copying implementation code.
