# Roadmap and work packages

Baseline: **2026-10-12**. This is a planning backlog, not a queue for automatic execution. Current authorization covers P0 documentation/public recon and GitHub publishing only.

## Phases

| Phase | State | Work package | Exit criteria / output |
| --- | --- | --- | --- |
| P0 | Complete in this snapshot | Public-source recon, objectives, methodology, plans | Source-linked baseline, hypotheses, templates, reviewed documentation for `main` |
| P1 | Not started | Official APK acquisition + offline teardown | Complete split set, hashes/signers, component/native inventory, auth/provisioning/request maps |
| P2 | Not started | Own-account/device baseline observation | Controlled normal flows, timestamped network/pairing metadata, observed endpoint/state maps |
| P3 | Not started | Hardware identity + read-first acquisition | Board map, security-state evidence, reproducible dump or documented blocking state |
| P4 | Not started | Controlled hypothesis validation | Minimal A/B-account/device tests with positive/negative controls, scoped impact and limitations |
| P5 | Not started | Findings, fixes, reporting | Reviewed evidence, mitigation recommendations, coordinated next steps requested by owner |

P1 can begin without a handset if an official installed app is available. P2 needs controlled accounts and suitable devices. P3 needs a spare physical unit and bench equipment. P4 test selection depends on P1/P2/P3 evidence; no hypothesis is a finding yet.

## Concrete backlog

| ID | Task | Depends on | Completion artifact |
| --- | --- | --- | --- |
| R01 | Identify available Android device/profile and intended handset model | Future phase selection | Filled inventory, sanitized scope/session record |
| R02 | Acquire official package and all installed splits | R01 | Hash/signer/version manifest, source provenance |
| R03 | Decode manifest/resources and inventory wrapper/plugins/native assets | R02 | Component and library tables, tool versions |
| R04 | Reconstruct parent-auth, ownership, and activation flows | R03 | State diagrams with code/evidence locators |
| R05 | Map contact, schedule, history, and voicemail operations | R03, R04 | Candidate request inventory, unresolved enforcement questions |
| R06 | Plan minimal normal-flow observation for two controlled households | R04, R05 | Scenario matrix, expected controls, capture plan |
| R07 | Observe local discovery and provisioning | Suitable owned handset, P2 | Observed transport/identity map; synthetic Wi-Fi credentials |
| R08 | Identify spare unit/board/module and measure interface levels | Future P3 selection | Device/PCB photos and pin map |
| R09 | Read security state, then select supported acquisition branch | R08 | ROM/fuse evidence and decision record |
| R10 | Acquire/triage image or document extraction limits | R09 | Verified hashes/lengths, partitions, provenance |
| R11 | Correlate app, firmware, and observed cloud/voice identities | R04, R05, R07, R10 | Consolidated architecture with evidence status per edge |
| R12 | Validate the highest-priority unresolved hypothesis | Evidence sufficient for a specific P4 session | Observation or supported finding with bounded impact |
| R13 | Prepare report and practical mitigations | R12 | Sanitized report and reproducibility references |

## First execution session to request later

R01–R03: **official APK acquisition and offline static inventory**. Required inputs: a controlled Android installation and chosen profile; no real contacts or family credentials. Finish with provenance, split coverage, manifest/permissions, native-library list, and a list of unconfirmed endpoint candidates. Do not roll straight into backend tests.

## Later scenario matrix

| Scenario | Owned assets needed | Expected invariant to investigate |
| --- | --- | --- |
| Household A attempts an operation on B's lab object | Accounts A/B and test objects | B's objects remain unauthorized to A |
| Parent logs out / changes credentials | Lab account and two app sessions | Old-session behavior matches documented revocation policy |
| Device is transferred between lab accounts | Spare handset and A/B accounts | Prior manager/device credentials lose intended privileges |
| Contact is removed while handset loses connectivity | Controlled devices/numbers | No unintended calls after the relevant enforcement point |
| Lab number makes approved/unapproved inbound calls | Owned numbers and handset | Caller authenticity and approval rules both hold |
| Synthetic voicemail is retrieved/deleted | Lab endpoints only | Ownership and deletion apply consistently |
| Handset is on-hook/off-hook | Spare unit, synthetic audio | Measured microphone behavior matches the actual circuit/state |

Specify expected results more precisely after normal behavior and documented policies are understood. No 911 test, production enumeration, arbitrary ID sweep, load test, forced update, or modified firmware install belongs to these baseline scenarios.
