# System Architecture

Theia OS is more than a kernel. It is a complete system structure that begins at
the firmware boundary and continues through kernel services, system images, and
the user-mode console.

This document explains how the major parts of the system fit together, what
interfaces separate them, and how the system behaves when something fails. It
focuses on public architecture rather than private implementation.

For the kernel's internal identity, see [Kernel Architecture](Kernel-Architecture.md).
For the boot sequence, see [Boot Architecture](Boot-Architecture.md).

## System composition

| Component | Role | Runs in |
| --- | --- | --- |
| Platform firmware | Starts the boot process and provides basic platform services | Firmware |
| Theia boot path | Discovers memory and loads the kernel and system images | Firmware/boot environment |
| Theia NT Kernel | Processor control, interrupts, memory, scheduling, objects, IPC, synchronization, diagnostics | Kernel mode |
| System API | Defines how user-mode code requests kernel services | Kernel/user boundary |
| Standway (planned) | Intended application-facing API for native applications | Planned; not implemented |
| Init image | Prepares the transition from the kernel to the interactive environment | User mode/system image |
| Console image | Provides the VGA text console, keyboard input, and built-in commands | User mode/system image |
| User tasks | Execute application or demonstration code in isolated address spaces | User mode |
| Diagnostics | Reports boot progress, system state, time, uptime, and errors | Kernel and console |

Each component has a defined responsibility. The kernel does not embed the
console's command logic, and the console does not access kernel memory directly.

## End-to-end flow

At a high level, a running Theia OS system follows this path:

1. **Firmware starts the boot path.** The platform firmware transfers control to
   the first stage of the Theia loader.
2. **The boot path discovers memory.** The loader obtains the platform memory
   map and prepares the environment for the kernel.
3. **The kernel is loaded and started.** The boot path places the kernel in
   memory and transfers control to it.
4. **The kernel initializes the system.** Processor state, interrupts, timing,
   memory, scheduling, objects, IPC, synchronization, diagnostics, and platform
   services are brought up in a defined order.
5. **Init runs.** The Init image performs the system's initial user-mode
   preparation and hands control to the console image.
6. **Console runs.** The console uses the kernel's published interface to query
   system information, read the keyboard, and launch the user-mode
   demonstration.
7. **User tasks run.** A user task executes in its own address space and
   requests kernel services through the system API.

The flow is intentionally sequential during boot. Later components depend on
guarantees established by earlier ones.

## The system API boundary

The System API is the contract between user mode and the kernel for the
documented 0.18 services. It is a foundation, not a complete final API. It is
organized by service area:

| Service area | Purpose |
| --- | --- |
| Tasks | Identify, create, query, wait for, yield, sleep, and terminate tasks |
| Memory | Allocate, free, map, unmap, protect, query, share, and unshare memory |
| Handles | Close and duplicate object handles |
| IPC | Create ports and send, receive, call, and reply to messages |
| Synchronization | Create and operate events, mutexes, and semaphores |
| System | Query system information, read time and uptime, retrieve the last error, and write diagnostic log records |

The API is designed around a consistent request/response model:

- the caller supplies arguments and, where needed, a user buffer;
- the kernel validates the arguments and the buffer;
- the kernel verifies handles and ownership;
- the kernel performs the operation;
- the kernel copies results back to user memory;
- the kernel returns a status that describes success or failure.

A request that fails validation must not change kernel state. This rule is
central to the system's security model.

The detailed numeric identifiers and binary layouts of the system API are part
of the proprietary interface specification. They are not reproduced in this
public documentation.

### Standway, the planned application-facing API

Theia OS distinguishes the kernel-service boundary from the interface intended
for applications. The System API is the kernel-service boundary. **Standway** is
the planned application-facing API that native applications would use.

Standway is not implemented in the documented 0.18 system. No Standway
interface details, behavior, or delivery date are published. The current console
and user-mode demonstration use the System API directly and are not Standway
clients.

The long-term intent is a clear separation between system interfaces and
application interfaces, so that native applications can be built against a
stable application model rather than against kernel-service calls directly. That
separation is an architectural goal, not a current capability.

## Objects and handles as system resources

Theia OS uses a uniform resource model across kernel services.

- **Objects** represent kernel-managed resources. Ports, events, mutexes, and
  semaphores are objects.
- **Handles** are opaque references to objects. A handle belongs to one task's
  handle table and is not a global identifier.
- **Reference tracking** keeps an object alive while handles or internal
  operations still refer to it.
- **Deferred destruction** retires an object through a controlled path after
  its last reference is released.
- **Generation tags** make stale handles detectable after a slot is reused.

This model provides several system-wide benefits:

- user-mode code cannot access kernel memory through a handle;
- handles can be type-checked before an operation;
- a closed or stale handle fails instead of silently addressing a different
  object;
- object lifetimes are managed centrally rather than by each service;
- cross-task sharing can be explicit and authorized.

The object model is also the basis for IPC and synchronization. A port, event,
mutex, or semaphore follows the same lifecycle rules as other kernel objects.

## Tasks and address spaces

Theia OS uses tasks as its unit of execution.

- A task has an identity, a state, and an ownership relationship with its
  creator.
- Kernel tasks run with kernel privileges.
- User tasks run in user mode and can have their own address space.
- The scheduler selects runnable tasks and performs context switches.
- Blocked tasks wait on objects, timeouts, or other tasks.
- Termination releases the task's handles, object references, and memory
  resources through defined cleanup paths.

A user task cannot arbitrarily operate on another task. Operations such as
waiting, terminating, querying, and sharing memory are governed by recorded
ownership and explicit authorization rules.

The task model is described in
[Process Management](../Components/Process-Management.md).

## Memory and isolation

Memory isolation is a foundation of the system architecture.

- The boot path supplies the platform memory map.
- The kernel classifies physical memory and manages page allocation.
- Virtual memory maps pages into address spaces with controlled permissions.
- Each user task can have a private address space.
- User buffers are validated before the kernel accesses them.
- Shared memory is tracked with references so pages are not released
  prematurely.

The system does not currently provide a general file-backed virtual memory
system, demand paging from storage, or swapping. Memory is managed explicitly
within bounded capacities.

The memory model is described in
[Memory Management](../Components/Memory-Management.md).

## Init and Console

The system uses separate images for early user-mode preparation and the
interactive environment.

### Init

Init is the first system image after the kernel. Its role is to perform the
initial user-mode step and hand control to the console. In the current 0.18
line, Init is deliberately small; it is a handoff stage, not a general service
manager.

### Console

Console is the interactive system image. It:

- presents the VGA text interface;
- reads keyboard input;
- validates the kernel interface it receives;
- provides built-in commands;
- reports system information, memory information, and uptime;
- supports reboot and shutdown;
- can launch the user-mode demonstration task.

The console does not bypass the system API to inspect kernel internals. It uses
the interface published by the kernel.

## Diagnostics and observability

Theia OS treats diagnostics as part of the architecture.

- **Kernel logging** records boot and runtime events in a bounded log.
- **System information queries** expose memory, scheduler, kernel, and interface
  information.
- **Time and uptime** provide real-time clock time and monotonic uptime.
- **Last-error reporting** lets a user task retrieve the status of its most
  recent failed system call.
- **Console commands** provide a human-readable view of system state.

Diagnostics are designed to be useful without exposing private kernel
structures. The kernel log is bounded and is not a persistent system log; the
public API does not provide a read, drain, or flush operation for it.

## Failure model

Theia OS separates failures by scope.

### Request failures

Invalid arguments, invalid handles, insufficient resources, permission
failures, and unsupported operations are returned to the caller as defined
status values. They do not silently corrupt kernel state.

### User-mode faults

A fault in user mode is handled at the user/kernel boundary. The kernel can
terminate the offending task without granting it kernel privileges. The system
remains responsible for cleaning up the task's resources.

### Resource exhaustion

Kernel services operate within fixed capacities. When a resource is exhausted,
the operation fails with an explicit status rather than overwriting state or
allocating without bound.

### Fatal failures

Fatal boot or kernel failures follow a defined diagnostic path and stop the
system. The system does not pretend to continue after a failure that leaves its
state undefined.

The failure model is supported by the system API's validation order and by the
kernel's separation of user memory from kernel memory.

## Security model

The primary security boundary is between user mode and kernel mode. Additional
boundaries exist between tasks.

The architecture enforces:

- processor privilege separation;
- per-task address spaces for user tasks;
- per-task handle tables;
- validation of user-supplied arguments and buffers;
- verified object types and handle validity;
- ownership checks for task operations and memory sharing;
- explicit authorization for directed object sharing.

The current system does not provide:

- a multi-user account model;
- a general sandbox for untrusted third-party applications;
- a complete driver isolation model;
- a verified formal security proof.

These are not claimed as implemented. The existing boundaries are the
foundation on which future work can build.

## Resource model

Theia OS prefers bounded, predictable resource use in the kernel.

- Core kernel services use fixed-capacity resource pools.
- Object counts and per-task handle usage are limited.
- IPC messages have a fixed maximum size and bounded queues.
- Task slots and address spaces are finite.
- Kernel logging uses a bounded buffer.

These limits make behavior easier to reason about and prevent unbounded kernel
growth. They also mean that resource exhaustion is a normal, reportable
condition rather than an unexpected collapse.

## Initialization order

The kernel initialization order reflects hard dependencies:

1. establish the processor environment;
2. initialize diagnostic logging;
3. set up interrupts and timing;
4. discover and initialize memory;
5. initialize virtual memory and privilege-transition support;
6. initialize scheduling;
7. initialize handles, objects, IPC, and synchronization;
8. initialize platform services;
9. publish the interface used by system images;
10. enable interrupts and hand control to Init.

Each step depends on the guarantees of the steps before it. For example,
scheduling cannot operate before memory and timing are available, and object
services cannot operate before the scheduler can host their worker activity.

## Extensibility

The architecture is intended to grow without losing its identity.

New services should:

- fit the object and handle model where they expose user-visible resources;
- validate user input at the system API boundary;
- respect the kernel's bounded-resource discipline;
- report failures through defined status values;
- avoid exposing private kernel structures to user mode;
- preserve the stability of existing public contracts.

No specific future service is promised by this document. Extensibility is an
architectural property, not a feature list.

## Current constraints

The documented 0.18 system:

- runs on single-processor systems;
- boots through BIOS, not UEFI;
- has a fixed-capacity kernel resource model;
- has no general file system, networking, USB, audio, or accelerated graphics; a
  dedicated persistent filesystem is a long-term goal;
- has no Standway application-facing API or native application ecosystem;
- has no graphical desktop; a desktop experience is a long-term goal;
- has no POSIX, Unix, Linux, or Windows compatibility layer;
- has a small user-mode environment;
- has limited physical-hardware validation;
- is not production-ready.

## Related documentation

- [Architecture Overview](Overview.md)
- [Kernel Architecture](Kernel-Architecture.md)
- [Boot Architecture](Boot-Architecture.md)
- [Memory Management](../Components/Memory-Management.md)
- [Process Management](../Components/Process-Management.md)
- [Features](../Introduction/Features.md)
- [Development Status](../Development/Development-Status.md)

---

Theia OS is proprietary software. This document describes public system
architecture and does not disclose private implementation details.
