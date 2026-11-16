# Standalone release ledger

Recorded 2026-11-16 by the publishing coordinator. Each tool was built, installed and tested independently. These are observations of the listed revisions, not a production certification.

| Tool / version | Published revision | Local suite | Installed acceptance |
| --- | --- | --- | --- |
| BEC 0.7.0 | [b997172](https://github.com/G1tLost/BEC/commit/b997172611dd53af4d4c9eafe2e20b9ed05f0386) | 64 passed | Base demo, raw verification and saved-case comparison; 11 additional offline lab-runner checks; no live tenant collection |
| CrumbTrail 0.2.1 | [220c1e6](https://github.com/G1tLost/CrumbTrail/commit/220c1e613c2d0bd172b608980a896c69956fa031) | 56 passed | 21 inert events, four findings and zero gaps; fractional native timestamp/import window; packaged actual System EVTX in a 150 ms window: 1/1 events, zero mismatches, verified unchanged inputs |
| HashBrown 0.2.0 | [db395de](https://github.com/G1tLost/HashBrown/commit/db395deef7456a72f5f80dde16337f67a78317fe) | 65 passed | Two inert images verified; preflight leaves evidence unchanged; 13 additional helper checks for records, target/output isolation and controlled transfer using API doubles; no physical capture |

Local execution used Windows and Python 3.14.7. A separate fresh virtual environment was created for each wheel. All **185 tests** passed against the installed packages using Python isolated mode. Repository-only lab scripts were tested from their own checkouts against those installed packages. BEC's extended base smoke, including saved-case comparison, passed before optional libraries were installed; its full offline suite requires the documented `live` extra because transport tests import provider SDK types. CrumbTrail and HashBrown declare no third-party runtime Python dependencies. Import checks confirmed each package resolved from its environment's `site-packages` and neither sibling tool was available.

The preceding offline feature iteration published BEC [3c7dc77](https://github.com/G1tLost/BEC/commit/3c7dc77a51e16ef17c84fa6b5d283243202cf2fc), CrumbTrail [690b37a](https://github.com/G1tLost/CrumbTrail/commit/690b37a14185b7fe564e4795dc94eebe792afe9d) and HashBrown [9b6e19e](https://github.com/G1tLost/HashBrown/commit/9b6e19e8b3e544f70cf74d5ec33655d34940da06), with 53, 44 and 52 tests respectively. BEC and HashBrown retain their package versions because this iteration adds repository validation support; CrumbTrail's runtime fixes require patch version 0.2.1.

The prior standalone baseline was BEC 0.6.0 ([253c01c](https://github.com/G1tLost/BEC/commit/253c01cbd76f2bab352f22828b17d37c6367218a), 40 tests), CrumbTrail 0.1.0 ([9092e46](https://github.com/G1tLost/CrumbTrail/commit/9092e46d6e18dcc01555fc4e3227f0aef71f3672), 22 tests) and HashBrown 0.1.0 ([c15d5c7](https://github.com/G1tLost/HashBrown/commit/c15d5c7af5ae812b51fe551217c159a79d302feb), 30 tests). Those results are historical; the table above records the current independently validated revisions.

Each repository's `scripts/installed_smoke.py` invokes its own installed module with Python `-I` from a temporary working directory. This checks that source-checkout files and sibling projects are not supplying missing modules or packaged fixtures. Each tool's own CI repeats installation, tests, installed smoke and wheel build on Windows and Linux with Python 3.12.

Common local commands, run separately from each corresponding checkout/environment:

```text
python -m pip wheel . --no-deps --no-build-isolation -w <tool-specific-wheel-directory>
<clean-tool-python> -m pip install <that-tool-wheel>
<clean-tool-python> -I -m unittest discover -s tests -v
<clean-tool-python> scripts/installed_smoke.py
```

For BEC's full suite, install the wheel's `live` extra first. No live provider credentials are used by these tests.

## Delivered offline milestones

- BEC E3: `bec compare BEFORE_DIR AFTER_DIR` verifies and compares preserved raw evidence, including supported JSON transport wrappers. It exposes native-ID conflicts and coverage changes, refuses identity/integrity failures and leaves both cases unchanged. Missing observations never establish deleted configuration.
- CrumbTrail W3: `crumbtrail sessions --root CASE` produces source-ID views in JSON and the offline report. Host/context boundaries, repeated starts, missing IDs and ambiguous associations stay explicit. Supported imported Security 4634/4608 boundaries require the matching provider. Native selected IDs remain unchanged; provider-aware derived IDs may change when older reports are rebuilt.
- HashBrown A3: `--preflight` on its existing acquisition import/collect/sync commands checks local metadata, declared budgets, collisions, prerequisites and advisory analyst-volume capacity. It does not open image/credential contents, invoke a collector/API or modify evidence. Remote extent, driver readiness and capture completeness remain unknown. The Windows-only short-path test is skipped on Linux.

## Native fixture checks and their scope

For CrumbTrail 0.2.1, the owner performed actual file-only System and Security EVTX comparisons against Windows `Get-WinEvent -Path`, with pinned sources/hashes and separate reader copies. System full-window, fractional-window and empty-window checks passed. Security produced 181 selected events from 789 native records; every selected identity, provider and explicit source-logon field agreed. **Eight timestamps failed the unchanged 1,000 ns precision gate**, with a maximum 1,300 ns difference from upstream FILETIME-to-floating-point conversion. W2 remains partial. Both source and collector copies stayed unchanged; Windows altered only its separate disposable Security reader copy. The coordinator separately repeated the fractional System check against the final installed wheel. See [CrumbTrail's measured runbook](https://github.com/G1tLost/CrumbTrail/blob/220c1e613c2d0bd172b608980a896c69956fa031/docs/evtx-lab.md) for exact scope, limitations and resource measurements.

BEC's new runner performs bounded, explicit-target preflight and records live-origin, known-event, integrity, query-bound, coverage, pagination and restricted-access checks when executed. **E2 was not executed:** no designated tenant profile, accounts, time window or native expected events were supplied. Offline checker success does not qualify a provider. See the [BEC lab procedure](https://github.com/G1tLost/BEC/blob/b997172611dd53af4d4c9eafe2e20b9ed05f0386/docs/lab-validation.md).

HashBrown's new helper records explicit local file fingerprints, read-only image snapshots, bounded same-flow transfer checkpoints and manual-review assessments. Its separate fixture-only preparation matched both generated image hashes and reported `incomplete`, `qualification: not_granted`, with native capture and independent readability `not_executed`. **Physical A2 was not executed:** no designated VM/device/evidence destination was supplied. Remote helper tests used API doubles only. See the [HashBrown A2 procedure](https://github.com/G1tLost/HashBrown/blob/db395deef7456a72f5f80dde16337f67a78317fe/docs/a2-lab-acceptance.md).

For CrumbTrail 0.2.0, the Windows triage owner reran its actual Velociraptor 0.77.3 inert lab and original-artifact verification from the development installation. It produced 21 events, four findings, one duplicate and zero gaps, with one session start, three ID-linked events and 18 unlinked events. The coordinator separately validated the final wheel and extended installed smoke. Logoff/startup tests use synthetic imported records; the native artifact does not select those additional boundary IDs.

The earlier HashBrown 0.1.0 native test installed its three `Custom.HashBrown.*` artifacts on a temporary local Velociraptor 0.77.3 server. An enrolled Windows client uploaded generated disk and memory-transfer files. HashBrown verified the actual endpoint binding, preserved original API metadata, downloaded both server copies and matched their source SHA-256 values. An injected disconnect at 1,048,576 bytes recovered from the saved prefix using the same flow. Exactly two collections were submitted for two images; final verification passed with no acquisition gaps. The server was stopped. This is historical transfer evidence, not a new remote or physical-capture run for 0.2.0.

These fixtures contain no ransomware. The small FAT12 disk image is synthetic; the memory-transfer fixture is not a Windows RAM dump. The native test used file uploads, not the physical-disk or WinPmem capture paths.

## Hosted checks

- [BEC CI](https://github.com/G1tLost/BEC/actions/runs/43932791738): Windows and Linux jobs succeeded for the listed revision.
- [CrumbTrail CI](https://github.com/G1tLost/CrumbTrail/actions/runs/43933780048): Windows and Linux jobs succeeded for the listed revision; Linux skips the Windows-native helper guard.
- [HashBrown CI](https://github.com/G1tLost/HashBrown/actions/runs/43932800523): succeeded for the listed revision.

## Open qualification work

- BEC: authorized live tenant authentication, permissions, retention and actual provider behavior.
- CrumbTrail: strict Security timestamp precision; additional real Sysmon/Defender/PowerShell and boundary EVTX, the declared OS/parser matrix, responder review, resource/stress testing and telemetry completeness. Remote API scheduling is not implemented in v0.2.
- HashBrown: real physical-disk and WinPmem driver acquisition on the designated Windows VM; independent image readability/content checks; driver compatibility; read errors and large-image throughput. Hashes establish received-byte integrity, not capture completeness.

CI and local fixture successes do not close these qualification gates. See each tool's own validation document for its detailed procedure. No real cases, credentials, runtime executables or image files were committed to these public repositories.
