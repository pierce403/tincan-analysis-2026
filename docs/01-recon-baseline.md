# Recon baseline

Checked **2026-10-12**. This is the historical P0 baseline: statements found in public sources, not results from examining a device or app binary. A later [P1 APKMirror sample](08-apkmirror-sample.md) has separate artifact identity observations.

## Documented statements

| ID | Statement | Source | Evidence class and limit |
| --- | --- | --- | --- |
| F01 | The product uses Wi-Fi, with a parent-managed approved-contact model. | [S001](../sources/README.md#s001) | Vendor product description; enforcement not tested |
| F02 | Android package is `app.tincan`; listing shows update date 2026-10-02. | [S002](../sources/README.md#s002) | Store metadata; installed version code/name and signer unknown |
| F03 | App features include approved contacts, calling hours, call history, and multiple-device management. | [S002](../sources/README.md#s002) | Declared features; no app execution |
| F04 | App settings include Wi-Fi and emergency-calling configuration. | [S005](../sources/README.md#s005) | Support documentation; no evidence Wi-Fi passwords are cloud-stored |
| F05 | Bluetooth and permissions are needed for discovery during setup. | [S007](../sources/README.md#s007) | Bluetooth confirmed; BLE/GATT details and provisioning protections unknown |
| F06 | Calling another Tin Can requires approval at both ends according to the FAQ. External callers must be approved. | [S006](../sources/README.md#s006) | Intended rule; caller authenticity and enforcement location unknown |
| F07 | Policy says live calls are not recorded; greetings and voicemail messages are recorded/stored. Device identifiers, call metadata, and quality metrics are collected. | [S003](../sources/README.md#s003) | Policy updated 2026-05-21; no implementation verification or E2EE conclusion |
| F08 | The TCM1 regulatory filing references Espressif certification `2AC7Z-ESPS3WROOM1`. | [S008](../sources/README.md#s008), [S038](../sources/README.md#s038) | Mirrored vendor letter; useful ESP32-S3-WROOM-1 lead, not a fleet-wide teardown |
| F09 | Engineering listing describes a Flutter iOS/Android app, ESP32 firmware, SIP, and a call-decision engine. | [S010](../sources/README.md#s010) | Vendor hiring description; no SDK, firmware, or protocol versions established |
| F10 | Backend/platform hiring mentions AWS/Node.js/TypeScript; a Communities role identifies APIs on AWS Lambda with Postgres. | [S011](../sources/README.md#s011), [S012](../sources/README.md#s012) | Evidence of company infrastructure; cannot map every app request to this stack |
| F11 | FAQ recommends shared login credentials while an additional primary-user feature is on the roadmap. | [S004](../sources/README.md#s004) | Current support wording; role/session behavior not examined |
| F12 | Play's developer declaration says data is encrypted in transit. | [S002](../sources/README.md#s002) | Self-reported app data-safety statement; no call-media encryption claim established |

## Inferences to verify

- **I01:** App/API/handset configuration likely forms a distributed control system. Exact calls, caching, push channels, and authority remain unknown.
- **I02:** Parent-account compromise could affect who may communicate with the handset. The scope and reversibility of resulting changes need observation.
- **I03:** Firmware may follow ESP-IDF conventions. Chip identification does not prove the SDK, RTOS, partition layout, or enabled security features.

## Unknowns

No verified package version, APK hash/signer, native-library inventory, device PCB revision, flash capacity, firmware build, service endpoint list, identity provider, MFA capability, API schema, session lifetime, device certificate, or provisioning protocol.

Also unresolved: live-audio topology/key custody; SIP TLS and RTP/SRTP use; voicemail encryption/access/retention; contact-removal propagation; local cached rules; hook-switch/microphone gating; secure boot; flash/NVS encryption; signed OTA; rollback controls; UART/JTAG/USB debug restrictions; reset/transfer credential invalidation.

## Source/open-source status

No official licensed product firmware source, public firmware dump, or detailed ESP32-product teardown was located in the bounded searches listed in [search-log.md](../sources/search-log.md). Status: **not located**, not “proven nonexistent.” Related open-source projects are distinguished in [prior work](05-prior-work.md).

## Interpretations to avoid

An approved phone number is not automatically authenticated caller identity. TLS does not establish E2EE. A simple user interface does not identify the underlying OS. Espressif platform support does not establish enabled protections. A policy promise is not a measured security property. A closed-source release status is not itself a vulnerability.
