# P1 endpoint and parental-control map from APK 3.1.2 (433)

**Checked 2026-10-13.** This extends the [offline static review](09-apk-static-analysis.md) of the third-party [APKMirror sample](08-apkmirror-sample.md), package `app.tincan`, version `3.1.2` (`433`). The ARM64 `libapp.so` used for string locators has SHA-256 `08db4efee9cac7f13c4c36c7eaeafcabb932f4cce46c0bdbc6d1bc8dc616bb6b`. No app execution, traffic capture, target API request, or device test occurred. **Actual connections, request bodies, and server/handset enforcement are unobserved.**

## Configured hosts, not observed connections

| Source in base APK | Embedded base or URL | Evidence status |
| --- | --- | --- |
| `assets/flutter_assets/.env.prod` | `api.tincan.com/identity` for `AUTH_BASE_URL`; `api.tincan.com` for `USER_DATA_API_URL` | Observation: production-labeled values; runtime selection unknown. |
| `assets/flutter_assets/.env.dev` | `cw6594f2pa.execute-api.us-west-2.amazonaws.com/dev` for auth; `4xeq93xxfl.execute-api.us-west-2.amazonaws.com/dev` for user data | Observation: development-labeled values shipped in the bundle; activity/reachability unknown. |
| `assets/flutter_assets/assets/environment_values/environment.json` | `bzn8lpg267.execute-api.us-west-2.amazonaws.com/v3` for `tincanApiUrl`; `archie.tincan.kids` for `authUrl` | Observation: additional configuration; precedence and runtime use unknown. |
| ARM64 `libapp.so` printable strings | `https://archie.tincan.kids/wp-json/custom/v1/delete-key` | Observation: full URL embedded in AOT string pool; use and authorization unknown. |

The APK also bundles Firebase Auth, Firestore, Messaging, Remote Config, Performance, and Crashlytics integration classes and Shorebird update metadata. These identify possible service families, not a measured connection inventory. The Firebase/Maps client identifiers in the asset files are not request bodies or privileged secrets by themselves; see the [static review](09-apk-static-analysis.md#embedded-configuration-and-candidate-hosts).

## Candidate path and request families

All paths below are **standalone AOT strings**. The APK does not show which configured host they join, which HTTP method uses them, whether they are active, or whether some are UI routes. Locators refer to ignored `evidence/work/apk/2026-10-13-apkmirror-3.1.2-433/strings/arm64-libapp.txt` line numbers.

| Family | Candidate strings and locators | Associated app-specific symbols | What can be concluded |
| --- | --- | --- | --- |
| Auth/account | `/auth/login` (20082), `/auth/refresh` (30802), `/auth/request-reset` (6281), `/account/devices` (23935) | `AuthNotifier.signIn` log strings describe `authRepo.signIn` (4619), Firebase anonymous auth (22558), current-profile fetch (23499), `updateFirestoreUid` (10878/23335), then session save (29138). | Static names imply a multistep sign-in design; credentials, tokens, methods, and exact destinations are unknown. |
| Contacts/calling permissions | `/contacts/v2` (18764), `/community-contacts/` (17087), `/contact/` (26489), `/dial-plan` (21105) | `ContactClient` (20988), `ContactStateManager` (9694), `enableContact` (10150), `disableContact` (5053), `enableCommunityContact` (4055), `disableCommunityContact` (33082), `updateCommunityContactDialPlanOrClear` (5040), `UpdateCommunityContactsDialPlanRequestDto` (6214). | App code contains client-side contact approval/disable and dial-plan update concepts. No server authorization or call-enforcement behavior is established. |
| Availability/Quiet Mode | `/availability` (15329), `/time-condition/by-type` (15442), `/time-conditions` (16756) | `AvailabilityClient` (13638), `setSchedule` (10429), `updateDnd` (25892), `UpdateDndRequestDto` (15943), `UpdateAvailabilityTypeOneRequestDto` (4112), `UpdateAvailabilityTypeThreeRequestDto` (17740), `CurrentAvailabilityResponseDto` (26462), `TimeConditionResponseDto` (16471). | App code contains schedule and Do Not Disturb update/read concepts. The URL-to-method and request schemas remain unresolved. |
| Device/call status | `/device/status` (27452), `/device/ota-status` (19252), `/calls` (18634) | `DeviceRepository` (9533), `DeviceStatusResponseDto` (18510), `AddDeviceNotifier._pollDeviceStatus` (27704). | Candidate status/call surfaces; no parental-control propagation path was recovered. |

The same AOT pool contains scalar names `account_id`, `subscription_id`, `contact_id`, `phone_number`, `phone_number_id`, `approved`, `dial_plan`, `start_time`, `end_time`, `time_condition_id`, `timezone`, `dnd_enabled`, `dnd_until`, and `dnd_type`. These could be request fields, response fields, local model fields, or combinations. **Do not interpret this list as any actual JSON body.** The request-DTO class names narrow likely operations, but full parsing failed on both ARM64 and x86_64 AOT snapshots. The x86_64 failure was `fill: cluster 1009 (CID 9): obj 1084/5409 ref 0: stream: value too large`; its diagnostic scan had recognized the snapshot. The bundled ARM64 Flutter engine reports Dart `3.11.4`.

**Documented data categories, separate from this APK trace.** Tin Can's [privacy policy](../sources/README.md#s003) says its services collect approved-contact names/numbers and phone configuration such as quiet hours; it also describes phone identifiers/status, call metadata, and voicemail collection. The policy does not identify which request from this Android version sends each category or to which configured host. It is not evidence that the app uploads all phone contacts or voicemail content in a parental-control update.

## How parental controls appear to change

**Documented user workflow.** Tin Can's current support instructions say a parent adds an approved contact in the app's Contacts screen, or reviews another Tin Can's request and explicitly accepts or declines it ([S046](../sources/README.md#s046), [S047](../sources/README.md#s047)). The vendor says Quiet Time can be toggled on the app home screen or scheduled, with account timezone affecting the schedule ([S048](../sources/README.md#s048), [S049](../sources/README.md#s049)). These are vendor statements about intended use, not a protocol trace.

**Static app-side inference.** The contact state manager's enable/disable methods and repository/client names suggest the app submits a change for a selected contact or subscription through a backend client. The availability client, update DTOs, and `setSchedule`/`updateDnd` symbols similarly suggest app-side submissions for calling hours and Quiet Mode. `AvailabilityStateManager` contains DND polling/error strings, so the app also appears to refresh displayed availability. This does **not** show whether the phone polls, receives a push, caches rules, or relies on a server-side call gate. It also does not establish when a revocation takes effect or whether rejected calls are enforced in the handset, telephony service, or both.

**Separate Android permissions.** The Android manifest asks the OS for Contacts, Bluetooth, Location, Camera, and other permissions. The support article about importing phone contacts mentions enabling the phone's Contacts permission ([S046](../sources/README.md#s046)). That OS grant is separate from a parent's approval of who may call the Tin Can.

## Remaining evidence gate

To answer “what does it send” and “how are changes applied” precisely, a later, separately authorized session would need a controlled installed app and account, synthetic contacts, owned Tin Can lines, and correlated app/network/handset observations. Record exact request method, host, redacted body, response, account/object scope, and time from approval or revocation to call behavior. No such observation is claimed here.
