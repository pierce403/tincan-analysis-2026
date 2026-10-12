# Instructions for this research repository

## Current scope

The initial user instruction is **recon and planning only**, with direct publishing to `main`. Do not interpret command examples, unchecked roadmap items, or hypothetical tests as instructions to execute them.

Permitted in the current phase: read public product/support/regulatory/tool documentation, search for prior work, maintain research plans, verify repository documentation, and commit/push those notes.

APK acquisition/decompilation, app/account use, traffic capture, API requests for testing, device pairing, disassembly, bootloader access, firmware extraction, instrumentation, or exploit development belong to future work. Wait for the user to move the project into an applicable execution phase. Do not create scheduled jobs or launch agents automatically.

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
