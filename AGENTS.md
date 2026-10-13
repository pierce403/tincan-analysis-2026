# Instructions for this research repository

## Current scope

P0 recon and planning is complete. On 2026-10-13 the owner moved the project to **P1 for acquisition and offline static analysis of an APKMirror research sample**, with direct publishing of sanitized notes to `main`. The APKMirror bundle is a third-party sample, not an official Google Play installation baseline. Do not interpret command examples, unchecked roadmap items, or hypothetical tests as instructions to execute them.

Current work includes preserving the downloaded sample outside Git; recording source, hashes, package/version/split identity and signatures; offline APK decoding/decompilation and native-library inspection; and updating sanitized research notes. The later official Play installation remains separately tracked P1 work. No app/account use, traffic capture, API requests for testing, device pairing, bootloader access, firmware extraction, dynamic instrumentation, or exploit development is authorized by this phase change. Do not create scheduled jobs or launch agents automatically.

## Evidence discipline

- Use the statuses in [docs/07-evidence-method.md](docs/07-evidence-method.md).
- A vendor document is evidence of a vendor statement, not evidence of tested implementation.
- Never promote an unanswered security question into a finding. State affected package/version/board revision when known; otherwise say unknown.
- Source links and checked dates belong in [sources/README.md](sources/README.md). Date changes to claims. Public search non-results do not prove absence.
- Separate the ESP32 Wi-Fi product from Flashback/Grandstream leads. Separate the commercial product from unrelated projects named `tincan`.
- Keep future tooling commands in Markdown. No execution harness, downloader, scanner, or CI that contacts Tin Can services in this phase.

## Publishing and data

- Respect the requested `main` workflow. Inspect the remote head and preserve unrelated work; never force-push.
- Track only scoped text documentation and sanitized metadata. Do not commit raw artifacts, tokens, certificates containing private keys, account data, children's information, home addresses, or audio.
- Ignoring files does not secure or back them up. Verify the staged files explicitly before publishing.
- For documentation-only changes, check internal links, source references, examples, and the staged diff. No device or service tests are needed.
