# Recorded Checksums

All values below are SHA-256 checksums from the supplied repair record. They are documented for traceability and have not been recalculated here because the firmware files are not part of this documentation. A checksum identifies content; it does not establish that an image is safe or suitable for another machine.

## Original U300 reads — private

| File | Recorded size | SHA-256 |
| --- | --- | --- |
| `G815LP_ORIGINAL_1.bin` | 33,554,432 bytes | `2c9062249650f8980a33795541c912f6e6ce7fe37445fda7d34a5dd03d80e980` |
| `G815LP_ORIGINAL_2.bin` | 33,554,432 bytes | `2c9062249650f8980a33795541c912f6e6ce7fe37445fda7d34a5dd03d80e980` |

The two independent reads matched byte for byte with zero differences. These dumps capture the original machine state, may contain client-specific data, and must remain private.

## NVRAM-limited repair — did not resolve the fault

| File | SHA-256 |
| --- | --- |
| `G815LP_338_CLEAN_NVRAM.bin` | `e9d61bf43c99e6553b2d1a8ddeb11309b7a7139f3e7d6aa57e8d64ce4f12b1e7` |

Programming and verification succeeded, but the ROG-logo freeze remained. An exact file size was not separately supplied for this artifact.

## Complete reconstructed SPI image — successful repair

| Artifact | Recorded size | SHA-256 |
| --- | --- | --- |
| `G815LP_338_FULL_SPI_REBUILT.bin` | 33,554,432 bytes | `a7e8c799510b69e963fb9802815c08aceb2f09e596f31f74d764603231a6757e` |
| Subsequent full U300 readback; filename not supplied | Full-chip read | `a7e8c799510b69e963fb9802815c08aceb2f09e596f31f74d764603231a6757e` |

**`G815LP_338_FULL_SPI_REBUILT.bin` was the image that resolved the reported fault.** The readback matched its SHA-256, and the repair record reports byte-for-byte write verification. After U300 was reinstalled, the laptop started correctly.

The reconstructed image was based on this specific machine's original dump while preserving machine-specific regions. It may contain or depend on unit-specific data. It is not a universal G815LP firmware image and is not distributed here.

## Secondary U2750 reads — no modification required

| Artifact | Device capacity | SHA-256 |
| --- | --- | --- |
| Two identical U2750 reads; filenames not supplied | 8 Mbit / 1 MiB | `166cb444abb468fc07132c7e6da8c5cdbfa85841f93ef2c54c3c6c237bedb524` |

U2750 did not require modification to resolve the fault.

## Official ASUS source package

`G815LPAS.338` measured **38,928,384 bytes**. No SHA-256 was supplied for this file, so none is asserted here. The record identifies it as an AMI PFAT / Intel BIOS Guard package, not a raw 32 MiB SPI image. It must not be programmed directly into U300 using a CH341A.
