# Architecture Overview

Theia OS is a layered operating system with the Theia NT Kernel at its center.
Its architecture is designed to remain coherent from the firmware boundary to
Ring 3 userland without inheriting the structure of Linux, Unix, POSIX, or
another operating system.

## Architectural principles

1. **One system, one model.** Boot, kernel, memory, tasks, resources, System API,
   and userland are designed together.
2. **Explicit privilege boundaries.** Ring 0 kernel and Ring 3 userland are the
   active levels. Ring 1 and Ring 2 are deferred for general availability until
   HAL Foundation.
3. **Hybrid kernel structure.** Core services stay in Ring 0 for directness,
   while resources and communication form explicit boundaries.
4. **Opaque user-visible resources.** Userland receives references, not private
   kernel structures.
5. **Bounded resources.** Kernel services operate within known limits.
6. **Long-lived contracts.** Public interfaces have defined behavior and result
   reporting.
7. **Evidence-based claims.** Documentation distinguishes implemented,
   verified, planned, deferred, historical, and unverified items.

## System layers

| Layer | Responsibility | Public description |
| --- | --- | --- |
| Firmware and native boot | Platform initialization and loading of system components | [Boot Architecture](Boot-Architecture.md) |
| Boot coordination | Mandatory handoff role documented as `initland` | [Boot Architecture](Boot-Architecture.md) |
| Kernel core | Processor state, interrupts, timing, memory, scheduling, faults | [Kernel Architecture](Kernel-Architecture.md) |
| Kernel services | Resources, IPC, synchronization, diagnostics | [Kernel Architecture](Kernel-Architecture.md) |
| System API | Authorized userland boundary with eight service areas | [System API](../System-API/System-API.md) |
| System interface | Public overview; low-level details are private | [System Interface](../ABI/Syscall-ABI.md) |
| Ring 3 userland | Init and Mashel | [System Architecture](System-Architecture.md) |
| User environment | Mashel commands and text output | [Mashel](../Components/Mashel.md) |

The layers are parts of one operating system, not independent products.

## Kernel model

The Theia NT Kernel is a hybrid kernel:

- **Monolithic characteristic:** core services execute in Ring 0 and operate
  directly on memory, interrupts, and managed platform resources.
- **Microkernel-inspired boundaries:** user-visible resources are referenced
  opaquely, communication uses bounded message channels, and user input is
  validated before kernel state changes.

This is not a pure microkernel and does not claim that every service runs in a
separate user-mode server.

## Privilege and execution model

- **Ring 0:** kernel and privileged kernel services.
- **Ring 3:** Init, Mashel, and user tasks.
- **Ring 1 and Ring 2:** reserved by the architecture and deferred for general
  availability until HAL Foundation. They are not public userland facilities
  and are not part of the normal user environment.

The scheduler schedules **Tasks**, not executables. Tasks may be kernel tasks or
user tasks. User tasks have isolated address spaces and consume kernel services
through the System API.

See [Process Management](../Components/Process-Management.md).

## Memory model

- The boot path obtains basic memory information from firmware.
- Physical memory is tracked and classified.
- Virtual memory provides isolation and permissions.
- User memory regions have ownership and controlled sharing.
- User-provided data is validated before the kernel uses it.
- No demand paging, swap, or file-backed memory is provided in 0.18.

See [Memory Management](../Components/Memory-Management.md).

## Objects, handles, IPC, and synchronization

Kernel-managed resources are referenced through opaque handles. Message ports
carry bounded communication. Synchronization objects integrate with task
waiting. Details of their internal organization are not published.

See [IPC and Synchronization](../Components/IPC-and-Synchronization.md).

## System API and system interface

The System API has eight areas: System, Diagnostics, Text Output, Memory,
Handle, Task, Synchronization, and IPC. Its public surface is available to
authorized userland; the privileged implementation remains private.

Standway is the planned application-facing API and is not part of 0.18. See
[System API](../System-API/System-API.md) and
[System Interface](../ABI/Syscall-ABI.md).

## Boot and initialization

1. Firmware starts the native Theia boot path.
2. The boot path prepares the system and loads kernel and userland components.
3. Kernel initialization establishes Ring 0 services.
4. The mandatory boot coordinator, documented as `initland`, governs the
   handoff to userland.
5. The kernel starts Init in Ring 3.
6. Init starts Mashel in Ring 3 through the public system interfaces.
7. Mashel presents the interactive environment.

The kernel never skips Init to start Mashel directly. See
[Boot Architecture](Boot-Architecture.md).

## Failure model

- Invalid requests return defined results.
- User-mode faults are contained at the user/kernel boundary.
- Resource exhaustion is reported explicitly.
- Fatal kernel failures use a defined presentation and always end in a safe
  halt.

## Documented constraints

- single-processor execution;
- BIOS boot, not UEFI;
- bounded kernel resources;
- no persistent filesystem in 0.18;
- no Standway, native application loader, or desktop;
- no general third-party driver model;
- limited physical-hardware validation;
- not production-ready.

## Reading guide

- [Kernel Architecture](Kernel-Architecture.md) for kernel identity and duties.
- [System Architecture](System-Architecture.md) for composition and boundaries.
- [Boot Architecture](Boot-Architecture.md) for the actual boot path.
- [System API](../System-API/System-API.md) for the public service model.
- [Features](../Introduction/Features.md) for status distinctions.

---

Theia OS is proprietary software. This overview describes public architecture
only and does not disclose private implementation details.
