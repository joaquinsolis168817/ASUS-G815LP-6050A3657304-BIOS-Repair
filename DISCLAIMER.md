# Disclaimer

This repository documents a single repair case for technical reference. It is intended for experienced technicians familiar with board-level repair, SPI flash programming, voltage compatibility, firmware formats, and verification procedures.

## Electrical and physical risks

External programming and chip removal or reinstallation can damage the SPI device or motherboard, including pads and traces. Incorrect voltage, orientation, wiring, or handling can cause permanent damage. Both U300 and U2750 in this record are identified as **1.8 V** devices. The recorded U300 setup used a CH341A with a 1.8 V adapter; the programmer name alone does not establish voltage compatibility.

Verify the actual device, supply and signal compatibility, orientation, connections, and equipment before any operation. This documentation does not provide a wiring diagram or a complete desoldering, soldering, or electrical-isolation procedure.

## Firmware and data risks

Erasing or programming SPI memory can leave a system unable to boot and can overwrite machine-specific information. Keep original reads privately and verify their consistency before modifying firmware. Original dumps and derived images may contain information specific to a client's machine and should not be published as part of this case study.

The official `G815LPAS.338` file described here is an **AMI PFAT / Intel BIOS Guard package**, not a raw 32 MiB SPI image. **Do not write that package directly to the W25Q256JW using a CH341A.**

`G815LP_338_FULL_SPI_REBUILT.bin` was reconstructed for one specific unit using its original dump and components from official ASUS BIOS 338. It may contain or depend on machine-specific regions. Its successful use in this case does not establish compatibility with another G815LP laptop, even if its model or board identifiers match.

## Limits of the documentation

The notes reproduce the supplied repair record. They do not provide a complete reconstruction recipe, independently verified firmware analysis, or proof of the exact underlying fault. A matching checksum confirms content identity, not functional compatibility. Successful programming verification does not by itself guarantee a working system.

The outcome is limited to the reported restoration of startup in this case. No guarantee is made that following a similar approach will resolve another machine's fault. Anyone undertaking repair must independently assess the hardware, firmware, and risks and is responsible for their own work. The documentation is provided without a warranty of suitability or outcome.

No original dumps or firmware binaries are included. The repository's ignore rules reduce accidental inclusion but do not protect files already tracked by Git or prevent an explicit forced addition. Review staged content before publication.
