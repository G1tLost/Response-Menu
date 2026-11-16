# Standalone release ledger

Recorded 2026-11-16 by the publishing coordinator. Each tool was built, installed and tested independently. These are observations of the listed revisions, not a production certification.

| Tool / version | Published revision | Local suite | Installed demo |
| --- | --- | --- | --- |
| BEC 0.6.0 | [253c01c](https://github.com/G1tLost/BEC/commit/253c01cbd76f2bab352f22828b17d37c6367218a) | 40 passed | 30 email/cloud events, zero gaps, raw evidence verified |
| CrumbTrail 0.1.0 | [9092e46](https://github.com/G1tLost/CrumbTrail/commit/9092e46d6e18dcc01555fc4e3227f0aef71f3672) | 22 passed | 21 Windows events, four findings from three rules, one duplicate, zero gaps |
| HashBrown 0.1.0 | [c15d5c7](https://github.com/G1tLost/HashBrown/commit/c15d5c7af5ae812b51fe551217c159a79d302feb) | 30 passed | Two inert image fixtures preserved and verified, zero acquisition gaps |

Local execution used Windows and Python 3.14.7. A separate fresh virtual environment was created for each wheel. BEC's base demo passed before optional libraries were installed; its full offline suite requires the documented `live` extra because transport tests import provider SDK types. CrumbTrail and HashBrown declare no third-party runtime Python dependencies. Import checks in their clean environments confirmed that neither BEC nor the other sibling tool was available.

HashBrown's local suite and native fixture checks ran at `26d2a3e`; `c15d5c7` changes only the installed smoke script to canonicalize Windows temporary path aliases. Its installed demo was also rerun successfully using an actual Windows short-path alias. The package code and native artifacts are unchanged between these revisions.

Each repository's `scripts/installed_smoke.py` invokes its own installed module with Python `-I` from a temporary working directory. This checks that source-checkout files and sibling projects are not supplying missing modules or packaged fixtures. Each tool's own CI repeats installation, tests, installed smoke and wheel build on Windows and Linux with Python 3.12.

Common local commands, run separately from each corresponding checkout/environment:

```text
python -m pip wheel . --no-deps --no-build-isolation -w <tool-specific-wheel-directory>
<clean-tool-python> -m pip install <that-tool-wheel>
<clean-tool-python> -m unittest discover -s tests -v
<clean-tool-python> scripts/installed_smoke.py
```

For BEC's full suite, install the wheel's `live` extra first. No live provider credentials are used by these tests.

## Native fixture checks

CrumbTrail's installed package ran `python -I -m crumbtrail lab --root <new-case> --velociraptor <runtime>` against the actual Velociraptor 0.77.3 Windows executable. It produced the expected 21 events, four findings, one duplicate and zero gaps with its own `Custom.CrumbTrail.LabReplay` artifact and preserved raw results.

HashBrown's installed package installed its three `Custom.HashBrown.*` artifacts on a temporary local Velociraptor 0.77.3 server. An enrolled Windows client uploaded generated disk and memory-transfer files. HashBrown verified the actual endpoint binding, preserved original API metadata, downloaded both server copies and matched their source SHA-256 values. An injected disconnect at 1,048,576 bytes was recovered from the saved prefix using the same flow. Exactly two collections were submitted for two images; final verification passed and no acquisition gaps remained. The temporary server was stopped after testing.

These fixtures contain no ransomware. The small FAT12 disk image is synthetic; the memory-transfer fixture is not a Windows RAM dump. The native test used file uploads, not the physical-disk or WinPmem capture paths.

## Hosted checks

- [BEC CI](https://github.com/G1tLost/BEC/actions/runs/43813039101): succeeded for the listed revision.
- [CrumbTrail CI](https://github.com/G1tLost/CrumbTrail/actions/runs/43813623242): succeeded for the listed revision.
- [HashBrown CI](https://github.com/G1tLost/HashBrown/actions/runs/43814008898): succeeded for the listed revision.

## Open qualification work

- BEC: authorized live tenant authentication, permissions, retention and actual provider behavior.
- CrumbTrail: exported-EVTX acceptance on the declared Windows/Sysmon matrix, responder review, throughput and telemetry completeness. Remote API scheduling is not implemented in v0.1.
- HashBrown: real physical-disk and WinPmem driver acquisition on the designated Windows VM; independent image readability/content checks; driver compatibility; read errors and large-image throughput. Hashes establish received-byte integrity, not capture completeness.

CI and local fixture successes do not close these qualification gates. See each tool's own validation document for its detailed procedure. No real cases, credentials, runtime executables or image files were committed to these public repositories.
