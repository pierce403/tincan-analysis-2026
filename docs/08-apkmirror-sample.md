# P1 APKMirror research sample: Tin Can 3.1.2 (433)

**Status:** acquisition and offline identity verification complete for this third-party sample on 2026-10-13. The owner authorized moving to P1 and downloading the APKMirror bundle as a research sample. No installation, account use, decompilation, endpoint request, or device interaction occurred during acquisition. A subsequent separately authorized [offline static review](09-apk-static-analysis.md) is recorded separately. This sample is not the official Google Play installation baseline planned in [Stage A](03-apk-teardown-plan.md#stage-a-acquisition-and-identity).

## Source and preserved artifact

| Field | Observation |
| --- | --- |
| Source | [S039](../sources/README.md#s039), APKMirror Tin Can 3.1.2 (433) listing, checked 2026-10-13 |
| Acquisition route | Browser download from the listing's APK bundle control; acquired 2026-10-13T11:20:22Z |
| Original filename | `app.tincan_3.1.2-433_3arch_7dpi_24lang_2ffcee7bb50e0cb48d338f370e67f215_apkmirror.com.apkm` |
| Preserved copy | `evidence/raw/apk/2026-10-13-apkmirror-3.1.2-433/` (directory mode 0700, bundle mode 0400, excluded from Git) |
| Byte length | 75,295,463 |
| SHA-256 | `55d70b144d4745b365a37f6884ae86de62c879e50f22a4100a71185206233cb3` |
| Bundle format | ZIP/APKM archive with 41 entries, including `base.apk` and 34 `split_config.*.apk` entries |

The downloaded byte length and SHA-256 match APKMirror's published values. The original browser download remains in `~/Downloads`; the ignored evidence copy has the same hash. The bundle and extracted APK working copies are **not in Git**. Ignoring a directory is not a backup or encryption; no independent backup has been verified.

## Offline identity checks

Python 3.14.4 `zipfile` extracted only the 35 APK entries into ignored `evidence/work/apk/2026-10-13-apkmirror-3.1.2-433/splits/`. All 35 passed `apksigner verify --verbose --print-certs` (apksigner 0.9) and `aapt dump badging` (Android Asset Packaging Tool v0.2-11948202). The base manifest reports `app.tincan`, version name `3.1.2`, version code `433`, minimum API 24, and target API 36. Each split reports package `app.tincan`, version code `433`, an empty split manifest version name, and the expected split name. All 35 APKs report one signing certificate with SHA-256 fingerprint `05f2274d345cd61b1dddc489371d3584ee073b1f2b7fb7def647d4a162ff0be8`, matching the fingerprint shown by APKMirror.

The archive contains three ABI splits (`arm64-v8a`, `armeabi-v7a`, `x86_64`), seven density splits, and 24 language splits. [The sanitized artifact manifest](../sources/apkmirror-3.1.2-433-manifest.csv) records the bundle and every APK entry's filename, byte length, SHA-256, parent artifact, package identity, and signer. Detailed verifier output is retained only in ignored `evidence/work/apk/2026-10-13-apkmirror-3.1.2-433/verification.txt`.

## Procedure and limits

| Order | Action | Result |
| --- | --- | --- |
| 1 | Opened [S039](../sources/README.md#s039) in the browser and selected its 3.1.2 bundle download | Completed; browser file timestamp 2026-10-13T11:20:22Z |
| 2 | `stat`, `sha256sum`, and `file` on the download; copied it into `evidence/raw/` with `install -m 0400`; checked the copy with `sha256sum` and `git check-ignore -v` | Size/hash match listing and copy; `/evidence/raw/` ignore rule applies |
| 3 | Listed archive entries and extracted only `.apk` entries using Python `zipfile` into `evidence/work/` | 35 APKs extracted; no unsafe archive paths observed |
| 4 | Ran `apksigner verify --verbose --print-certs` and `aapt dump badging` for each extracted APK | 35/35 exited 0 and matched expected package, version code, split identity, and certificate |

These checks establish internal file identity and signature validity for the downloaded mirror bundle. They do not establish that APKMirror received it from Tin Can or Google Play, that the bundle matches a current device installation, or that the app implements any security property. The bundle's 34 splits are APKMirror archive entries, not an observed device-specific installed split set. No vulnerability claim follows from this acquisition.

The official baseline remains open: identify a controlled Android device/profile, install from the vendor-linked Play listing, pull every installed split, and compare package/version/signing identity with this mirror sample. A prior `adb devices -l` attempt in the restricted environment could not start its local server (`Operation not permitted`); it did not establish whether an Android device was connected.
