# Theia OS — Compatibility

## 1. Purpose

This document describes the intended platform and compatibility scope of Theia OS 0.15.

It distinguishes the platform Theia OS is designed for from environments that have been tested successfully. Compatibility with a particular system must not be assumed without verification.

## 2. Current Platform Target

Theia OS 0.15 targets the following environment:

| Component | Current target |
|---|---|
| Firmware | Legacy BIOS |
| Architecture | x86, 32-bit |
| Boot method | Theia OS's own BIOS bootloader |
| Distribution format | 1.44 MB floppy disk image |
| Operating system type | Independent operating system |
| Kernel implementation | Freestanding C and NASM assembly |

These specifications describe the current target, not a guarantee that every compatible-looking machine will boot successfully.

## 3. Virtual Machines

Virtual machines provide a controlled environment for testing Theia OS without installing it on physical hardware.

### QEMU

QEMU is used for rapid development and testing. Results depend on the virtual machine's configuration and the version of QEMU in use.

### VirtualBox

VirtualBox is intended to serve as the primary virtualized testing environment. Use a configuration compatible with legacy BIOS, 32-bit x86, and floppy disk images.

Compatibility should be recorded only after a successful test with the specific VirtualBox version and configuration.

### VMware

VMware is a potential future testing environment. Compatibility must be verified independently before it can be described as confirmed.

## 4. Physical Hardware

Booting on physical hardware has not been established as a general compatibility guarantee.

Results may vary depending on firmware behavior, hardware configuration, and available legacy boot support. A successful virtual-machine test does not prove that the same image will work on every physical computer.

## 5. Firmware Compatibility

### Legacy BIOS

Legacy BIOS is the intended boot environment for version 0.15.

### UEFI

Native UEFI boot is outside the currently documented compatibility scope. Support should not be assumed unless it is implemented and tested in a future release.

### Secure Boot

Secure Boot compatibility has not been established for version 0.15.

## 6. Architecture Compatibility

### x86, 32-bit

This is the current target architecture.

### x86-64

Native x86-64 support is not part of the current documented scope. Future support would require implementation and verification.

### Other architectures

ARM, RISC-V, and other architectures are outside the current compatibility scope.

## 7. Image and Boot Limitations

The distributed image is intended for the documented boot environment and format.

The following conditions may prevent a successful boot:

- The image is incomplete or corrupted.
- The virtual machine uses an incompatible firmware mode.
- The virtual machine is configured with an incompatible architecture.
- The image is not attached as a bootable floppy disk.
- The selected boot order prevents the image from being loaded.

A boot failure does not, by itself, identify which condition caused the problem.

## 8. Functional Compatibility

Compatibility also includes the behavior of the operating system after boot.

Version 0.15 includes an interactive text console with commands such as `ECHO`, `HELP`, `VERSION`, `CLS`, `SYSINFO`, `MEMINFO`, `REBOOT`, and `SHUTDOWN`.

The presence of a command does not imply that every related hardware operation is fully implemented. In particular, requesting shutdown should not be interpreted as confirmation that the machine has powered off completely.

## 9. Reporting Compatibility Results

When reporting a compatibility result, include:

- Theia OS version and image source.
- Virtualization software or physical hardware model.
- Firmware mode and architecture configuration.
- Whether the image began booting.
- The last visible boot message.
- Any commands tested and their observed results.

Distinguish between **tested successfully**, **tested unsuccessfully**, and **not yet tested**. Avoid describing an environment as supported solely because it appears compatible on paper.

## 10. Future Compatibility

Compatibility may expand as Theia OS develops. Possible future targets must remain documented as planned until their implementations and tests provide evidence of support.

## 11. Source Code and Development

Theia OS is a closed-source project. This document covers platform compatibility and observable behavior; it does not disclose private source code or internal build procedures.

---

**Document version:** Theia OS 0.15  
**Scope:** Platform compatibility and testing  
**Source code:** Closed-source
