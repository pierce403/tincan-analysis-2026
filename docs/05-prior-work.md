# Prior work and useful leads

Checked **2026-10-12**. Searches were bounded and web-index based; see [the search log](../sources/search-log.md). None of the leads below establishes a vulnerability in the current ESP32 product.

## Directly relevant discovery

| Lead | What was found | How to use it | Limit |
| --- | --- | --- | --- |
| TCM1 regulatory exhibits | An Espressif change-of-ID authorization letter and a Tin Can module filing | Identify likely silicon, then compare with actual unit labels | Regulatory module evidence, not a retail-board teardown; [S008](../sources/README.md#s008), [S038](../sources/README.md#s038) |
| Flashback ATA repurposing report | A first-person Reddit post reports obtaining a Tin Can-branded Grandstream HT801 and repurposing it | Historical lead for the separately identified Flashback product | Unverified third-party report; not an ESP32 dump or proof of current cloud behavior; [S016](../sources/README.md#s016) |
| Hacker News product discussion | Discussion of the product, privacy, DIY phones, and provider dependence | Discover questions and links; revisit current policies | Commentary, not a firmware or API audit; [S015](../sources/README.md#s015) |

The Flashback author describes configuration/auto-provisioning behavior, but this project has not reproduced it. Do not copy its reset/credential instructions into the current handset workflow. Vendor FAQ documentation distinguishes Flashback from the newer phone ([S006](../sources/README.md#s006)).

## Independent projects and name collisions

| Project | Relationship | Research value |
| --- | --- | --- |
| [Open Lounge Phone](https://openloungephone.app/project/about/) | Explicitly independent, Tin Can-inspired project; AGPL software/CERN-OHL hardware; physical hardware still in development at the checked date | Comparative architecture/design reference, not Tin Can source or compatible replacement firmware; [S013](../sources/README.md#s013) |
| [GlomarGadaffi/tincan](https://github.com/GlomarGadaffi/tincan) | ESP32-S3 push-to-talk SIP proof of concept for another project | Generic ESP32 voice implementation reference, not a commercial Tin Can dump; [S014](../sources/README.md#s014) |
| Other `tincan` results | Linux/iPhone companions, e-learning Tin Can API/xAPI, string phones, and unrelated DIY intercoms | Exclude from claims about this product unless a direct connection is established |

## What was not located

- An official licensed source release for the commercial firmware or parental app.
- A public firmware dump with a Tin Can model/revision, acquisition provenance, and artifact hash.
- A detailed current-product APK/ESP32 reverse-engineering report.
- A verified current-product vulnerability report supported by a reproducible artifact.

These are P0 search outcomes, not statements that such material cannot exist. At the time of that search, no exhaustive GitHub code crawl, vulnerability-database coverage audit, APK inspection, or vendor inquiry was performed. A later [offline APK review](09-apk-static-analysis.md) is recorded separately. Re-run the dated queries before future analysis and archive only concise bibliographic metadata/sanitized notes.

## Prior-work intake criteria

For each new lead record: product/model applicability; author; date; URL/commit; artifact version/hash where provided; method; reproduced versus asserted results; source license; and remaining uncertainty. Compare the work with this project's actual unit and version before adopting any technique or claim.
