# Evidence and reporting method

## Evidence classes

| Class | Meaning | Example |
| --- | --- | --- |
| Documented statement | A source says this; no implementation check | Vendor lists Bluetooth discovery |
| Observation | Directly recorded result, bound to artifact/device/version | An identified APK exports a specific component |
| Inference | Interpretation supported by evidence but not established | A component likely participates in provisioning |
| Hypothesis | Question for later testing | Ownership checks might be incomplete |
| Verified finding | Reproduced security consequence with controls and scope | A lab account can access another controlled account's object |
| Not located | A bounded search did not find the item | No public dump found in logged queries |

No observations of app/device internals or verified findings exist at initialization. Baseline F IDs identify facts/statements; H IDs identify hypotheses; reserve `FIND-` IDs for actual findings.

## Artifact identity

Use [artifact-manifest.csv](../templates/artifact-manifest.csv). Each artifact needs an ID, acquisition date/time and route, original name, size, SHA-256, immutable original location, product/package/version/ABI, signer where relevant, parent artifact if derived, operator, and tool/version. A digest without provenance is not enough.

Do not invent a hash, version, endpoint, fuse value, or signer fingerprint to fill a template. Empty cells mean not yet recorded; write `unknown` in prose where that makes the limitation clearer. Treat two APK distributions/split sets and two hardware revisions as distinct samples.

Record actual execution commands, exit codes, stdout/stderr locations, interventions, and state before/after in [a lab session](../templates/lab-session.md). Planned commands stay labeled planned. Hash extracted partitions and connect them to the source dump plus offsets/lengths.

## Evidence locations and data handling

Commit sanitized documentation, bibliographic metadata, inventories stripped of sensitive values, and reports. Store future raw APKs, firmware, captures, logs, audio, household records, tokens, and private keys in a separately protected durable evidence location. Ignored paths such as `evidence/raw/` and `private/` are workspace conventions; they are neither encryption nor backups.

Use synthetic contacts, audio, networks, and test labels. Maintain an access-controlled mapping to real lab asset IDs where required. Replace secrets and private identifiers consistently in report copies. Avoid publishing bearer tokens, presigned links, actual home/emergency addresses, Wi-Fi passwords, and unique device IDs even in screenshots.

Vendor/private APK and firmware redistribution is not part of this repo's deliverable. Link public sources and record hashes/methods instead. Do not upload samples to public analysis services automatically.

## Finding criteria

Before using “vulnerability,” require:

1. A precise affected artifact/version/configuration and tested ownership scope.
2. A security boundary and expected control, with supporting rationale.
3. Minimal reproducible steps and actual results, not just a suspicious string.
4. Positive/negative controls and an explanation of alternate causes.
5. Evidence-supported impact and limitations, including necessary access.
6. A practical mitigation or concrete engineering question.

Use [finding.md](../templates/finding.md). Severity stays **unassigned** until impact/access are established. A policy concern, missing public whitepaper, generic dependency name, pinning status, or scanner warning alone does not meet these criteria.

## Publication review

Check that every claim has a source/evidence locator and appropriate class; no planned work is described as executed; third-party leads are not promoted to current-product facts; raw/sensitive files are absent; internal links resolve; and the remote head has not changed unexpectedly. Review and publish scoped documentation to `main`, preserving existing history.
