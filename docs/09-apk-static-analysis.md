# P1 offline APK analysis: Tin Can 3.1.2 (433)

**Status:** static review completed 2026-10-13 for the third-party [APKMirror sample](08-apkmirror-sample.md). Package `app.tincan`, version `3.1.2` (`433`); bundle SHA-256 `55d70b144d4745b365a37f6884ae86de62c879e50f22a4100a71185206233cb3`. This is the mirror bundle as downloaded, not an observed installed or patched application. No app was run, no account was used, and no Tin Can, Firebase, Google Maps, AWS, or Shorebird endpoint was requested for testing. **Zero verified vulnerabilities.**

## Method and coverage

| Input or tool | Offline result and limit |
| --- | --- |
| Base APK plus 34 splits | Base: eight DEX files, 414 asset entries, no native libraries. Three ABI splits (`arm64-v8a`, `armeabi-v7a`, `x86_64`) each contain six native libraries, including `libapp.so` and `libflutter.so`. The 7 density and 24 language splits were inventoried during acquisition; detailed review here centers on base and ABI splits. |
| Android `apkanalyzer` | Decoded the base manifest. Package/version/SDK values: `app.tincan`, `3.1.2`/`433`, minimum 24, target 36. |
| [JADX 1.5.6](../sources/README.md#s040) | Decompilation of base DEX produced Java sources with 137 reported errors among approximately 11,008 classes. The app's `MainActivity` is a thin `FlutterActivity` subclass. Decompiled plugin or SDK code is not evidence that a particular Dart call path uses it. |
| [Apktool 3.0.3](../sources/README.md#s041) | Decoded base resources/assets; reported unresolved references consistent with split resources. Asset observations below were checked against extracted archive entries. |
| ELF/`strings` | Inspected the three ABI copies of `libapp.so`; strings are candidates, not a call graph. ARM64 `libapp.so` is a stripped ELF with build ID `fd3d9b5c8bf892e6c38ee1bf2496d953`. |
| [AOTopsy source at `01011df`](../sources/README.md#s042) | The `doctor` command recognized the ARM64 ELF and AOT snapshot. Full parsing exited 1 at `fill: cluster 1008 (CID 8): obj 91/946 ref 0: stream: value too large`. The x86_64 snapshot also failed full parsing; see the [endpoint map](10-endpoints-and-parental-controls.md). Neither produced a trustworthy Dart call graph or decompiled app source. |

The JADX download SHA-256 was `545ea2be9c242511bc145755cf4bda2485ade42966e096f8b4d3da2a230e8974`; Apktool JAR SHA-256 was `dbf930b076c6b9be08d57c449cacefc3bdd6b71ebd59b3066fc0e1f5b14f9423`. AOTopsy was built locally from the pinned source with Go 1.26.0 and `golang.org/x/arch` v0.23.0. Tools and detailed outputs stay under ignored `evidence/work/`. Tool errors narrow the conclusions below.

The key command outputs were retained under `evidence/work/apk/2026-10-13-apkmirror-3.1.2-433/`: `manifest.xml` from `apkanalyzer manifest print` (exit 0), `apktool-base/` and `apktool.log` from `apktool d -s -f` (exit 0 with resource warnings), `jadx-base/` and `jadx.log` from `jadx --no-res -j 4` (exit 3), `strings/` from `strings` on each ABI's `libapp.so` (exit 0), and `aotopsy.log` from full AOT parsing (exit 1). The AOT diagnostic was `aotopsy doctor` on the ARM64 `libapp.so` (exit 0). All input paths pointed to extracted copies of the verified bundle; no target service or device was contacted.

## Observations and assessment

### Embedded configuration and candidate hosts

**Observation.** `base.apk` contains `assets/flutter_assets/.env.dev` and `.env.prod`. Both have the same 12 variable names: Firebase project/client configuration, `AUTH_BASE_URL`, `USER_DATA_API_URL`, and Google Maps API keys for iOS, Android, and web. The Firebase and Maps values are identical between the two files; the two base URLs differ. A separate `assets/flutter_assets/assets/environment_values/environment.json` has `tincanApiUrl` and `authUrl`. Sanitized host/path inventory:

| Asset | Embedded host or route | Interpretation |
| --- | --- | --- |
| `.env.prod` | `api.tincan.com/identity` (auth); `api.tincan.com` (user data) | Production-labeled configuration string; runtime selection unverified. |
| `.env.dev` | `cw6594f2pa.execute-api.us-west-2.amazonaws.com/dev` (auth); `4xeq93xxfl.execute-api.us-west-2.amazonaws.com/dev` (user data) | Development-labeled API Gateway configuration bundled in this release; reachability and access control unknown. |
| `environment.json` | `bzn8lpg267.execute-api.us-west-2.amazonaws.com/v3` (Tin Can API); `archie.tincan.kids` (auth) | Additional bundled configuration; relationship to the `.env` files and runtime routing unknown. |
| ARM64 `libapp.so` string pool | `https://archie.tincan.kids/wp-json/custom/v1/delete-key` | Candidate account deletion or key workflow endpoint. Its authentication, arguments, use, and implementation are unknown. |

**Inference.** Shipping `.env.dev` exposes development-labeled routing metadata to anyone with the APK. That is worth a configuration review, but the filenames and URLs alone do not establish a reachable development service, missing authentication, or data access. The shared Firebase values could be intentional; project separation and rules cannot be determined offline.

**Observation.** The two `.env` files contain Google/Firebase client API identifiers. A bounded pattern scan of decoded base asset/resource files up to 3 MiB each and the three `libapp.so` printable-string lists found four distinct `AIza`-format values across those surfaces. It found no AWS access-key ID, Stripe secret-key, GitHub/Slack token, JWT-like string, or URL userinfo match. Two `BEGIN ... PRIVATE KEY` marker matches were in Apache Tika's bundled MIME-signature XML, without matching key endings; they are not evidence of an embedded private key. This scan did not cover every large, opaque, or encoded value. No raw key value is recorded here.

Firebase documents that its client API keys identify a project and are generally public; restriction and backend authorization still matter ([S043](../sources/README.md#s043)). Whether these particular keys have suitable Android/iOS/web and API restrictions is **unknown**. No privileged hard-coded secret was verified.

### Android entry points and transport

**Observation.** The decoded base manifest exports `app.tincan.MainActivity` with an `https://app.tincan.com` App Link filter and `autoVerify="true"`; the filter has no path constraint. It also exports the Firebase `GenericIdpActivity` and `RecaptchaActivity`. Exported receivers/services are the Firebase Messaging receiver and instance-ID receiver (both require `com.google.android.c2dm.permission.SEND`), Google sign-in revocation service (requires its revocation permission), and AndroidX profile receiver (requires `android.permission.DUMP`). MainActivity's Java subclass adds no visible URL handling; the Dart handler could not be recovered. The manifest also requests contacts, Bluetooth, location, camera, and network permissions, among others. Export or permission declarations alone are not vulnerabilities. Android's App Link verification behavior is documented in [S045](../sources/README.md#s045); domain verification and incoming-link handling were not tested.

**Observation.** The base manifest does not declare `android:networkSecurityConfig` or `android:usesCleartextTraffic`; no decoded network-security XML was located. This is a bounded manifest/resource observation, not proof about Dart or native transport, certificate pinning, or server TLS. No app-specific trust-manager bypass was located in the generated Java source; JADX errors and Flutter AOT prevent a complete negative assertion.

### Dart strings, update mechanism, and limits

**Observation.** The ARM64 `libapp.so` printable strings include `/auth/login`, `/auth/refresh`, `/auth/request-reset`, `/account/devices`, `/contacts/v2`, `/calls`, `/device/status`, `/device/ota-status`, `/dial-plan`, `/emergency-address`, `/subscriptions`, and `/app-versions/check-upgrade`. Some similarly formatted strings are clearly UI routes. Without a Dart call graph, these are **candidate route strings**, not confirmed HTTP methods, authorization rules, or live endpoints. The local string list gives reproducible locators; it remains outside Git.

**Observation.** `assets/flutter_assets/shorebird.yaml` contains a Shorebird `app_id`, and `shorebird` strings occur in `libapp.so` and `libflutter.so`. Shorebird's documentation describes Dart code patches delivered outside an app-store release ([S044](../sources/README.md#s044)). This establishes bundled update metadata and engine clues; it does not establish that a patch exists or is active. A later installed-app assessment must record the applied patch state in addition to APK version/hash.

## Follow-up questions, not findings

1. Obtain a controlled Google Play installation and compare its exact split set/signing identity with this mirror bundle. Record whether a Shorebird patch is applied before comparing behavior to the base APK.
2. Review the release build configuration with the publisher: which of `.env.prod`, `.env.dev`, and `environment.json` is loaded, whether development services are intentionally packaged, and whether Firebase projects are shared by design.
3. In a separately authorized lab phase, use owned test accounts and synthetic data to trace the `app.tincan.com` link handler, authentication/account-deletion flows, and object ownership checks for device/contact/call routes. Static strings cannot answer these questions.
4. Check Google Cloud API-key restrictions, Firebase Security Rules, and App Check in an authorized configuration review; key presence in an APK is insufficient.

The original APK, decoded sources, logs, raw client keys, and generated string lists remain in ignored workspace paths. `.gitignore` prevents routine staging but does not encrypt or back up those files.
