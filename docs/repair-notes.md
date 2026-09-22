# Repair Notes

## Evidence and scope

These notes document the supplied repair record for an ASUS ROG Strix G815LP-IS96, motherboard G615L-PMV-GBM, board P/N 6050A3657304-MB-A01. They distinguish recorded observations from conclusions that cannot be established from this case alone. No firmware files were provided for independent analysis during documentation preparation.

## 1. Failure history

The laptop worked correctly before Secure Boot was enabled in response to a Fortnite requirement. Immediately afterward, it froze at the ROG logo and no longer started Windows correctly. Normal access to BIOS using F2 was also unavailable.

External SPI diagnosis and programming were selected. The timing associates the symptom with the Secure Boot change, but the record does not establish the exact root cause.

## 2. Establishing a consistent original U300 read

U300, a 1.8 V Winbond W25Q256JW, was desoldered. A CH341A with a 1.8 V adapter was used with IMSProg for reading and programming.

Two independent reads were saved as `G815LP_ORIGINAL_1.bin` and `G815LP_ORIGINAL_2.bin`. Each measured 33,554,432 bytes. Byte-by-byte comparison found zero differences, and both produced this SHA-256:

```text
2c9062249650f8980a33795541c912f6e6ce7fe37445fda7d34a5dd03d80e980
```

The agreement supports read consistency. It does not establish that the original firmware's logical state was healthy: these were reads of the machine in its failed condition.

The original dumps remain private because they may contain client-machine-specific information. Publishing them is outside the scope of this repository.

## 3. NVRAM investigation and unsuccessful limited repair

Two NVRAM regions of approximately 192 KiB were located at offsets `0x10B0000` and `0x10E0000`. Their contents included the AMI Secure Boot-related variable names:

- `SecureBootSetup`
- `DeploymentModeNv`
- `VendorKeysNv`
- `dbx`

The record does not supply the variable values or establish that any particular variable was corrupt.

A limited NVRAM repair used components from official ASUS BIOS 338 and produced `G815LP_338_CLEAN_NVRAM.bin`, with SHA-256:

```text
e9d61bf43c99e6553b2d1a8ddeb11309b7a7139f3e7d6aa57e8d64ce4f12b1e7
```

Programming and verification completed correctly, but the laptop still froze at the ROG logo. This separates successful chip programming from successful functional repair. It shows that this particular NVRAM-limited attempt was insufficient, not that NVRAM can be ruled out of the failure mechanism.

## 4. Official BIOS input and format distinction

The ASUS BIOS version used was 338 for G815LP. The extracted EZ Flash file, `G815LPAS.338`, measured 38,928,384 bytes and was identified in the repair record as an AMI PFAT / Intel BIOS Guard package.

It is not a raw 32 MiB SPI image and must not be written directly to U300 with a CH341A. The package and the reconstructed image are different artifacts with different roles. The supplied record does not specify extraction tools, reconstruction commands, or the package checksum.

## 5. Complete SPI reconstruction

The successful image combined:

- The valid, consistently read original dump from this laptop.
- Corresponding components from official ASUS BIOS 338.
- Preservation of machine-specific regions present in the original firmware.

The resulting `G815LP_338_FULL_SPI_REBUILT.bin` was 33,554,432 bytes, with SHA-256:

```text
a7e8c799510b69e963fb9802815c08aceb2f09e596f31f74d764603231a6757e
```

The exact replaced regions, preserved-region boundaries, and reconstruction method are not documented in the supplied record. No offsets, commands, or reconstruction script are inferred here. A technician cannot reproduce the complete reconstruction from these notes alone.

This image was specific to the repaired unit. It may contain or depend on machine-specific data and is not presented as a universal replacement for other G815LP machines.

## 6. U300 programming and verification

The recorded programming sequence was:

1. **Erase** the SPI device.
2. **Blank Check** the erased device.
3. **Program** the reconstructed image.
4. **Verify** the programmed contents.
5. Read the entire chip again and compare the readback with the intended image.

The full readback had SHA-256 `a7e8c799510b69e963fb9802815c08aceb2f09e596f31f74d764603231a6757e`, matching the reconstructed image. The repair record reports that the write was verified byte for byte. A separate readback filename was not supplied.

These checks support that the intended image was written correctly; functional success was established separately by the subsequent startup.

## 7. Functional outcome

After U300 was reinstalled with the reconstructed image, the laptop started correctly. The ROG-logo freeze observed after enabling Secure Boot was resolved.

The first boot can take longer than normal because of firmware/hardware initialization; no exact wait time is recorded. No claim is made about the resulting Secure Boot setting, Fortnite testing, a specific Windows version, or long-term validation because those details were not supplied.

## 8. Secondary device U2750

U2750 was identified as a Winbond W25Q80EW, 8 Mbit / 1 MiB, 1.8 V, JEDEC ID `EF 60 14`. Two identical reads were obtained, with SHA-256:

```text
166cb444abb468fc07132c7e6da8c5cdbfa85841f93ef2c54c3c6c237bedb524
```

Its contents included `ASUS-ROG-NB`, `G615LM`, `EE_USB_RETIMER`, `EE_PCIE_DN0`, `CIO_PHY_SECTION`, `DROM`, and `ARC PARM`. These strings are recorded observations, not a verified assignment of the chip's complete function.

**U2750 did not need to be modified to solve this fault.** Its read filenames were not supplied, and its contents are not included in this repository.

See [hardware identification](hardware.md), [checksums](checksums.md), and the [risk disclaimer](../DISCLAIMER.md).
