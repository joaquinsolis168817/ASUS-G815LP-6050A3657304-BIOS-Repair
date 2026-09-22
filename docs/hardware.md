# Hardware Identification

The following identifiers and observations come from the supplied repair record.

## System and board

| Field | Value |
| --- | --- |
| Laptop | ASUS ROG Strix G815LP-IS96 |
| Model / series | G815LP |
| Motherboard | G615L-PMV-GBM |
| Board P/N | 6050A3657304-MB-A01 |
| Observed PCB date | 2025-02-25 |

## SPI devices

| Property | U300 | U2750 |
| --- | --- | --- |
| Manufacturer | Winbond | Winbond |
| Part | W25Q256JW | W25Q80EW |
| Capacity | 256 Mbit / 32 MiB | 8 Mbit / 1 MiB |
| Operating voltage recorded | 1.8 V | 1.8 V |
| JEDEC ID | `EF 60 19` | `EF 60 14` |
| Role in this repair | Main BIOS SPI; reconstructed image programmed | Secondary SPI; no modification needed |
| Original reads | Two independent, identical reads | Two identical reads |

Capacity units differ: Mbit denotes megabits, whereas MiB denotes mebibytes. The recorded U300 image size was 33,554,432 bytes.

## U300 handling and equipment

U300 was desoldered for reading and programming. The recorded equipment was a **CH341A with a 1.8 V adapter**, operated through **IMSProg**.

The successful image was programmed using Erase, Blank Check, Program, and Verify, followed by a full-chip readback. U300 was then reinstalled and the laptop started correctly.

No package suffix, pinout diagram, board-location photograph, programmer revision, or IMSProg version was supplied. This page must not be used as a wiring diagram; verify the actual device and hardware configuration before working on a board.

## U2750 content observations

The secondary device contained these references:

```text
ASUS-ROG-NB
G615LM
EE_USB_RETIMER
EE_PCIE_DN0
CIO_PHY_SECTION
DROM
ARC PARM
```

The strings are retained exactly as recorded. In particular, `G615LM` is an observed firmware string, not a replacement for the motherboard identification above. The record does not establish the device's complete functional role or state whether it was desoldered for reading.

U2750 did not require modification. Refer to [checksums](checksums.md) for its recorded read hash and to [repair notes](repair-notes.md) for the sequence of work.
