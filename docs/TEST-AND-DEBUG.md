# Theia OS — Installation and Testing Guide

This document explains how to run and test an authorized distribution of Theia OS.

Theia OS is a closed-source operating system. Its source code and internal build process are not part of the public distribution documentation.

## 1. Overview

Theia OS is an independent operating system developed using a custom BIOS bootloader and kernel.

Version 0.15 marks the completion of the Basic Functionality development phase.

This guide focuses on running the distributed disk image in a virtual machine and verifying the behavior available in that build.

## 2. Requirements

The documented development target for Theia OS 0.15 is:

- BIOS-based boot.
- 32-bit x86 architecture.
- A compatible virtual machine.
- An authorized Theia OS disk image.

Theia OS is designed for a specific low-level hardware environment. Successful execution may depend on the virtual machine's firmware, device configuration, and supported hardware interfaces.

## 3. Obtaining the Disk Image

Use a disk image obtained through an official or otherwise authorized distribution channel.

Before running the image:

- Confirm the version.
- Check the file's integrity when a published checksum is available.
- Keep the original download unchanged if you need to investigate a problem.
- Review the release notes for known limitations.

Do not assume that an image is authentic or intact solely because its filename contains the expected version number.

## 4. Running Theia OS in a Virtual Machine

### VirtualBox

VirtualBox can be used to test Theia OS in a virtual machine.

General procedure:

1. Create a virtual machine suitable for the 32-bit x86 target.
2. Configure BIOS firmware where supported.
3. Configure the virtual machine to boot from the authorized Theia OS floppy image.
4. Check the boot order.
5. Start the virtual machine.
6. Observe the boot sequence.
7. Verify whether Theia Console becomes available.

The exact settings may vary with the VirtualBox version and host configuration.

### QEMU

QEMU can be used for rapid boot and functionality tests.

Use a QEMU configuration compatible with the image format and BIOS-based target.

The appropriate launch options depend on the installed QEMU version and the devices configured for the virtual machine.

## 5. Verifying the Boot Process

After starting the virtual machine, check the following:

- The BIOS begins executing the boot process.
- The bootloader progresses through its implemented stages.
- The kernel starts successfully.
- The expected initialization sequence runs.
- Theia Console appears, if supported by the tested build.

A successful virtual-machine launch does not by itself verify every operating-system feature.

## 6. Testing Theia Console

Theia Console uses the following prompt:

`theia>`

Commands documented for Theia OS 0.15 include:

| Command | Purpose |
|---|---|
| `ECHO` | Displays text. |
| `HELP` | Displays command help. |
| `VERSION` | Displays version information. |
| `CLS` | Clears the console screen. |
| `SYSINFO` | Displays system information. |
| `MEMINFO` | Displays memory information. |
| `REBOOT` | Requests a system restart. |
| `SHUTDOWN` | Initiates the implemented shutdown request sequence. |

Test only the behavior actually available in the image you are running.

In particular, a shutdown request should not be interpreted as proof that the virtual machine or physical computer has powered off completely.

## 7. Recording Test Results

When reporting a problem, include the following information where available:

- Theia OS version.
- Source of the disk image.
- Virtualization software and version.
- Relevant virtual-machine configuration.
- Last visible boot message.
- Command being tested.
- Expected behavior.
- Actual behavior.

Screenshots can help document visible behavior and error messages.

Avoid publishing private information that may appear in screenshots or diagnostic output.

## 8. Troubleshooting

### The image does not boot

Verify that the image is intact, that the virtual machine uses compatible BIOS settings, and that the disk image is selected in the boot order.

### The console does not appear

Record the last visible boot message and check whether the issue occurs consistently with the same configuration.

### A command does not behave as expected

Verify the command spelling, the tested version, and the behavior documented for that build.

### The virtual machine restarts or stops unexpectedly

Record the sequence of events and the virtualization environment. Do not assume that the cause is a particular kernel component without further evidence.

## 9. Closed-Source Development

Theia OS is distributed as closed-source software.

The public documentation describes its supported usage, visible behavior, compatibility, and known limitations. It does not provide the private source code or internal build instructions.

Internal compilation and development procedures remain separate from this public guide.

## 10. Scope and Limitations

This guide documents the BIOS-based, 32-bit x86 target of Theia OS 0.15.

Compatibility with other firmware, architectures, virtual machines, or physical hardware must be verified independently.

Future capabilities must not be treated as supported until they have been implemented and tested.

---

**Document version:** Theia OS 0.15  
**Scope:** Authorized distribution, execution, and testing  
**Source code:** Closed-source
