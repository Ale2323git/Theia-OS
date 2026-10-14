# Theia OS Architecture

This document describes the architecture of Theia OS 0.15, including its main components, their responsibilities, and their relationships.

## 1. Overview

Theia OS is an independent operating system developed from the ground up.

Its current implementation targets 32-bit x86 systems and uses a custom BIOS bootloader, a kernel developed in freestanding C and NASM assembly, and an interactive command-line interface.

The architecture is designed to evolve incrementally. Features described as future development are not necessarily present in the current implementation.

## 2. Architectural Principles

Theia OS development follows three priorities:

1. Core functionality.
2. Security.
3. Visual interface.

The project maintains its own operating-system architecture rather than depending on an existing general-purpose operating system kernel.

The source code is closed-source, while public documentation describes the system's design and behavior.

## 3. Main Components

The current boot and execution structure includes the following components:

| Component | Responsibility |
|---|---|
| Stage 1 | Starts the custom boot process and loads Stage 2. |
| Stage 2 | Continues boot preparation and loads subsequent components. |
| Kernel | Provides the central execution environment and implemented kernel facilities. |
| Init | Participates in the current initialization sequence. |
| Theia Console | Provides the interactive command-line interface. |

These components are part of the current development architecture. Their responsibilities may change as the system evolves.

## 4. Boot Architecture

Theia OS uses a custom BIOS bootloader.

The boot process begins when the BIOS transfers control to Stage 1. Stage 1 loads Stage 2, which prepares and loads the subsequent components.

The kernel then receives control and establishes the functionality available in the current build.

For additional details, see [Boot Process](BOOT-PROCESS.md).

## 5. Kernel

The kernel is the central operating-system component.

The current implementation is developed using:

- Freestanding C.
- NASM assembly.
- A 32-bit x86 target.
- A custom boot and kernel-loading process.

The kernel provides the foundation for implemented low-level facilities. Its exact responsibilities depend on the functionality available in the corresponding version.

## 6. Memory Management

Memory management is developed incrementally.

The documented milestones include:

- **0.8 — Memory Foundation**
- **0.9 — Memory Region Manager**
- **0.10 and 0.11 — Memory Allocator Foundation work**

The boot process obtains physical memory information through the BIOS E820 interface. This information provides a basis for identifying available and reserved memory regions.

The presence of a milestone in the development history does not imply that every possible memory-management feature is implemented.

## 7. Initialization and Console

### Init

Init participates in the current boot sequence. Its role may evolve as initialization responsibilities are refined and integrated into the architecture.

### Theia Console

Theia Console provides the current interactive command-line interface.

The prompt is:

`theia>`

Documented commands include `ECHO`, `HELP`, `VERSION`, `CLS`, `SYSINFO`, `MEMINFO`, `REBOOT`, and `SHUTDOWN`.

The behavior of each command must be evaluated against the current build. For example, accepting a shutdown command does not by itself establish that the machine has powered off.

## 8. Memory Layout

The current development configuration uses designated physical memory addresses for boot and system components.

| Component | Documented address |
|---|---:|
| Stage 1 | `0x7C00` |
| Stage 2 | `0x8000` |
| Kernel | `0x9000` |
| Init | `0x11000` |
| Theia Console | `0x12000` |

These addresses describe the documented build configuration, not a permanent architectural requirement.

Reserved buffers and component sizes must be considered when evaluating memory usage and preventing overlaps.

## 9. Development and Testing

Theia OS is developed as a standalone operating system rather than as an application that runs inside another operating system's process environment.

Virtual machines provide environments for testing the boot process and kernel behavior.

QEMU is suitable for rapid development tests, while VirtualBox can be used for additional virtual-machine testing.

Testing results should be recorded against the specific build and configuration used.

## 10. Current Scope and Limitations

This document describes the BIOS-based, 32-bit x86 development target of Theia OS 0.15.

It does not claim that the following capabilities are already implemented:

- UEFI boot support.
- x86-64 support.
- A complete graphical user interface.
- Every planned kernel subsystem.
- Complete hardware power-off behavior.

Such features should be documented as implemented only after they have been developed and verified.

## 11. Future Architecture

Theia OS is intended to evolve beyond its initial foundational stage.

Future architectural proposals may introduce additional kernel facilities, revised component responsibilities, and new platform capabilities.

These plans remain distinct from the implemented architecture of version 0.15.

---

**Document version:** Theia OS 0.15  
**Scope:** System architecture  
**Status:** Technical documentation
