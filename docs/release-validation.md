# Standalone release ledger

## Recorded published baseline

Recorded 2026-11-16 by the publishing coordinator. Each tool was built, installed, reviewed and tested independently. These observations apply to the exact revisions below; they do not grant broad lab qualification.

| Tool / version | Published revision | Local installed suite | Installed acceptance |
| --- | --- | --- | --- |
| BEC 0.8.0 | [07c4593](https://github.com/G1tLost/BEC/commit/07c4593215d11db0e8d2ebc57e2797e000c85e12) | 77 passed | Base demo, raw verification, saved-case comparison and evidence-linked indicator search; no live tenant collection |
| CrumbTrail 0.3.0 | [e131727](https://github.com/G1tLost/CrumbTrail/commit/e1317275714c472b516db2a4576e20d59ea4f65c) | 76 passed | Base smoke, seven-digit ISO window, then actual System/Security EVTX through the final wheel: all 182 selected timestamps agree with Windows at 100 ns resolution, maximum difference 0 ns |
| HashBrown 0.3.0 | [859c46f](https://github.com/G1tLost/HashBrown/commit/859c46f30767bd468f40de76a717c11e9b92bc52) | 90 discovered: 88 passed, 2 skipped | Inert images, preflight and detached handoff; selected image manually relocated, original store hidden, recipient verification and tamper detection passed; no physical capture |

Local execution used Windows and Python 3.14.7, with a separate fresh virtual environment for each wheel. Across the three suites, **243 tests were discovered: 241 passed and two were skipped**. HashBrown's linked-ancestor integration test was skipped because this host cannot create symlinks; its FIFO test was skipped because Windows lacks `os.mkfifo`. Its Windows short-path alias test passed. Skips are not passes.

BEC's base installed smoke passed before optional libraries were installed; its full offline suite then used the wheel's `live` extra because transport tests import provider SDK types. CrumbTrail's base smoke passed before installing its pinned `evtx` extra (`python-evtx==0.8.1`); the full suite and actual local EVTX workflow used that extra. Completed exact cases remain verifiable without it. HashBrown has no third-party runtime Python dependency. Imports resolved each package from its own environment's `site-packages`, with both sibling packages absent.

Each repository's `scripts/installed_smoke.py` runs its own installed module under Python `-I` from temporary working directories. Repository-only acceptance runners use the corresponding installed package without injecting source paths. Common commands were run separately per project:

```text
python -m pip wheel . --no-deps --no-build-isolation -w <tool-specific-wheel-directory>
<fresh-tool-python> -m pip install <that-tool-wheel>
<fresh-tool-python> -I scripts/installed_smoke.py
<fresh-tool-python> -I -m unittest discover -s tests -v
```

Install the same wheel's documented optional extra before its full suite where required. No provider credentials or sibling installations were used. The final HashBrown wheel was rebuilt and its suite/smoke rerun after the last transaction-journal guard was added.

CrumbTrail's first hosted Windows run exposed a path-spelling bug in the input-growth test mock. The owner reproduced it with an alias path, corrected the mock's resolved-path matching and added assertions proving growth injection and zero native invocations. Only that test changed after the final wheel/native checks; product code is identical. The coordinator reran all 76 tests against the same installed wheel successfully. The [original failed run](https://github.com/G1tLost/CrumbTrail/actions/runs/43967169306) remains visible; the final run is listed below.

## Delivered offline milestones

- **BEC E4:** `bec search CASE_DIR TYPE VALUE` searches verified saved evidence for email, domain, IP, URL, SHA-256 or SHA-1 indicators. It reports original pointers, matching field/relation and query/field coverage, preserves conflicting native-ID variants and avoids double-counting duplicate evidence. URL matching preserves literal case/path/query/encoding and never visits links. JSON and inert escaped HTML output require a new destination outside the case. Source bytes and modification times remain unchanged; pending or changing evidence fails safely. See the revision's [search procedure](https://github.com/G1tLost/BEC/blob/07c4593215d11db0e8d2ebc57e2797e000c85e12/README.md).
- **HashBrown A4:** `hashbrown --root STORE handoff create CASE --job ACQ-ID --output NEW.json` freshly hashes explicitly selected committed images and emits a detached manifest. `hashbrown handoff verify --manifest FILE --evidence-root DIR` verifies relocated evidence without the source case, API or credentials. Images are not copied automatically. Original metadata and selected custody records are preserved; partial-capture status stays partial. An independently supplied manifest hash can anchor a reference; the manifest is unsigned. Missing/tampered files, unsafe paths, hardlink/reparse aliases, invalid schemas and pending source writes are covered. Exclusive atomic publication requires a hardlink-capable output filesystem. See the revision's [handoff procedure](https://github.com/G1tLost/HashBrown/blob/859c46f30767bd468f40de76a717c11e9b92bc52/README.md).
- **CrumbTrail W2 precision subgate:** exact `Event/System/TimeCreated/@SystemTime` FILETIME bytes now produce integer 100 ns timestamps. Source hash, XML identity, record/field offsets, exact bytes and original native-row hash are preserved in registered sidecars. Native raw evidence stays unchanged. Matching requires exact identity and the known upstream numeric conversion, without tolerance joins or Windows-oracle substitution. Window inclusion, ordering and session boundaries retain 100 ns precision. JSON-only numeric imports keep explicitly labeled microsecond behavior. See the revision's [exact-time design](https://github.com/G1tLost/CrumbTrail/blob/fdfbf879c28a347c88169151977cbf8e30918b3b/docs/exact-time.md).

Each implementation owns its own format and dependencies. No shared runtime, schema, installer or mandatory evidence store was added. BEC remains email/account focused; CrumbTrail remains Windows event triage; HashBrown remains acquisition, integrity and custody.

## Actual native timestamp acceptance

The CrumbTrail owner exercised the approved file-only System/Security corpus using Velociraptor 0.77.3, the pinned optional parser and independent Windows `Get-WinEvent -Path` on separate private copies. The coordinator repeated the System single-tick and full Security checks against the final fresh wheel. Corpus/collector hashes, provenance limits and reader versions remain in the revision's [lab runbook](https://github.com/G1tLost/CrumbTrail/blob/fdfbf879c28a347c88169151977cbf8e30918b3b/docs/evtx-lab.md).

| Gate | Observed final-wheel result |
| --- | --- |
| System selected event | 1/1, all projected facts and exact timestamp agree; record-header timestamp and ID differ from XML, and are correctly kept separate |
| Security selected events | 181/181 from 789 native-reader records; identities, providers, explicit source-logon fields and exact timestamps agree |
| Timestamp precision | Maximum difference **0 ns** for both sources; **all eight former failures closed**; original **1,000 ns acceptance gate unchanged** |
| Ordering/provenance/integrity | Exact integer timeline order, complete per-row provenance and raw/sidecar verification pass |
| Source preservation | Original and collector inputs unchanged; Windows changed only its separate disposable Security oracle copy |
| Coverage and exit status | Four expected missing channels per single-channel run; local CLI exit 2, overall acceptance runner exit 0 |

The owner additionally verified the real single-tick inclusive window (1/1) and each immediately adjacent 100 ns exclusion window (0/0), and replayed the inert native fixture (21 events, four findings, one duplicate, zero gaps and one verified original result). These bounded files do not qualify the wider W2 channel/OS/parser/responder matrix. Exact representation does not prove clock accuracy, causal ordering or complete telemetry. No live endpoint was collected by this iteration.

## Hosted checks

Each executable repository independently runs installation, tests, installed smoke and wheel build on Windows and Linux with Python 3.12. CrumbTrail runs its base smoke before installing the exact-parser extra. Platform-specific skips remain visible in job logs; hosted success does not mean every test ran on every platform.

- [BEC CI](https://github.com/G1tLost/BEC/actions/runs/43965863810): both jobs succeeded for `07c4593`.
- [HashBrown CI](https://github.com/G1tLost/HashBrown/actions/runs/43966523504): both jobs succeeded for `859c46f`.
- [CrumbTrail CI](https://github.com/G1tLost/CrumbTrail/actions/runs/43967714934): both jobs succeeded for `e131727`.

## Historical evidence

The [previous immutable release ledger](https://github.com/G1tLost/Response-Menu/blob/d00938a7287ca22920a1d79f0e1d00c74fd1e822/docs/release-validation.md) retains all earlier revisions, commands and native-transfer observations. The preceding checkpoint was BEC 0.7.0 (`b997172`, 64 tests), CrumbTrail 0.2.1 (`220c1e6`, 56 tests) and HashBrown 0.2.0 (`db395de`, 65 tests): all 185 local tests passed, but eight of 181 Security timestamps failed the unchanged precision gate, with maximum difference 1,300 ns. That defect is now closed; its historical record is preserved.

Earlier HashBrown native transfer of inert files and interrupted-download recovery did not exercise physical-disk or RAM-driver capture. The synthetic disk fixture and memory-transfer file do not establish complete device acquisition. Earlier comparison/session/preflight milestones remain in the [offline history](current-iteration.md), with subsequent lab preparation in the [historical lab record](lab-iteration.md).

## Open qualification work

- **BEC E2 remains NOT EXECUTED:** designated live tenant/account/window/native expected events, authentication, permissions, retention and actual provider behavior.
- **CrumbTrail W2 remains partial:** broader real Sysmon/Defender/PowerShell and supported boundary EVTX, declared Windows/export/parser versions, responder review, telemetry completeness and resource/stress testing. The exercised timestamp defect is closed. Remote scheduling remains unimplemented.
- **HashBrown physical A2 remains NOT EXECUTED:** designated Windows VM/device/destination, actual disk/WinPmem capture, driver compatibility, read errors, independent image readability/content/extent and large-image throughput. Qualification remains `not_granted` without that evidence.

Search and detached verification do not require those live resources. Matching hashes establishes agreement with reference bytes, not capture completeness or authenticity. No real cases, credentials, event payloads, runtime executables or image files were committed to the public repositories.


## Local review tranche: separate from published releases

### Version continuity and Git release conventions

- Keep each tool's existing version sequence and release conventions. The recorded published baselines for this review are **BEC 0.8.0**, **CrumbTrail 0.3.0**, and **HashBrown 0.3.0**. These local review changes do not bump package versions, create release tags, or reset numbering. A locally installed build can still display its base version; identify it by the base commit plus the reviewed local diff, not that number alone.
- Before an approved release, check the tool's actual upstream release history, then select its next version independently based on compatibility and scope. Keep the package metadata, CLI version, README, release notes, and tags (where the repo uses them) aligned. Do not use one suite-wide version or introduce a tagging scheme merely because the other repos use it.
- Flapjacks is a product rename of the existing M365 investigator, not a new version lineage. Keep the published main and draft audit baselines separate; do not invent a version or tag if the repository has no established one. Response Menu remains documentation-only and records each tool's released version, revision and validation; it does not define runtime versions.
- Evidence-contract versions are independent of product versions. For example, the mailbox snapshot schema stays at v1 unless its format changes incompatibly. Keep release/version changes and publication behind the separate Git approval.


The following is a planning/status structure, not a claim that new code has shipped. Historical counts above apply only to their pinned releases. Do not add local tests to those totals, promote a draft PR to main, or mark a capability qualified based on another tool's suite.

| Tool | Review base | Local change under review | Validation status for new work |
| --- | --- | --- | --- |
| BEC | `07c4593215d11db0e8d2ebc57e2797e000c85e12` | Optional mailbox snapshot import and incident view | 88 local offline tests passed; no new release or live E2 qualification |
| Flapjacks | Published main `3490b92126d020794b8951dfa7ae0275cad94703`; separate draft PR #1 `eea3134e5969c2ac37343815b8bb9de43dafd865` | M365 audit positioning and optional snapshot export | Mac PowerShell suite passed, including 152 toolkit checks, 50 mailbox snapshot checks and 167 audit assertions; not full tenant qualification or published main behavior |
| CrumbTrail | `e1317275714c472b516db2a4576e20d59ea4f65c` | Offline correctness and collection guardrails | 79 passed, 2 optional-parser errors, 1 Windows-only skip in local Mac run; synthetic demo passed, wider W2 still open |
| HashBrown | `859c46f30767bd468f40de76a717c11e9b92bc52` | Generic existing-evidence registration | 102 local tests passed, 2 platform skips; separate offline manifest handoff verification passed; no A2 capture qualification |
| Response Menu | `ac4f3af91025b305aaa81539ffa5637d04183d90` | Documentation menu, artwork, handoffs and ledger structure | Local Markdown/TOML, JSON-example and banner checks passed on macOS Python 3.11.15; not a hosted CI or sibling runtime claim |

Flapjacks baseline provenance: [published main](https://github.com/FordTruckMonth/m365-bec-investigator/tree/3490b92126d020794b8951dfa7ae0275cad94703), [draft PR source](https://github.com/FordTruckMonth/m365-bec-investigator/tree/eea3134e5969c2ac37343815b8bb9de43dafd865). The selected product name does not imply a GitHub rename. Draft audit and Pull Logs functionality must not be described as already published main behavior.

### Evidence required to advance each local row

Record a separate entry per capability:

```text
Tool / capability / accountable owner:
Base commit / local diff identifier / final commit if published later:
Status: planned | implemented | fixture validated | lab qualified | field evaluated
Inputs and expected facts (synthetic flag, source/evidence class):
Command / OS / runtime / isolated environment:
Actual passed / failed / skipped / not-run counts and limits:
Evidence location / artifact hashes / reviewer:
Target effects: none, or separately authorized exact scope:
Local writes: destinations, original preservation and overwrite behavior:
Interchange peer revision / fixture hash / observed compatibility (if tested):
Hosted run for exact published SHA: not run unless independently verified
Remaining qualification / next owner action:
```

Use this ledger as the single status source; workflow and roadmap documents link here rather than inventing new release numbers or test counts. Documentation CI checks local file targets and TOML parsing only; it does not verify remote links, sibling releases, schema interoperability, live safety or lab qualification. Branch protection and permission changes remain separate decisions.


### Response Menu local documentation check, 2026-11-16

Against the uncommitted local tree based on `ac4f3af`, the existing workflow's Python Markdown-target/TOML checks passed on macOS using installed Python 3.11.15: 32 local references across 11 Markdown documents. The optional-contract JSON example parsed and its required source/status structure passed a local assertion. This tests documentation syntax, not producer/consumer interoperability. `git diff --check` passed. The initial attempt with the macOS default Python 3.9.6 could not import `tomllib`; the successful rerun used the already installed Python 3.11.15, not a newly installed dependency. Hosted CI's Python 3.12/Linux environment was not exercised.

The approved banner was visually inspected and copied byte-for-byte; SHA-256 `aef2ed02edd030666f79440180a09122a2388f1f3b90f52b5b6f611110bc76b0`. No sibling runtime tests, live provider calls, endpoint operations, publication or new release validation were performed by the documentation task. Other local rows remain pending their owner packets.
