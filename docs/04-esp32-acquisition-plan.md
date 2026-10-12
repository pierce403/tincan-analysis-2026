# ESP32 acquisition and firmware notes

**Future work only. No physical device or flash operation has been examined.** The TCM1/Espressif filing is an ESP32-S3-WROOM-1 lead ([S008](../sources/README.md#s008)); validate the actual unit first.

## Desired outputs

Unit/PCB/module identity; annotated interface map; security-state record; a reproducible full-flash read **if permitted by the actual hardware state**; hashes and acquisition logs; partition/image inventory; or a well-supported explanation of why acquisition was unavailable.

## 1. Identify the unit before selecting a method

Record exterior model/FCC label, purchase batch if known, PCB revision, module marking, flash part/capacity, handset/base arrangement, power supply, and visible connectors/test pads. Assign an inventory ID and keep private identifiers out of published notes. Photograph before changing anything. Flashback/Grandstream ATA procedures are a separate branch, not instructions for this device.

Build a pin map from continuity, markings, and the **identified module datasheet**, not a generic development-board picture. Note UART, ground, supply, EN/reset, boot straps, USB, and JTAG candidates. Identify the microphone/codec and hook-switch path for O5. No schematic or test-pad pinout for the retail unit is established here.

## 2. Prepare a read-first bench session

Use a spare owned unit, ESD precautions, a meter/logic analyzer, and a USB-UART adapter matched to measured logic levels. Do not apply 5 V logic to a presumed 3.3 V interface. Avoid simultaneous programmer/phone power unless the power arrangement has been validated; do not back-power through signal pins. Prefer receive-only UART observation before connecting TX or reset controls. Log voltage and wiring before power-up.

Power/boot itself can change runtime logs, NVS, counters, OTA state, or configuration. Capture initial state and decide whether to isolate Wi-Fi before any application boot. “Read-first” does not mean zero side effects. A factory reset is not a prerequisite and would destroy evidence.

## 3. Establish ROM-loader accessibility and security state

For a confirmed ESP32-S3, Espressif documents GPIO0 low at reset and GPIO46 low/floating for ROM download mode ([S028](../sources/README.md#s028)). Locate those signals on the actual unit; do not short unidentified pads. Download mode can be restricted or disabled. A connection timeout alone does not establish a fuse state.

Prefer a minimal ROM security query without uploading the esptool RAM stub. Inspect output before choosing subsequent commands. Future example, after manually placing a correctly identified unit in ROM mode:

```bash
TC_SERIAL_PORT='/dev/REPLACE_CONFIRMED_PORT'
TC_FW_RAW='evidence/raw/firmware/SESSION_ID'
mkdir -p "$TC_FW_RAW"
esptool --chip esp32s3 --port "$TC_SERIAL_PORT" --baud 115200 \
  --before no-reset --after no-reset --no-stub get-security-info \
  > "$TC_FW_RAW/security-info.txt" 2>&1
```

The example uses the current hyphenated CLI spelling from [esptool source](../sources/README.md#s036). Some prose docs still show `get_security_info`; check the selected installed version's help. Pin the tool version/source before a bench session ([S035](../sources/README.md#s035)).

`--before/--after no-reset` avoids esptool's normal automatic resets; `--no-stub` avoids its RAM helper ([S026](../sources/README.md#s026)). These options do not compensate for wrong wiring or OS/adapter control-line behavior.

If unrestricted ROM access is established, a future read-only fuse summary can record field values/protection status:

```bash
espefuse --port "$TC_SERIAL_PORT" summary --format json \
  --file "$TC_FW_RAW/efuse-summary.json"
```

This summary is a **read operation** ([S029](../sources/README.md#s029)); the tool still interacts with the device. Check its connection/reset behavior for the installed version. Keep DTR/RTS disconnected in a manually controlled UART setup unless a verified reset circuit is intentionally used. Do not treat masked/read-protected fields as zero-valued secrets.

| Property to record | Candidate evidence | Interpretation limit |
| --- | --- | --- |
| Secure boot | `SECURE_BOOT_EN`, digest/key-purpose and revocation fields | Supported feature is not the same as enabled feature; preserve full summary |
| Flash encryption | `SPI_BOOT_CRYPT_CNT`, key purposes/protection, download restrictions | Match the actual S3/SDK documentation; do not use original-ESP32 fuse recipes |
| Download mode | ROM security summary and relevant restrictions | Restricted mode can permit a security query while denying reads |
| Debug paths | JTAG/USB disable fields and identified board interfaces | A fuse clue does not establish every physical/debug path |
| NVS encryption | Image/config/key-partition evidence and storage scheme | Flash encryption and NVS protection require separate analysis |

References: [security overview](../sources/README.md#s030), [flash encryption](../sources/README.md#s031), [secure boot v2](../sources/README.md#s032), [NVS encryption](../sources/README.md#s034).

## 4. Select the acquisition branch

| Observed state | Next planned action | What not to conclude |
| --- | --- | --- |
| Unrestricted loader; reads available | Identify flash size and attempt two full reads | Read access alone does not prove secure boot is off |
| Secure Download Mode | Record permitted security information and read denial; stop ordinary dump route | Do not expect `read-flash`, stub upload, or a full fuse summary to work |
| Download mode inaccessible | Check wiring, power, correct chip, straps, and documented configuration before classifying | A timeout is not proof of a locked chip |
| Raw read succeeds; encrypted regions present | Preserve raw bytes, region map, fuse evidence; seek a legitimate firmware-image acquisition route later | A raw ciphertext copy is not plaintext firmware or a decryption bypass |
| Separate accessible flash IC | Consider electrical isolation/external acquisition only as a later hardware method | External reads do not bypass flash encryption |

Security query and restricted-operation behavior are documented by [S027](../sources/README.md#s027), [S036](../sources/README.md#s036). This plan does not include fault injection, fuse changes, desoldering, or bypass development.

## 5. Future full-flash read example

Only after confirming unrestricted access and the chosen tool's behavior:

```bash
esptool --chip esp32s3 --port "$TC_SERIAL_PORT" --baud 115200 \
  --before no-reset --after no-reset flash-id \
  > "$TC_FW_RAW/flash-id.txt" 2>&1
esptool --chip esp32s3 --port "$TC_SERIAL_PORT" --baud 115200 \
  --before no-reset --after no-reset read-flash 0 ALL "$TC_FW_RAW/flash-a.bin"
esptool --chip esp32s3 --port "$TC_SERIAL_PORT" --baud 115200 \
  --before no-reset --after no-reset read-flash 0 ALL "$TC_FW_RAW/flash-b.bin"
sha256sum "$TC_FW_RAW/flash-a.bin" "$TC_FW_RAW/flash-b.bin"
cmp "$TC_FW_RAW/flash-a.bin" "$TC_FW_RAW/flash-b.bin"
```

`ALL` requests detected flash capacity ([S025](../sources/README.md#s025)). Verify lengths against the identified flash before declaring success. These default read examples may upload/execute the RAM stub; record that intervention. If selecting ROM-only reads with `--no-stub`, verify support and any required explicit flash-size option instead of substituting it blindly. Do not boot the application between reads; explain any differences rather than discarding them.

Keep first originals immutable. Never run flash writes/erases, eFuse burns/protection changes, register writes, or force overrides to “fix” extraction. No such command is part of this acquisition sequence.

## 6. Offline triage of a future image

Work on copies. Determine plaintext/encrypted/mixed regions using multiple clues; entropy alone is insufficient. Discover the partition table from evidence, validating entries/checksum/bounds. ESP-IDF's common `0x8000` partition-table location is a **candidate**, not the Tin Can layout. Decode a validated extracted table with the matching `gen_esp32part.py` version ([S033](../sources/README.md#s033)).

Inventory bootloader, application/OTA slots, NVS/key partitions, filesystems, and manufacturing data when present. For extracted valid app images, `esptool --chip esp32s3 image-info REPLACE_EXTRACTED_APP.bin` is an offline header inspection, not an instruction to run a complete flash dump as an app image.

Map endpoints/trust anchors, auth and device identity, provisioning, call policy, audio state, update verification, and credential storage. Identify SDK/library versions before comparing [Espressif advisories](../sources/README.md#s037); do not apply original-ESP32 vulnerabilities to an S3 without an affected-range and reachability argument.

**Exit:** documented identity, acquisition mode, security-state interpretation, hashes/lengths, limitations, and sanitized observations. Raw binaries and readable credentials never enter Git.
