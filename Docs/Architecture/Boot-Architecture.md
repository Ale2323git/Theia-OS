# Boot Architecture

Theia OS uses a native BIOS boot path. It does not use GRUB, UEFI, or another
operating system's loader. This document describes the public boot architecture
without publishing private implementation details.

## Platform and distribution

- BIOS-class x86 system;
- complete bootable `.img` disk image;
- native Theia boot path;
- Ring 0 kernel, Ring 3 Init, and Ring 3 Mashel.

Images are distributed through
[GitHub Releases](https://github.com/Ale2323git/Theia-OS/releases). The
published 0.18 image was validated in the reference virtual environment.
Physical-hardware coverage remains limited.

## Boot phases

| Phase | Purpose | Result |
| --- | --- | --- |
| Firmware | Starts the boot device | Native Theia boot path running |
| Native boot path | Prepares the system and loads components | Kernel and system components available |
| Kernel initialization | Establishes Ring 0 services | Boot coordinator available |
| Boot coordinator | Mandatory handoff role documented as `initland` | Init prepared in Ring 3 |
| Init | Prepares the user environment | Mashel started in Ring 3 |
| Mashel | Presents the interactive environment | `theia>` prompt |

## Firmware and native boot

The native boot path prepares the execution environment, obtains basic memory
information from firmware, and loads the kernel and system components. Invalid
or missing components stop the boot with a diagnostic.

## Kernel initialization

Kernel initialization brings up, in dependency order:

1. the execution environment;
2. diagnostic reporting;
3. interrupts, exceptions, and timing;
4. memory management;
5. privilege and task support;
6. resources, communication, and synchronization;
7. platform services;
8. system state available to the user environment.

A fatal initializer failure produces a diagnostic and stops the system safely.

## Boot coordination and `initland`

Theia OS requires a mandatory coordinator for the kernel-to-userland handoff.
The public architecture names this role `initland`. The coordinator is
established by kernel initialization and is not a userland component.

The coordinator prepares the handoff, creates Init as a Ring 3 component, and
remains responsible for the privileged side of the startup path. An unexpected
failure of this phase is fatal.

## Init in Ring 3

Init is the first Ring 3 component. It confirms the handoff, prepares the user
environment, creates Mashel through the public system interfaces, and reports
readiness. The kernel never skips Init to start Mashel directly.

## Mashel in Ring 3

Mashel is the official console component. It presents the Theia OS Mashel banner
and the `theia>` prompt. It interacts through public system services. It does
not access hardware directly; privileged work is handled through defined
system-provided channels.

## Failure behavior

| Failure | Result |
| --- | --- |
| Firmware or boot path cannot start | Diagnostic and halt |
| Required system component cannot be loaded | Diagnostic and halt |
| Fatal kernel initializer fails | Diagnostic and safe halt |
| Non-fatal platform service unavailable | Warning where safe to continue |

## Validation

- A complete bootable image is produced by the internal build process.
- The published 0.18 image was validated in the reference virtual environment.
- Validation confirmed the Mashel banner and `theia>` prompt on text output.
- Physical-hardware validation is limited and not broadly claimed.

## Related documentation

- [Architecture Overview](Overview.md)
- [Kernel Architecture](Kernel-Architecture.md)
- [System Architecture](System-Architecture.md)
- [Running Theia OS](../Installation/Running-Theia-OS.md)
- [Development Status](../Development/Development-Status.md)

---

Theia OS is proprietary software. This document describes public boot
architecture only and does not disclose private implementation details.
