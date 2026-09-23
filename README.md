# ASUS ROG Strix G815LP BIOS Repair Case Study

Technical documentation of a successful repair of an ASUS ROG Strix G815LP-IS96 that became stuck at the ROG logo immediately after Secure Boot was enabled. Normal Windows startup and normal BIOS access with F2 were affected.

**The successful repair was programming a complete, machine-specific, reconstructed 32 MiB SPI image into U300.** A prior repair limited to NVRAM did not resolve the fault. U2750 did not require modification.

This repository documents one real repair, based on the supplied repair record. It contains documentation only: no original dumps, rebuilt images, or other firmware binaries. The recorded measurements and checksums have not been independently reproduced as part of preparing this documentation.

## Hardware identification

| Item | Identification |
| --- | --- |
| Laptop | ASUS ROG Strix G815LP-IS96 |
| Model / series | G815LP |
| Motherboard | G615L-PMV-GBM |
| Board P/N | 6050A3657304-MB-A01 |
| Observed PCB date | 2025-02-25 |
| Main BIOS SPI, U300 | Winbond W25Q256JW; 256 Mbit / 32 MiB; 1.8 V; JEDEC EF 60 19 |
| Secondary SPI, U2750 | Winbond W25Q80EW; 8 Mbit / 1 MiB; 1.8 V; JEDEC EF 60 14 |

## Original symptoms

The laptop had been working correctly. The user enabled Secure Boot because Fortnite indicated that it was required. Immediately afterward, the laptop froze at the ROG logo, stopped starting Windows correctly, and could no longer be accessed normally through F2 BIOS entry.

This sequence establishes the observed trigger in this case. It does not establish the exact underlying firmware defect or prove that every similar symptom has the same cause.

## Repair summary

1. U300 was desoldered for external reading and programming using a CH341A with a 1.8 V adapter and IMSProg.
2. Two independent original reads, each 33,554,432 bytes, matched byte for byte with zero differences. Both had the same SHA-256.
3. Two approximately 192 KiB NVRAM regions were located at offsets `0x10B0000` and `0x10E0000`. They contained AMI variables related to Secure Boot, including `SecureBootSetup`, `DeploymentModeNv`, `VendorKeysNv`, and `dbx`.
4. An initial NVRAM repair using components from official ASUS BIOS 338 was programmed and verified successfully, but the ROG-logo freeze remained.
5. A complete 32 MiB SPI image was then reconstructed using the valid original dump and corresponding components from official ASUS BIOS 338, preserving machine-specific regions from the original firmware.
6. U300 was processed through **Erase → Blank Check → Program → Verify**. A subsequent full-chip read had the same SHA-256 as the reconstructed image, and the repair record reports byte-for-byte verification.
7. After U300 was reinstalled, the laptop started correctly. The reported fault was resolved without modifying U2750.

## The image that resolved this case

| Property | Recorded value |
| --- | --- |
| Filename | `G815LP_338_FULL_SPI_REBUILT.bin` |
| Size | 33,554,432 bytes / 32 MiB |
| SHA-256 | `a7e8c799510b69e963fb9802815c08aceb2f09e596f31f74d764603231a6757e` |
| Full-chip readback SHA-256 | Same as the reconstructed image |
| Outcome | Normal startup restored after reinstalling U300 |

**This filename does not identify a universal firmware image for G815LP laptops.** The image was reconstructed from one specific machine's original dump and may contain or depend on unit-specific regions. Neither that image nor the original dumps are included or requested for publication.

## Official ASUS package is not a raw SPI image

The official ASUS BIOS 338 EZ Flash file used in this case was `G815LPAS.338`, measuring **38,928,384 bytes**. The repair record identifies it as an **AMI PFAT / Intel BIOS Guard package**, not a raw 32 MiB SPI image.

**Do not program `G815LPAS.338` directly into the W25Q256JW with a CH341A.** Its package format must not be confused with the complete raw image used for external SPI programming. No third-party firmware source is used or linked here.

## Scope and limitations

The exact reconstruction recipe, component map, and boundaries of the preserved machine-specific regions were not supplied. This documentation therefore describes the recorded process and outcome; it is not a reproducible image-building recipe. The unsuccessful NVRAM-only attempt does not, by itself, identify which additional component or state required repair.

The first boot can take longer than normal because of firmware/hardware initialization. No measured first-boot duration, exhaustive post-repair test results, or post-repair Secure Boot state was supplied.

## Documentation

- [Repair notes](docs/repair-notes.md): diagnostic sequence, programming, verification, and limits of the evidence.
- [Hardware](docs/hardware.md): board and SPI device identification.
- [Checksums](docs/checksums.md): all recorded SHA-256 values and their outcomes.
- [Disclaimer](DISCLAIMER.md): risks and intended technical audience.

The included `.gitignore` helps prevent accidental firmware uploads. Ignore rules are a safeguard, not a substitute for reviewing the files staged for publication; they do not remove files already tracked by Git.

## Professional BIOS Repair Service

I also provide paid remote BIOS/SPI firmware analysis and reconstruction services for laptop repair technicians.

Services may include:

- Verification and comparison of SPI dumps
- NVRAM / Secure Boot related repair
- Machine-specific BIOS reconstruction
- Analysis of corrupted or incomplete firmware
- Remote assistance for SPI programming and firmware recovery
- Complex firmware cases involving AMI PFAT / Intel BIOS Guard structures

### How it works

The technician sends at least two verified dumps read from the original SPI flash.

The dumps are compared and analyzed before any reconstruction work is performed.

Machine-specific data should always be preserved whenever possible.

### Important

This is a professional repair service, not a guarantee that every firmware case can be recovered.

Never send customer passwords, personal documents or unnecessary private information.

For pricing and availability, contact me through GitHub.

## Support this work

If this documentation helped you, you can support future repair documentation through GitHub Sponsors.
