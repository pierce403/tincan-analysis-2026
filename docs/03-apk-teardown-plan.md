# Android APK teardown plan

**P1 acquisition started.** A third-party [APKMirror 3.1.2 (433) bundle](08-apkmirror-sample.md) has been preserved and its base plus 34 splits verified. The official installed Play split set and its provenance remain unknown. The offline teardown stages below have not been executed.

## Outputs

A verified artifact inventory; Android component/permission map; native/Flutter inventory; candidate endpoint and auth/provisioning state maps; version-supported dependency notes; and a small set of evidence-backed hypotheses for later controlled testing. Raw artifacts remain outside Git.

## Stage A: acquisition and identity

1. Select a dedicated adult-controlled Android device/profile. Record OS build, ABI, region, installation source, profile ID, and capture time. Do not use an active family account or import real contacts.
2. Install from the vendor-linked Play listing when this phase is started. Capture store metadata separately from the installed package identity. A listing's update date is not an APK version or hash.
3. Enumerate installed package paths, then pull **the base APK and every returned split**. Preserve the original filenames and sizes. A base-only extraction may omit the architecture-specific native code.
4. Hash each artifact. Verify each APK's signature; record signer certificate fingerprint, signature schemes, and available signing-lineage information. Verification alone establishes an internally valid signature, not publisher identity without a trusted provenance baseline.
5. Compare split identity/version/signing metadata. Record any discrepancy and stop treating the set as a coherent installed release until resolved.

Future official-installation acquisition examples, using [ADB](../sources/README.md#s018): replace `SESSION_ID`, device/profile values, and returned paths before use. These commands have not been run against a device.

```bash
TC_ANDROID_SERIAL='REPLACE_DEVICE_SERIAL'
TC_ANDROID_USER='REPLACE_PROFILE_ID'
TC_APK_DIR='evidence/raw/apk/SESSION_ID'
mkdir -p "$TC_APK_DIR"
adb -s "$TC_ANDROID_SERIAL" shell pm path --user "$TC_ANDROID_USER" app.tincan > "$TC_APK_DIR/package-paths.txt"
adb -s "$TC_ANDROID_SERIAL" shell dumpsys package app.tincan > "$TC_APK_DIR/package-metadata.txt"
# Repeat pull for EACH package: path in package-paths.txt, removing that prefix.
adb -s "$TC_ANDROID_SERIAL" pull 'REPLACE_RETURNED_APK_PATH' "$TC_APK_DIR/"
sha256sum "$TC_APK_DIR"/*.apk > "$TC_APK_DIR/SHA256SUMS.txt"
```

The selected user's package paths, not the default profile, determine the split set. Do not root or alter the research phone merely to obtain package files in the first pass. If access is blocked, document it and choose a controlled acquisition route later. Avoid unverified mirror APKs as the baseline; record any mirror sample separately with its provenance limitations.

Future signer and metadata examples ([S019](../sources/README.md#s019), [S020](../sources/README.md#s020)):

```bash
apksigner verify --verbose --print-certs "$TC_APK_DIR/base.apk"
apkanalyzer apk summary "$TC_APK_DIR/base.apk"
apkanalyzer manifest min-sdk "$TC_APK_DIR/base.apk"
apkanalyzer manifest target-sdk "$TC_APK_DIR/base.apk"
apkanalyzer files list "$TC_APK_DIR/base.apk"
```

Repeat signature/identity inspection on every split. These examples assume a base named `base.apk`; use the acquired filename instead if different. Add each file to a copy of [artifact-manifest.csv](../templates/artifact-manifest.csv), with raw location, SHA-256, acquisition route, version, signer, and tool versions.

**Exit:** coherent package/split provenance and identity; no analysis result yet.

## Stage B: offline structure and Android surface

Create fresh working copies; originals remain immutable. Record versions of SDK tools, JADX, Apktool, and any native analyzers. Use the actual tools' help to resolve version differences; no tool installation or execution is included in P0.

Example offline operations for a future acquired sample:

```bash
TC_APK_WORK='evidence/work/apk/SESSION_ID'
mkdir -p "$TC_APK_WORK"
apkanalyzer manifest print "$TC_APK_DIR/base.apk" > "$TC_APK_WORK/manifest.xml"
apktool d "$TC_APK_DIR/base.apk" -o "$TC_APK_WORK/apktool-base"
jadx -d "$TC_APK_WORK/jadx-base" "$TC_APK_DIR/base.apk"
```

Tools: [JADX](../sources/README.md#s022), [Apktool](../sources/README.md#s023). Process relevant splits in their own output directories and link results back to the original artifact. Decode errors and missing/obfuscated code are limitations, not evidence of malicious behavior.

| Review | What to record | Relevant question |
| --- | --- | --- |
| Manifest | Activities, services, receivers, providers, exported flags, intent filters, permissions | Unintended component entry points or overly broad access? |
| App links / WebViews | URI handling, origin checks, JS bridges, redirects | Can untrusted input trigger privileged behavior? |
| Storage / backup | Preferences, databases, credential handling, backup rules, logs | Where could tokens or household data persist? |
| Network configuration | Cleartext exceptions, trust anchors, debug overrides, custom verification | Are transport decisions appropriate on the actual code path? |
| SDK/dependencies | Library names, reliable versions, native build notes, license metadata | Which advisories actually apply? |
| Environment/config | Production/test endpoints, feature flags, public client identifiers | What is routing metadata versus an actual privileged secret? |

Android Network Security Configuration is one input ([S021](../sources/README.md#s021)); do not assume it covers native/Dart networking. SDK strings or public client API identifiers are leads, not vulnerabilities by themselves.

**Exit:** component inventory and evidence locators, with uncertainty explicitly attached.

## Stage C: Flutter and native paths

Vendor hiring material describes Flutter ([S010](../sources/README.md#s010)). Verify the sample before selecting a Flutter-specific approach. Inventory **all ABIs/splits**, looking for engine/application native libraries and Flutter assets; record ELF headers, build IDs, symbols, strings, and version clues where available.

JADX can help with the Android wrapper/plugins but should not be expected to recover Dart business logic compiled to native AOT code. Flutter documentation describes compiled Dart native libraries and AOT artifacts ([S024](../sources/README.md#s024)). Check native code and assets for the pieces absent from the DEX view. Do not assume compiler/engine compatibility for a third-party recovery tool; choose and pin tooling after identifying the sample. No such tool is selected or run now.

Priorities: Bluetooth plugin interfaces; secure-storage implementations; platform channels; auth redirects and token handling; networking/TLS call sites; device registration; contacts and quiet-time operations; update metadata; voicemail references.

**Exit:** code-path map with the distinction between DEX, assets, native plugins, and Dart/native logic preserved.

## Stage D: protocol and lifecycle reconstruction

Use [endpoint-inventory.csv](../templates/endpoint-inventory.csv) to track candidates. Record hostname/path, operation, object IDs, source locator, apparent auth, and whether the path is observed or only embedded. Do not send requests to confirm candidates in P0. Embedded test endpoints may be stale or unrelated.

Draw state machines for:

- Parent registration/login/recovery/logout and session refresh/revocation.
- Device discovery/activation/binding, Wi-Fi change, reset, and transfer.
- Contact request/approval/removal and quiet-hours changes.
- Call-history and voicemail retrieval/deletion, if present.

Distinguish client-side UI restrictions from server authorization; static code cannot establish the latter. Determine whether ownership is checked by household, device, line, or a separate membership object before designing tests.

**Exit:** candidate schema/state maps and a ranked, testable hypothesis list.

## Stage E: later controlled observation

Start only in a separately requested execution phase. Observe normal flows on a dedicated device/network first. Use lab accounts A/B, owned numbers/devices, and synthetic data. Record timestamps and successful own-object controls alongside rejected cross-object controls.

Capture DNS/TLS metadata and Bluetooth setup where feasible. An HTTPS proxy is a later option; user CAs may not be trusted, pinning may exist, and native stacks may behave differently. Document failed capture rather than treating it as proof of a security feature. Instrumentation or modified builds would need separate provenance because they can change behavior.

Do not enumerate production IDs, fuzz fleet endpoints, try leaked credentials, dial emergency services, or fetch unrelated users' data. End the session if a response unexpectedly contains another household's information and preserve only what is needed to document the event.

## Review framework and final deliverables

Use [OWASP MASTG](../sources/README.md#s017) as a review guide, selecting checks relevant to the observed implementation. Do not report an automated scan score as a finding.

Deliver: versioned inventory, component/protocol maps, observations using [the template](../templates/observation.md), unresolved questions, controlled test proposals, and findings only where the evidence meets [the finding criteria](07-evidence-method.md#finding-criteria).
