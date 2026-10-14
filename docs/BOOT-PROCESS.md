# Theia OS Boot Process

This document describes the boot process of Theia OS 0.15, including the responsibilities of the BIOS boot sector, Stage 2, the kernel, and the initialization components.

## 1. Overview

Theia OS uses its own BIOS bootloader and kernel-loading process.

The boot process is responsible for preparing the system, loading the required components into memory, transferring execution to the kernel, and establishing the initial operating environment.

The boot architecture is developed specifically for Theia OS.

## 2. Boot Sequence

The boot process is organized into the following stages:

1. BIOS initialization.
2. Stage 1 execution.
3. Stage 2 execution.
4. Kernel loading and initialization.
5. Init execution.
6. Theia Console execution.

The exact behavior of each stage depends on the implementation included in the corresponding build.

## 3. BIOS Initialization

The BIOS initializes the system sufficiently to begin executing boot code from the selected boot device.

For the floppy disk configuration, the BIOS loads the boot sector into physical address `0x7C00` and transfers control to it.

Theia OS uses a BIOS-based boot process rather than relying on an external bootloader.

## 4. Stage 1

Stage 1 is the first Theia OS boot component executed by the BIOS.

Its responsibilities include:

- Starting execution from the BIOS-loaded boot sector.
- Loading Stage 2 from the boot device.
- Transferring execution to Stage 2.

The Stage 1 implementation uses 16-bit x86 assembly and is assembled with NASM.

### Memory Address

| Component | Address |
|---|---|
| Stage 1 | `0x7C00` |
| Stage 2 | `0x8000` |

Stage 1 must fit within the boot-sector size and preserve the required boot signature.

## 5. Stage 2

Stage 2 continues the boot process after Stage 1.

Its responsibilities include preparing the information required by the kernel, loading the subsequent components, and transferring control to the next stage.

Depending on the build configuration, Stage 2 handles:

- Loading the kernel.
- Loading Init.
- Loading Theia Console.
- Collecting BIOS memory-map information through the E820 interface.
- Preparing the transition to protected mode.

### Memory Map

The following addresses describe the documented memory layout of the development build. They must remain consistent with the actual binary sizes and linker scripts.

| Component | Load address |
|---|---:|
| Stage 1 | `0x7C00` |
| Stage 2 | `0x8000` |
| Kernel | `0x9000` |
| Init | `0x11000` |
| Theia Console | `0x12000` |

The memory map is specific to the current implementation and may change as the operating system evolves.

## 6. Kernel

The kernel is the central operating-system component.

After the bootloader prepares the execution environment and transfers control, the kernel initializes the facilities required by the current build.

The kernel is responsible for foundational operating-system functionality, including the memory-management facilities implemented in the relevant version.

Theia OS is developed using freestanding C and NASM assembly.

## 7. Memory Map Discovery

The boot process uses the BIOS E820 interface to obtain information about the system's physical memory layout.

This information allows the kernel to distinguish between different memory regions instead of assuming that all physical memory is available for general use.

The memory map is stored in a designated memory buffer before it is used by later components.

The precise region types and limits are implementation-specific.

## 8. Init

Init is a component in the current boot sequence.

Its role and degree of responsibility may evolve as Theia OS development progresses. Its presence in the boot sequence does not imply that every planned initialization service is already implemented.

## 9. Theia Console

Theia Console provides the interactive command-line interface available in the current development build.

The console uses the `theia>` prompt and processes supported commands.

Commands documented for the current project include:

- `ECHO`
- `HELP`
- `VERSION`
- `CLS`
- `SYSINFO`
- `MEMINFO`
- `REBOOT`
- `SHUTDOWN`

Individual command behavior depends on the implementation in the tested build. In particular, a shutdown request must not be interpreted as proof of complete hardware power-off.

## 10. Boot Failures

A failure during any boot stage can prevent Theia OS from reaching the interactive console.

When investigating boot failures, verify:

- The boot image and boot-sector signature.
- Stage 1's ability to load Stage 2.
- Component load addresses and binary sizes.
- Kernel entry-point and protected-mode setup.
- The memory map and reserved memory regions.
- The handoff between the kernel and subsequent components.

Diagnostic output should describe the actual failure rather than imply that a later stage has executed successfully.

## 11. Compatibility

This boot process documents the BIOS-based, 32-bit x86 development target of Theia OS 0.15.

UEFI boot support and x86-64 support are not claimed by this document.

## 12. Future Development

Future boot-process changes may include additional platform support, revised initialization responsibilities, and further kernel integration.

These possibilities are part of the development roadmap and should not be treated as implemented features until verified.

---

**Document version:** Theia OS 0.15  
**Scope:** BIOS boot process and component initialization  
**Status:** Technical documentation
