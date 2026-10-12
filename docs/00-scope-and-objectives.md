# Scope and objectives

Baseline date: **2026-10-12**. Phase: **P0 — recon and planning**. Owner: Dean Pierce. Target: Tin Can's commercial Wi-Fi phone and parental ecosystem. Specific APK version, firmware version, and physical unit: **not yet identified**.

## What we want to learn

| ID | Objective | Evidence needed to answer it |
| --- | --- | --- |
| O1 | Identify the app, handset, cloud, and telephony components and their trust boundaries. | Versioned APK inventory, board/module identification, documented or observed protocol map |
| O2 | Understand parent-account security and isolation between households. | Auth/session flows, ownership model, later controlled A/B-account tests with negative controls |
| O3 | Establish how device ownership is claimed, transferred, reset, and revoked. | Activation state machine, physical-presence proof, identity/credential lifecycle |
| O4 | Understand approved-contact enforcement in both directions. | Call-decision model, external caller authentication, mutual approval, revocation and stale-state behavior |
| O5 | Establish live-audio, voicemail, metadata, and microphone protections. | Media/key path, storage/access model, hook-switch circuit and firmware state behavior |
| O6 | Determine whether firmware and stored credentials resist extraction or modification. | Actual security-state evidence, acquisition result, signed-update and boot-chain observations |
| O7 | Produce reproducible results and useful mitigations. | Artifact hashes, source locators, repeatable steps, limitations, evidence-supported impact |

An acceptable answer can be “unresolved” or “extraction blocked by observed controls.” A successful project does not require finding a vulnerability.

## Current work

Public documentation, app-store metadata, regulatory exhibits, prior-work discovery, research plans, templates, and GitHub publishing. No APK download or analysis, accounts, endpoint tests, network/device scans, captures, disassembly, or flash operations are included now.

## Planned future lab scope

Use dedicated adult-controlled accounts, test handsets, an isolated network, and synthetic contacts/audio. Record asset ownership and the selected work package before beginning. Third-party accounts, strangers' phones, production enumeration, unsolicited calls, service disruption, and emergency calls are outside the proposed lab. Never dial emergency services to validate a theory.

Publicly known domains are reference pointers, not an authorization list for active testing. A private GitHub repo does not broaden the current scope.

## Product boundaries

The current research target is the Wi-Fi phone associated with Android package `app.tincan` ([S002](../sources/README.md#s002)). Flashback appears separately in vendor documentation; a reported Grandstream ATA workflow must not be applied to the ESP32 device ([S004](../sources/README.md#s004), [S016](../sources/README.md#s016)). An actual unit's labels take precedence over assumptions.

## Completion criteria

P0 is complete when the source baseline, unresolved questions, work packages, command notes, and evidence templates are reviewable. Later work must attach observed evidence to a specific artifact or device; claims about the full product fleet require additional coverage.

The vendor's current terms contain modification/reverse-engineering restrictions ([S009](../sources/README.md#s009)). Record this as research context without making an enforceability conclusion. No vendor contact, disclosure, or request for permission has been made by this project.
