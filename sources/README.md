# Source register

Entries S001–S038 were checked **2026-10-12**; S039 was checked **2026-10-13**. Source IDs are stable. Notes paraphrase only the relevant material; no full documents, APKs, or firmware are redistributed here. Vendor claims remain unverified implementation statements. Mutable `latest`/`stable` documentation must be rechecked and paired with pinned tools before future execution.

## Product and vendor sources

### S001

[Tin Can product site](https://tincan.kids/) — vendor primary source. Wi-Fi product, approved contacts, parent controls, and calling-plan description. No technical security verification.

### S002

[Tin Can on Google Play](https://play.google.com/store/apps/details?id=app.tincan&hl=en_US) — developer-provided listing. Package `app.tincan`, declared app features and data safety; visible update date 2026-10-02. No installed binary/version/signer collected.

### S003

[Privacy policy](https://tincan.kids/policies/privacy-policy) — vendor primary source, last updated 2026-05-21. Read sections on contacts/configuration, automatic phone information, call logs, voicemail, and children's data. Policy language, not an audio/storage audit.

### S004

[Set-Up + Activation FAQ](https://faq.tincan.com/category/set-up-activation) — vendor primary source. Activation and required companion app; FAQ's additional-primary-user answer recommends credential sharing. No auth/session tests.

### S005

[App + Account Basics FAQ](https://faq.tincan.com/category/app-account-basics) — vendor primary source. Wi-Fi/emergency settings, naming/caller-ID settings, and quiet-time controls. Does not establish each operation's protocol.

### S006

[Making Calls FAQ](https://faq.tincan.com/category/calling-basics) — vendor primary source. Mutual Tin Can contact approval, external approved callers, and separate phone/Flashback instructions. Intended behavior only.

### S007

[The app can't find my Tin Can](https://faq.tincan.com/t/m1yp577/the-app-cant-find-my-tin-can) — vendor primary source. Bluetooth discovery and permissions during setup. No GATT service UUIDs, pairing mode, or wire protocol established.

### S008

[Espressif authorization letter, FCC exhibit 8743287](https://fccid.io/2BR2I-TCM1/Letter/Old-Owner-Letter-8743287) — primary vendor filing, hosted by a third-party mirror. Letter dated 2025-10-11 references original certification `2AC7Z-ESPS3WROOM1` for Tin Can's change of ID.

Inspected through [the mirrored text conversion](https://manuals.plus/m/50f78d8dd8d95973146ec6c6bea4efdf9cf4a05f98fbe02b200eba5cee2b27b5). [Regulator attachment](https://apps.fcc.gov/eas/GetApplicationAttachment.html?id=8743287) was inaccessible in this pass. Not a retail-board photograph or firmware dump.

### S009

[Terms of service](https://tincan.kids/policies/terms-of-service) — vendor primary source, last updated 2026-01-06. Section 6 lists modification/reverse-engineering restrictions. Recorded as context; no legal enforceability conclusion.

### S010

[Senior Software Engineer, Test](https://jobs.ashbyhq.com/tin-can/3377be4e-779b-4d77-854a-afae953c3eee) — first-party hiring description, reviewed through indexed excerpts because the page rendered a JavaScript shell. Mentions Flutter, ESP32 firmware, SIP, and a call-decision engine. No artifact-level stack/version confirmation.

### S011

[Software Engineer, Communities](https://jobs.ashbyhq.com/tin-can/25cfbac2-6afd-4009-99d8-0a25227739b0) — first-party indexed hiring text. Describes APIs on AWS Lambda with Postgres. Describes a company subsystem, not proof of the entire phone service topology.

### S012

[Staff Software Engineer](https://jobs.ashbyhq.com/tin-can/8b3cbfac-5c7c-42a0-b913-7c6783dacb9e) — first-party indexed hiring text. Backend/platform role with Node.js, TypeScript, AWS and IoT provisioning/firmware-management experience. No identity provider or deployed API hostname established.

## Prior-work and comparative leads

### S013

[Open Lounge Phone — About](https://openloungephone.app/project/about/) — project's own description. Independent Tin Can-inspired open project; software/hardware licenses declared. Hardware and physical firmware validation were still developing at the checked date. Not commercial Tin Can source.

### S014

[GlomarGadaffi/tincan](https://github.com/GlomarGadaffi/tincan) — repository author's primary description. Separate ESP32-S3 push-to-talk SIP proof of concept. Name collision, not a product firmware release/dump.

### S015

[Hacker News product discussion](https://news.ycombinator.com/item?id=44587018) — discussion dated 2025-07-16. Discovery lead for public questions and related links. Comments do not establish current implementation or vulnerabilities.

### S016

[Repurposing a Tin Can Telephone ATA](https://www.reddit.com/r/VOIP/comments/1r6o4d4/repurposing_a_tin_can_telephone_ata/) — firsthand author report, independently unverified. Describes a Tin Can-branded Grandstream HT801 associated with Flashback. Relevant to historical hardware only unless unit identity matches.

## Android/mobile review references

### S017

[OWASP Mobile Application Security Testing Guide](https://mas.owasp.org/MASTG/) — primary methodology reference. Select implementation-relevant checks; not an assertion of Tin Can noncompliance.

### S018

[Android Debug Bridge](https://developer.android.com/tools/adb) — Android primary documentation. Target device selection, shell/package inspection, file pulling, and split installation context. Future command reference; no ADB connection made.

### S019

[apksigner](https://developer.android.com/tools/apksigner) — Android primary documentation. APK signature verification and signing-certificate output. A valid signature needs trusted acquisition/signer provenance to establish publisher identity.

### S020

[apkanalyzer](https://developer.android.com/tools/apkanalyzer) — Android primary documentation. Package/version summary, files, manifest, SDK metadata, permissions and component inspection. No sample analyzed.

### S021

[Android Network Security Configuration](https://developer.android.com/privacy-and-security/security-config) — Android primary documentation. Trust anchors, user/system CAs, and cleartext policy. Native/Dart paths require their own verification.

### S022

[JADX](https://github.com/skylot/jadx) — tool project's primary README. DEX/resource decompilation and CLI usage. Not a complete Dart/native recovery tool; version to be selected later.

### S023

[Apktool CLI parameters](https://apktool.org/docs/cli-parameters/) — tool primary documentation. Decode/output parameters; current page identifies 3.x docs. Future workflow must record the actual installed version.

### S024

[Flutter — Measuring your app's size](https://docs.flutter.dev/perf/app-size) — Flutter primary documentation. Compiled Dart native libraries and AOT artifacts provide context for native-aware review. Does not identify the target app's engine version.

## ESP32 and firmware references

### S025

[esptool basic commands, ESP32-S3](https://docs.espressif.com/projects/esptool/en/latest/esp32s3/esptool/basic-commands.html) — Espressif primary documentation. Flash identification, full read using `ALL`, ROM-only caveats, and offline image inspection. Read examples remain unexecuted.

### S026

[esptool advanced options, ESP32-S3](https://docs.espressif.com/projects/esptool/en/latest/esp32s3/esptool/advanced-options.html) — Espressif primary documentation. Reset modes and disabling the RAM stub. Options do not eliminate electrical/reset side effects.

### S027

[esptool advanced commands, ESP32-S3](https://docs.espressif.com/projects/esptool/en/latest/esp32s3/esptool/advanced-commands.html) — Espressif primary documentation. Security-information query; spelling in prose differs from current source CLI. No unrestricted read support should be assumed.

### S028

[Boot Mode Selection, ESP32-S3](https://docs.espressif.com/projects/esptool/en/latest/esp32s3/advanced-topics/boot-mode-selection.html) — Espressif primary documentation. GPIO0/GPIO46/reset requirements and boot-mode evidence. Actual retail-board routing remains unknown.

### S029

[espefuse summary, ESP32-S3](https://docs.espressif.com/projects/esptool/en/latest/esp32s3/espefuse/summary-cmd.html) — Espressif primary documentation. Read-only summary with text/JSON output, field protection, and masked unreadable values. Not a method to recover protected keys.

### S030

[ESP-IDF Security Overview, ESP32-S3](https://docs.espressif.com/projects/esp-idf/en/stable/esp32s3/security/security.html) — Espressif primary documentation. Platform security concepts. Supported controls do not establish Tin Can's configuration or SDK version.

### S031

[Flash Encryption, ESP32-S3](https://docs.espressif.com/projects/esp-idf/en/stable/esp32s3/security/flash-encryption.html) — Espressif primary documentation. Encryption-related fuses, encrypted regions, release/development context and download restrictions. Raw external flash reads do not defeat encryption.

### S032

[Secure Boot v2, ESP32-S3](https://docs.espressif.com/projects/esp-idf/en/stable/esp32s3/security/secure-boot-v2.html) — Espressif primary documentation. Boot trust, key purposes/digests and revocation fields. No target fuse/security state collected.

### S033

[Partition Tables, ESP32-S3](https://docs.espressif.com/projects/esp-idf/en/stable/esp32s3/api-guides/partition-tables.html) — Espressif primary documentation. Configurable table location, partition types and binary/CSV conversion. Common offsets are candidates, not confirmed target values.

### S034

[NVS Encryption, ESP32-S3](https://docs.espressif.com/projects/esp-idf/en/stable/esp32s3/api-reference/storage/nvs_encryption.html) — Espressif primary documentation. Storage-protection mechanisms distinct from simple whole-image assumptions. Target use unverified.

### S035

[esptool v5 migration guide](https://docs.espressif.com/projects/esptool/en/latest/esp32s3/migration-guide.html) — Espressif primary documentation. Console-script invocation and breaking changes. Pin versions and check help before using future notes.

### S036

[esptool CLI source](https://github.com/espressif/esptool/blob/master/esptool/__init__.py) — upstream primary source, mutable branch inspected. Confirms `get-security-info` spelling and restricted-mode command gating. Record a concrete tool version/commit for future execution; no target-specific conclusion.

### S037

[ESP-IDF security advisories](https://github.com/espressif/esp-idf/security/advisories) — upstream primary advisory index. Starting point after exact versions/configuration/reachability are known; no advisory applicability assessed in P0.

### S038

[FCC filing index: 2BR2I-TCM1](https://fccid.io/2BR2I-TCM1) — third-party regulatory index linking primary exhibits. Filing links original Espressif certification. Index/provenance aid, not evidence of installed security settings or a complete handset board.

### S039

[APKMirror Tin Can 3.1.2 (433) bundle listing](https://www.apkmirror.com/apk/tincan/tin-can/tin-can-3-1-2-release/tin-can-3-1-2-android-apk-download/) — checked 2026-10-13. Third-party distribution page for package `app.tincan`; lists a base APK and 34 splits, bundle size 75,295,463 bytes, SHA-256 `55d70b144d4745b365a37f6884ae86de62c879e50f22a4100a71185206233cb3`, and signer certificate SHA-256 `05f2274d345cd61b1dddc489371d3584ee073b1f2b7fb7def647d4a162ff0be8`. The locally downloaded bundle matched the listed size/hash. This is not an official Google Play extraction or independent publisher-identity baseline; see [the acquisition record](../docs/08-apkmirror-sample.md).
