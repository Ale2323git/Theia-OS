# Boot Architecture

Theia OS uses a native boot path designed for its own kernel and system images.
It does not use GRUB, UEFI, or another operating system's loader in the
documented 0.18 line.

This document explains the public boot architecture: the platform assumptions,
the stages of the boot process, the handoff to the kernel, and the transition to
the interactive console. It does not disclose private memory addresses, disk
layouts, or implementation details.

## Platform and distribution

The documented Theia OS platform is:

- a BIOS-class x86 system;
- a complete bootable `.img` disk image;
- a native two-stage Theia boot path;
- a kernel followed by separate Init and Console system images.

The disk image is distributed through
[GitHub Releases](https://github.com/Ale2323git/Theia-OS/releases). The
repository does not distribute source code or buildable source packages.

The primary validation environment is QEMU. Earlier development sessions
recorded successful BIOS/QEMU boots, but the latest documentation pass did not
reproduce a boot. Physical-hardware coverage is limited and is not claimed
broadly.

## Boot stages at a glance

| Stage | Purpose | Output |
| --- | --- | --- |
| Firmware | Initializes the platform and starts the boot device | Control transferred to the Theia boot path |
| First-stage loader | Starts the second stage from the boot image | Second-stage loader running |
| Second-stage loader | Discovers memory and loads the kernel and system images | Kernel and images placed in memory |
| Kernel entry | Establishes protected-mode execution and paging | Kernel initialization running |
| Kernel initialization | Brings up interrupts, timing, memory, scheduling, objects, IPC, synchronization, diagnostics, and platform services | System ready for Init |
| Init | Performs the initial system-image handoff | Control transferred to Console |
| Console | Presents the interactive environment | User-visible console |

## Firmware and the first-stage loader

The platform firmware starts the boot process from the Theia disk image. The
first-stage loader is the initial Theia-controlled code that runs.

Its job is deliberately narrow:

- establish a stable real-mode environment;
- preserve the information needed by the next stage;
- load the second-stage loader from the boot image;
- transfer control to the second stage.

If the first stage cannot read the second stage, it reports the failure and
stops. It does not attempt to continue with an incomplete boot.

## Second-stage loader

The second stage performs the work needed to prepare the kernel environment.

### Memory discovery

The loader asks the platform firmware for the system memory map. It records the
usable and reserved regions and makes the map available to the kernel.

Memory discovery is a prerequisite for everything that follows. The kernel
cannot manage physical memory, build page tables, or place system images without
a reliable map of what memory exists and what must be reserved.

If the firmware memory map is invalid, the boot process reports the failure and
stops.

### Loading the kernel and system images

The loader places the following into memory:

- the kernel image;
- the Init image;
- the Console image.

Each image has a reserved destination and a size limit enforced by the boot
path. The loader verifies that the images fit and that their memory regions do
not overlap. A failure to load any required image is reported and stops the
boot.

### Transfer to the kernel

After the kernel and system images are in place, the loader transfers control to
the kernel entry point.

## Kernel entry

The kernel entry path establishes the execution environment required by the
kernel:

1. establish the processor's protected-mode environment;
2. prepare the kernel's execution state;
3. prepare the initial virtual memory environment;
4. enable paging;
5. enter the kernel's main initialization routine.

These steps are part of the public architecture at a conceptual level. The
specific processor structures, addresses, and instruction sequences are private
implementation details.

## Kernel initialization

The kernel initializes its services in a dependency order. The public sequence
is:

1. **Execution environment.** Establish the processor state and the kernel's
   basic runtime environment.
2. **Diagnostic logging.** Make it possible to record boot progress and
   failures before other services exist.
3. **Interrupt and exception handling.** Prepare the system to receive hardware
   events and processor faults.
4. **Timing.** Start the system timer that drives preemption, timeouts, and
   uptime.
5. **Memory discovery and management.** Classify physical memory, initialize
   page management, and prepare virtual memory.
6. **Privilege-transition support.** Prepare the structures needed for
   user-mode execution.
7. **Scheduling.** Initialize the task scheduler and its idle context.
8. **Objects, handles, IPC, and synchronization.** Bring up the kernel's
   resource model and services.
9. **Platform services.** Initialize firmware power management and other
   platform-dependent services. A failure here is treated as non-fatal where
   the system can continue safely.
10. **System interface publication.** Publish the interface that Init and
    Console use to interact with the kernel.
11. **Interrupts enabled.** Enable interrupts and complete kernel startup.

Each stage depends on the guarantees established by the previous stages. A
fatal failure produces a diagnostic message and stops the system instead of
continuing with an undefined state.

## Handoff to Init

After kernel initialization, control passes to the Init image.

Init is the first system image to run after the kernel. Its role in the
documented 0.18 line is deliberately small:

- establish its own execution environment;
- perform its initial user-mode preparation;
- hand control to the Console image.

Init is not yet a general service manager or application launcher. It is a
handoff stage in the boot architecture.

## Handoff to Console

Console is the interactive system image. It receives control from Init and:

- validates the kernel interface it was given;
- initializes the VGA text console;
- reads keyboard input;
- presents a command prompt;
- provides built-in commands for system information, memory information,
  uptime, help, screen clearing, reboot, shutdown, echo, and the user-mode
  demonstration;
- launches a user-mode task when requested.

Console does not access kernel memory directly. It uses the interface published
by the kernel during initialization.

## Boot-time interfaces

The boot path and the kernel exchange a small set of public, well-defined
information:

- the platform memory map;
- the location and size of the kernel and system images;
- the kernel interface used by Init and Console;
- the firmware services needed for early boot.

The exact binary layouts and addresses are private implementation details and are
not documented here.

## Failure behavior

The boot architecture defines explicit failure behavior:

| Failure | Result |
| --- | --- |
| First-stage loader cannot read the second stage | Diagnostic message and halt |
| Firmware memory map is invalid | Diagnostic message and halt |
| Kernel image cannot be loaded | Diagnostic message and halt |
| Init image cannot be loaded | Diagnostic message and halt |
| Console image cannot be loaded | Diagnostic message and halt |
| A fatal kernel initializer fails | Diagnostic message and halt |
| A non-fatal platform service is unavailable | Warning logged; boot continues where safe |

The system does not silently skip a required image or continue with an invalid
memory map.

## Boot validation

The boot architecture is implemented and compiled into the distributed disk
image. The current validation position is:

- **Build:** the system builds a complete bootable image.
- **Host tests:** kernel and system services are exercised by host-side test
  suites.
- **BIOS/QEMU:** earlier sessions recorded successful boots and console use.
  The latest documentation pass did not reproduce a QEMU or physical-hardware
  boot.
- **Physical hardware:** coverage is limited and not broadly claimed.

See [Development Status](../Development/Development-Status.md) for the current
evidence matrix.

## Relationship to the rest of the system

The boot path is the beginning of the system architecture, not a separate
product. It establishes the conditions that the kernel, Init, Console, and user
tasks rely on:

- a known memory map;
- a controlled processor environment;
- loaded system images;
- a published kernel interface;
- a defined failure path.

The rest of the architecture is described in
[Architecture Overview](Overview.md), [Kernel Architecture](Kernel-Architecture.md),
and [System Architecture](System-Architecture.md).

## Related documentation

- [Architecture Overview](Overview.md)
- [Kernel Architecture](Kernel-Architecture.md)
- [System Architecture](System-Architecture.md)
- [Memory Management](../Components/Memory-Management.md)
- [Process Management](../Components/Process-Management.md)
- [Development Status](../Development/Development-Status.md)
- [Version History](../Development/Version-History.md)

---

Theia OS is proprietary software. This document describes public boot
architecture and does not disclose private implementation details.
