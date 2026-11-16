# Standalone release ledger

Recorded 2026-11-16 by the publishing coordinator. Each tool was built, installed and tested independently. These are observations of the listed revisions, not a production certification.

| Tool / version | Published revision | Local suite | Installed acceptance |
| --- | --- | --- | --- |
| BEC 0.7.0 | [3c7dc77](https://github.com/G1tLost/BEC/commit/3c7dc77a51e16ef17c84fa6b5d283243202cf2fc) | 53 passed | Demo and verification; equivalent saved cases yield zero delta; added sign-in yields one source-linked observation; HTML and unchanged input hashes |
| CrumbTrail 0.2.0 | [690b37a](https://github.com/G1tLost/CrumbTrail/commit/690b37a14185b7fe564e4795dc94eebe792afe9d) | 44 passed | 21 events, four findings, one duplicate, zero gaps; one observed session start, three ID-linked events and 18 unlinked events; references and raw evidence verified |
| HashBrown 0.2.0 | [9b6e19e](https://github.com/G1tLost/HashBrown/commit/9b6e19e8b3e544f70cf74d5ec33655d34940da06) | 52 passed | Two inert images verified; JSON/text preflight, invalid budget and missing-root paths; unchanged case hashes and modification times |

Local execution used Windows and Python 3.14.7. A separate fresh virtual environment was created for each wheel. All **149 tests** passed against the installed packages using Python isolated mode. BEC's extended base smoke, including saved-case comparison, passed before optional libraries were installed; its full offline suite requires the documented `live` extra because transport tests import provider SDK types. CrumbTrail and HashBrown declare no third-party runtime Python dependencies. Import checks confirmed each package resolved from its environment's `site-packages` and neither sibling tool was available.

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

For CrumbTrail 0.2.0, the Windows triage owner reran its actual Velociraptor 0.77.3 inert lab and original-artifact verification from the development installation. It produced 21 events, four findings, one duplicate and zero gaps, with one session start, three ID-linked events and 18 unlinked events. The coordinator separately validated the final wheel and extended installed smoke. Logoff/startup tests use synthetic imported records; the native artifact does not select those additional boundary IDs.

The earlier HashBrown 0.1.0 native test installed its three `Custom.HashBrown.*` artifacts on a temporary local Velociraptor 0.77.3 server. An enrolled Windows client uploaded generated disk and memory-transfer files. HashBrown verified the actual endpoint binding, preserved original API metadata, downloaded both server copies and matched their source SHA-256 values. An injected disconnect at 1,048,576 bytes recovered from the saved prefix using the same flow. Exactly two collections were submitted for two images; final verification passed with no acquisition gaps. The server was stopped. This is historical transfer evidence, not a new remote or physical-capture run for 0.2.0.

These fixtures contain no ransomware. The small FAT12 disk image is synthetic; the memory-transfer fixture is not a Windows RAM dump. The native test used file uploads, not the physical-disk or WinPmem capture paths.

## Hosted checks

- [BEC CI](https://github.com/G1tLost/BEC/actions/runs/43826523321): succeeded for the listed revision.
- [CrumbTrail CI](https://github.com/G1tLost/CrumbTrail/actions/runs/43826530099): succeeded for the listed revision.
- [HashBrown CI](https://github.com/G1tLost/HashBrown/actions/runs/43826907895): succeeded for the listed revision.

## Open qualification work

- BEC: authorized live tenant authentication, permissions, retention and actual provider behavior.
- CrumbTrail: exported-EVTX acceptance on the declared Windows/Sysmon matrix, responder review, throughput and telemetry completeness. Remote API scheduling is not implemented in v0.2.
- HashBrown: real physical-disk and WinPmem driver acquisition on the designated Windows VM; independent image readability/content checks; driver compatibility; read errors and large-image throughput. Hashes establish received-byte integrity, not capture completeness.

CI and local fixture successes do not close these qualification gates. See each tool's own validation document for its detailed procedure. No real cases, credentials, runtime executables or image files were committed to these public repositories.
