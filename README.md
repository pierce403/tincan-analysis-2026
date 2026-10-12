# Tin Can analysis · 2026

Recon and research planning for the commercial [Tin Can](https://tincan.kids/) children's Wi-Fi phone, its parental app, and associated services.

**Status: public-source recon and planning only.** Started 2026-10-12. No APK has been acquired or decompiled, no handset has been opened or dumped, and no service has been tested. There are **zero verified vulnerabilities**. This is independent research, with no claimed affiliation with Tin Can.

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
| [Source register](sources/README.md) | Dated primary references and discovery leads |
| [Search log](sources/search-log.md) | Reproducible public-recon queries |

## First future work package

Acquire the official Android application from a controlled Android installation, preserve **every installed APK split**, establish package/version/signer/hash identity, then produce an offline component inventory. See [APK stage A](docs/03-apk-teardown-plan.md#stage-a-acquisition-and-identity). This work is planned, not started.

Hardware work begins with the actual unit's model/FCC label and board revision. The ESP32-S3 module evidence is a lead; it does not establish the configuration of every Tin Can sold.

## Contribution rules

Read [AGENTS.md](AGENTS.md). Cite source IDs; label vendor claims, observations, inferences, and hypotheses separately. Commit sanitized notes and metadata. Raw APKs, firmware, credentials, call audio, and captures stay out of Git. Templates live in [templates](templates/README.md).

Repository: <https://github.com/pierce403/tincan-analysis-2026> · working branch: `main`.
