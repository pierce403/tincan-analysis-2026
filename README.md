# Tin Can analysis · 2026

Independent research on the commercial [Tin Can](https://tincan.kids/) children's Wi-Fi phone, its parental app, and associated services.

**Status: P1 mirror-sample acquisition and offline static review complete.** Started 2026-10-12. An APKMirror 3.1.2 (433) bundle was acquired on 2026-10-13 as a third-party research sample; its base APK and 34 splits passed offline identity and signature checks. The base APK and ABI libraries were reviewed offline. This is not a controlled Google Play extraction; no handset has been opened or dumped, and no service has been tested. There are **zero verified vulnerabilities** and no claimed affiliation with Tin Can.

## Read first

| Document | Purpose |
| --- | --- |
| [Scope and objectives](docs/00-scope-and-objectives.md) | Research questions, current boundaries, success criteria |
| [Recon baseline](docs/01-recon-baseline.md) | Documented statements, inferences, unknowns |
| [Attack surface](docs/02-attack-surface.md) | Trust boundaries, assets, prioritized hypotheses |
| [APK teardown plan](docs/03-apk-teardown-plan.md) | Acquisition provenance, splits, Flutter-aware static review, later lab observations |
| [ESP32 acquisition notes](docs/04-esp32-acquisition-plan.md) | Board identification, read-first acquisition, security-state branches |
| [Prior work](docs/05-prior-work.md) | Relevant leads, exclusions, search limitations |
| [Roadmap](docs/06-roadmap.md) | Work packages and explicit exit criteria |
| [Evidence method](docs/07-evidence-method.md) | Artifact identity, reproducibility, reporting |
| [APKMirror sample](docs/08-apkmirror-sample.md) | Acquisition result, split verification, provenance limits |
| [APK static analysis](docs/09-apk-static-analysis.md) | Manifest, Flutter/native, configuration, and candidate endpoint observations |
| [Source register](sources/README.md) | Dated primary references and discovery leads |
| [Search log](sources/search-log.md) | Reproducible public-recon queries |

## P1 next work package

Acquire the official Android application from a controlled Android installation, preserve **every installed APK split**, establish package/version/signer/hash identity, and compare it with the [mirror sample and static review](docs/09-apk-static-analysis.md). The mirror sample does not satisfy the official acquisition gate. See [APK stage A](docs/03-apk-teardown-plan.md#stage-a-acquisition-and-identity).

Hardware work begins with the actual unit's model/FCC label and board revision. The ESP32-S3 module evidence is a lead; it does not establish the configuration of every Tin Can sold.

## Contribution rules

Read [AGENTS.md](AGENTS.md). Cite source IDs; label vendor claims, observations, inferences, and hypotheses separately. Commit sanitized notes and metadata. Raw APKs, firmware, credentials, call audio, and captures stay out of Git. Templates live in [templates](templates/README.md).

Repository: <https://github.com/pierce403/tincan-analysis-2026> · working branch: `main`.
