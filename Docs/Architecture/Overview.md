# Architecture Overview

Theia OS is a layered operating system with a custom kernel at its center. Its
architecture is designed to keep the system coherent from the firmware boundary
to user-mode tasks, without inheriting the structure of Linux, Unix, POSIX, or
another operating system.

This overview explains the major architectural layers and how they relate. More
detailed public descriptions are available in:

- [Kernel Architecture](Kernel-Architecture.md)
- [System Architecture](System-Architecture.md)
- [Boot Architecture](Boot-Architecture.md)
- [Memory Management](../Components/Memory-Management.md)
- [Process Management](../Components/Process-Management.md)

## Architectural principles

Theia OS follows a small set of principles consistently across its layers:

1. **One system, one model.** The boot path, kernel, memory model, task model,
   object model, and system API are designed to fit together.
2. **Explicit privilege boundaries.** User-mode code and kernel-mode code are
   separated by privilege, address space, and a defined system interface.
3. **Hybrid kernel structure.** Core services stay close to the kernel for
   directness and performance, while objects, handles, and communication are
   kept behind explicit boundaries.
4. **Opaque user-visible resources.** User-mode code receives handles and
   identifiers, not kernel pointers or private structures.
5. **Bounded core resources.** Kernel services operate within known capacity
   limits rather than relying on unbounded allocation.
6. **Long-lived contracts.** Public interfaces are treated as durable contracts
   with defined behavior and error reporting.
7. **Evidence-based claims.** The documentation describes what is implemented,
   what is limited, and what is absent.

## System layers

| Layer | Responsibility | Public description |
| --- | --- | --- |
| Firmware and boot | Platform initialization, memory discovery, loading the kernel and system images | [Boot Architecture](Boot-Architecture.md) |
| Kernel core | Processor state, interrupts, timing, memory, scheduling, fault handling | [Kernel Architecture](Kernel-Architecture.md) |
| Kernel services | Objects, handles, IPC, synchronization, logging, diagnostics | [Kernel Architecture](Kernel-Architecture.md) |
| System API | Defined interface between user mode and kernel services for the 0.18 scope | [System Architecture](System-Architecture.md) |
| Application-facing API | Standway, a planned interface for native applications | Planned; not implemented |
| System images | Init, Console, and user-mode tasks | [System Architecture](System-Architecture.md) |
| User environment | Interactive console, built-in commands, demonstration task | [Features](../Introduction/Features.md) |

The layers are not independent products. They are parts of one operating
system, and each layer depends on the guarantees provided by the layers below
it.

## Kernel model

Theia OS uses the **Theia NT Kernel**, a custom hybrid kernel.

The "monolithic" part of its identity refers to the fact that core services
operate in kernel mode and can interact directly with the hardware and with one
another. This keeps important paths direct and predictable.

The "microkernel-inspired" part refers to the way the kernel structures
user-visible resources:

- kernel objects have managed lifetimes;
- user-mode code refers to objects through opaque handles;
- communication uses explicit message ports rather than shared kernel
  structures;
- the boundary between user mode and kernel mode is enforced by privilege and
  address-space separation;
- kernel services validate user-supplied data before acting on it.

This is a hybrid design, not a claim that Theia OS is a microkernel. The system
does not move every service into separate user-mode servers. It places
boundaries where they provide meaningful protection and keeps the core kernel
coherent.

## Execution model

Theia OS uses preemptive multitasking on single-processor systems.

- **Kernel tasks** execute with kernel privileges.
- **User tasks** execute in user mode with their own address spaces.
- **The scheduler** selects runnable tasks in round-robin order.
- **Blocking** allows a task to wait for an object, a timeout, or another task
  without consuming processor time.
- **Termination** is orderly: a task's resources and object references are
  released through defined cleanup paths.

The task model is native to Theia OS. It is not a POSIX process model, and the
system does not present POSIX process semantics as its public interface.

See [Process Management](../Components/Process-Management.md) for details.

## Memory model

Memory management is built around isolation and explicit ownership:

- the boot path discovers the platform memory map;
- the kernel manages physical pages and virtual address spaces;
- each user task can have its own address space;
- user memory is mapped with controlled permissions;
- memory can be shared between related tasks under explicit rules;
- the kernel validates user buffers before accessing them.

The memory model does not currently include a general-purpose file-backed
virtual memory system, demand paging from storage, or a swap subsystem. Those
capabilities are outside the documented 0.18 scope.

See [Memory Management](../Components/Memory-Management.md) for details.

## Objects, handles, IPC, and synchronization

The kernel provides a resource model shared by its services:

- **Objects** represent kernel-managed resources such as ports, events, mutexes,
  and semaphores.
- **Handles** are opaque, per-task references to objects. A handle is not a
  pointer and cannot be used by another task without an explicit sharing
  mechanism.
- **Ports** provide bounded message-based inter-process communication.
- **Events, mutexes, and semaphores** provide synchronization between tasks.

The object and handle model is the main microkernel-inspired part of the
architecture: user-mode code never receives direct access to kernel object
storage. It receives a capability-like reference whose validity is checked on
each use.

## System API

User-mode code interacts with the kernel through a defined System API. The 0.18
API is a foundation, not a complete final interface. It is organized by service
area:

- task management;
- memory management;
- handle management;
- inter-process communication;
- synchronization;
- system information and diagnostics.

The API is designed around explicit argument validation, explicit result
reporting, and defined error behavior. A failed request does not silently
corrupt kernel state.

The System API is a public contract for the documented 0.18 services. It is a
foundation, not the complete final API, and additional services are expected.
Its numeric identifiers and binary layouts are not documented here because they
are part of the proprietary interface specification. This documentation
describes the service model and the behavior that users of the API can rely on.

### Standway (planned)

Above the System API, Theia OS intends a separate application-facing API called
**Standway**. The System API is the kernel-service boundary; Standway is intended
to be the interface that native applications use. Standway is planned and is not
part of the 0.18 system. No Standway interface details, behavior, or delivery
date are published.

The current console and user-mode demonstration use the System API directly.
They are not Standway clients.

## Boot and initialization

The boot path is native to Theia OS:

1. Platform firmware starts the first-stage Theia loader.
2. The loader starts a second stage that discovers available memory.
3. The kernel is loaded into memory and started.
4. The kernel initializes processor state, interrupts, timing, memory,
   scheduling, objects, IPC, synchronization, diagnostics, and platform
   services.
5. The kernel hands control to the Init image.
6. Init prepares the transition to the Console image.
7. Console presents the interactive environment.

The boot path does not use GRUB, UEFI, or another operating system's loader in
the documented 0.18 line.

See [Boot Architecture](Boot-Architecture.md) for details.

## Failure model

Theia OS distinguishes between different classes of failure:

- **Invalid requests** return an error to the caller through the system API.
- **User-mode faults** are contained at the user/kernel boundary and do not
  automatically compromise the kernel.
- **Resource exhaustion** is reported through explicit status values rather than
  silent corruption.
- **Fatal kernel or boot failures** stop the system with a defined diagnostic
  path.

The failure model is part of the architecture, not an afterthought. User-mode
input is treated as untrusted, and kernel operations are expected to validate it
before changing kernel state.

## Security boundaries

The primary security boundary is between user mode and kernel mode. Within user
mode, tasks are separated by address spaces and by per-task handle tables.

The architecture also applies these rules:

- user-mode code cannot dereference kernel pointers;
- user-mode buffers are validated before the kernel reads or writes them;
- handles are private to a task and are checked for type and validity;
- object sharing requires an explicit, authorized mechanism;
- task ownership governs operations such as waiting, terminating, and sharing
  memory;
- the kernel does not expose private data structures through public interfaces.

The current system does not yet provide a multi-user security model, a
general-purpose sandbox for untrusted third-party applications, or a complete
driver isolation model. Those are not claimed as implemented.

## Current architectural constraints

The documented 0.18 architecture has these deliberate limits:

- single-processor execution;
- a native BIOS boot path rather than UEFI;
- a fixed-capacity kernel resource model;
- no general file system or persistent storage stack;
- no networking stack;
- no Standway application-facing API or native application ecosystem;
- no graphical desktop;
- no POSIX or Linux compatibility layer;
- a small user-mode environment centered on the console and demonstration
  tasks.

These constraints define the current system. They are not presented as
permanent limits on the project's long-term vision, but they are also not
scheduled features.

## Long-term platform direction

The architecture is intended to grow into a complete, general-purpose platform.
The stated direction includes:

- a dedicated persistent filesystem;
- the Standway application-facing API;
- a native application ecosystem and development facilities;
- a coherent graphical desktop experience;
- broader hardware and device coverage.

These are long-term goals, not capabilities of the documented 0.18 system. No
specific sequence, release date, or compatibility commitment is published. The
[Roadmap](../Development/Roadmap.md) states the planning position, and
[Features](../Introduction/Features.md) distinguishes implemented from planned
work.

## Reading guide

- Start with [Kernel Architecture](Kernel-Architecture.md) to understand the
  kernel's identity and responsibilities.
- Read [System Architecture](System-Architecture.md) to see how the kernel,
  system images, and user environment fit together.
- Read [Boot Architecture](Boot-Architecture.md) to follow the system from
  firmware to console.
- Read the component documents for the public behavior of
  [memory management](../Components/Memory-Management.md) and
  [process management](../Components/Process-Management.md).

---

Theia OS is proprietary software. This overview describes public architecture
only and does not disclose private implementation details.
