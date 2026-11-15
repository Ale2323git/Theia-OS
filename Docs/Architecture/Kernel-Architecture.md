# Kernel Architecture

The heart of Theia OS is the **Theia NT Kernel**, a custom kernel designed from
the ground up for this operating system. It is not the Linux kernel, not a Unix
kernel, and not a Windows NT derivative. "Theia NT" is the name of Theia's own
kernel architecture.

This document explains the kernel's public architectural identity: what it is
responsible for, how it separates privilege levels, and why its hybrid design
matters. It does not describe private data structures, internal interfaces, or
implementation details.

## Hybrid architectural identity

Theia NT is a **hybrid kernel**. Its design combines two ideas that are often
treated as opposites:

- **Monolithic performance characteristics.** Core services run in kernel mode
  and can operate directly on hardware and kernel state. This keeps critical
  paths direct and avoids the overhead of routing every operation through a
  separate user-mode server.
- **Microkernel security principles.** User-visible resources are represented as
  kernel objects accessed through opaque handles. Communication uses explicit
  message ports. The user/kernel boundary is enforced through privilege and
  address-space separation rather than convention.

The hybrid identity is not a compromise between two unrelated designs. It is a
deliberate placement of boundaries:

- boundaries that provide real protection are enforced;
- boundaries that would only add complexity without improving safety are not
  added;
- the kernel remains one coherent component rather than a collection of
  separately scheduled servers.

This is why Theia OS can describe itself as having monolithic performance
characteristics without giving up the structural separation associated with
microkernel-inspired systems.

## Core responsibilities

The kernel is responsible for the system's privileged operations:

| Responsibility | Public description |
| --- | --- |
| Processor control | Establishing protected mode, privilege levels, and the execution environment |
| Interrupts and exceptions | Receiving hardware events and processor faults, and dispatching them to the correct handler |
| Timing | Maintaining the system timer and monotonic uptime |
| Memory | Managing physical pages, virtual address spaces, and memory protection |
| Scheduling | Selecting runnable tasks and performing context switches |
| Objects and handles | Managing kernel object lifetimes and per-task handle tables |
| IPC | Providing bounded message-passing between tasks |
| Synchronization | Providing events, mutexes, and semaphores |
| System API | Validating and dispatching user-mode service requests |
| Diagnostics | Maintaining kernel logs and system information |
| Platform services | Real-time clock access, firmware power management, and console support |

The kernel does not currently provide a file system, networking stack, USB
stack, audio stack, or accelerated graphics driver. Those services are outside
the documented 0.18 kernel scope.

## Privilege and isolation

Theia OS enforces a strict distinction between kernel mode and user mode.

### Kernel mode

Kernel-mode code can:

- execute privileged processor instructions;
- access kernel memory and hardware;
- manage page tables and address spaces;
- service interrupts and exceptions;
- create and manage kernel objects.

### User mode

User-mode code:

- runs with restricted processor privileges;
- can access only the memory mapped into its own address space;
- refers to kernel resources through opaque handles;
- requests kernel services through the system API;
- cannot read or write kernel memory directly.

The boundary is enforced by the processor and by the kernel's validation of
user-supplied arguments. User-mode code is treated as untrusted input, even when
it is part of the system's own user environment.

## Scheduling model

Theia NT uses a preemptive, round-robin scheduler for single-processor systems.

Key characteristics:

- tasks are selected from the set of runnable tasks;
- a running task can be preempted by the timer;
- a task can voluntarily yield, block, sleep, or wait;
- blocked tasks do not consume processor time;
- task termination follows an orderly cleanup path;
- the scheduler records ownership relationships between tasks.

The scheduler is not a real-time scheduler, and the current system does not
provide symmetric multiprocessing. Theia OS does not claim hard real-time
guarantees or multi-core scheduling.

The task model is described in [Process Management](../Components/Process-Management.md).

## Memory architecture

The kernel's memory architecture is built around isolation and explicit
ownership.

At a high level:

- the boot path provides a platform memory map;
- the kernel classifies usable and reserved physical memory;
- physical pages are allocated and released through the kernel's page manager;
- virtual memory maps pages into address spaces with controlled permissions;
- each user task can have its own address space;
- user buffers are validated before the kernel accesses them;
- shared memory is reference-tracked so a page is not released while still in
  use.

The kernel does not currently provide demand paging from a file system, a swap
subsystem, or a general-purpose file-backed memory model. Memory is managed
explicitly and within bounded capacities.

The memory model is described in
[Memory Management](../Components/Memory-Management.md).

## Objects and handles

The kernel's object model is one of the most important parts of its
microkernel-inspired design.

- A **kernel object** is a resource managed by the kernel, such as a port,
  event, mutex, or semaphore.
- The kernel tracks each object's type, lifetime state, and reference count
  internally. None of this metadata is exposed to user mode.
- A **handle** is an opaque value that a user task uses to refer to an object.
  Handles are private to a task's handle table.
- A **generation tag** makes stale handles detectable. If a handle slot is
  reused, an old handle value does not silently resolve to the new object.

User-mode code never receives a kernel pointer. It receives a handle whose
validity and type are checked when it is used.

The object model also defines a controlled destruction path. When the last
reference to an object is released, the object is retired through a deferred
destruction process rather than being freed while other components may still be
observing it.

## IPC and synchronization

The kernel provides two broad categories of inter-task communication:

### Message ports

Ports provide bounded message-based communication. A port has a message queue
with a fixed capacity and a maximum message size defined by the public system
API. Tasks can send, receive, call, and reply through port handles.

Port operations follow explicit ownership and authorization rules. A port is
not a global namespace, and a handle from one task's table is not valid in
another task's table.

### Synchronization objects

The kernel provides:

- **Events** for binary signaling, with manual-reset and auto-reset behavior;
- **Mutexes** for ownership-based mutual exclusion;
- **Semaphores** for counting-based resource signaling.

These objects integrate with the scheduler's wait mechanism. A task that cannot
proceed can block on an object instead of polling, and the scheduler will make
it runnable when the object's state changes or a timeout expires.

## System API boundary

User-mode code requests kernel services through the system API. The kernel's
role at this boundary is to:

1. validate the request's arguments and user buffers;
2. verify handles, task ownership, and permissions;
3. perform the requested operation;
4. copy results back to user memory;
5. report success or a defined error status.

Validation happens before kernel state is modified. This ordering is a core
part of the kernel's security model: invalid input must not be able to mutate
kernel state.

The System API is the public contract for the documented 0.18 services. It is a
foundation, not a complete final interface, and additional services are
expected. Its detailed numeric identifiers and binary layouts are part of the
proprietary interface specification and are not reproduced in this
documentation.

**Standway** is a separate, planned application-facing API. It is not part of
the kernel and is not implemented in the 0.18 system. The current system images
interact with the System API directly.

## Interrupts, exceptions, and faults

The kernel owns the processor's interrupt and exception handling.

- Hardware interrupts are received by the kernel and routed to the appropriate
  service.
- The system timer drives preemption, timeouts, and uptime accounting.
- Processor exceptions in user mode are contained at the user/kernel boundary.
- A user-mode fault can terminate the offending task without granting it kernel
  privileges.
- Fatal kernel or boot failures follow a defined diagnostic path and stop the
  system rather than continuing in an undefined state.

The kernel does not claim that every hardware interrupt path has been validated
on every platform. See [Development Status](../Development/Development-Status.md)
for the current evidence level.

## Diagnostics

The kernel includes a bounded logging service that records system events with
severity, category, timestamp, and message information. Boot-time and runtime
components can report milestones and failures through this service.

The kernel also exposes controlled system information through the system API,
including:

- memory information;
- the boot memory map;
- scheduler statistics;
- kernel identity and version information;
- system time and uptime;
- per-task last-error information.

The logging service is a diagnostic aid, not a general persistent system log.
The current public system API does not provide a read, drain, or flush operation
for the kernel log.

## Boot and initialization role

The kernel is entered after the Theia boot path has loaded it into memory. During
initialization, the kernel establishes:

1. the processor execution environment;
2. interrupt and exception handling;
3. timing;
4. memory discovery and management;
5. virtual memory and privilege transition support;
6. task scheduling;
7. objects, handles, IPC, and synchronization;
8. platform services such as firmware power management;
9. the interface used by the system images that follow.

After initialization, the kernel hands control to the Init image and remains
available to service system calls and interrupts.

See [Boot Architecture](Boot-Architecture.md) for the boot sequence.

## Design trade-offs

The hybrid design involves explicit trade-offs:

| Choice | Benefit | Cost |
| --- | --- | --- |
| Core services in kernel mode | Direct, predictable performance | A kernel fault can affect the whole system |
| Object and handle boundaries | Strong user/kernel separation and stale-handle detection | Additional validation and bookkeeping on each operation |
| Bounded resource pools | Predictable memory use and failure behavior | Fixed limits on concurrent objects and tasks |
| Single-processor scheduling | Simpler synchronization and scheduling model | No multi-core scalability today |
| BIOS boot path | Direct control of the boot process and broad virtual-machine compatibility | No UEFI support in the documented line |
| Native system API | A coherent Theia OS interface | No POSIX or Linux compatibility |

These trade-offs are intentional. They reflect the project's current stage and
its architectural priorities, not an absence of design.

## Current limits

The documented 0.18 kernel:

- targets single-processor systems;
- uses a native BIOS boot path;
- has fixed-capacity kernel resource pools;
- does not provide a file system, networking, USB, audio, or accelerated
  graphics; a dedicated persistent filesystem is a long-term goal;
- does not provide the Standway application-facing API or a native application
  ecosystem;
- does not provide a graphical desktop; a desktop experience is a long-term goal;
- does not provide POSIX, Unix, Linux, or Windows compatibility;
- has limited physical-hardware validation;
- is not production-ready.

## Related documentation

- [Architecture Overview](Overview.md)
- [System Architecture](System-Architecture.md)
- [Boot Architecture](Boot-Architecture.md)
- [Memory Management](../Components/Memory-Management.md)
- [Process Management](../Components/Process-Management.md)
- [Development Status](../Development/Development-Status.md)

---

Theia OS is proprietary software. This document describes the kernel's public
architecture and does not disclose private implementation details.
