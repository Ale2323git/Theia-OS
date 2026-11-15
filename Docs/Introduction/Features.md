# Features

This page describes the capabilities of Theia OS as of the documented **0.18**
development line. It separates what is implemented from what is limited, what is
not part of the current system, and what is only a future direction.

Theia OS is an active foundational project. A capability being listed as
implemented does not mean it has been validated on every platform or that it is
ready for production use. The evidence behind each status is summarized in
[Development Status](../Development/Development-Status.md).

## Status vocabulary

| Status | Meaning |
| --- | --- |
| **Implemented** | The capability exists in the documented system and is part of the current architecture. |
| **Implemented — validation limited** | The capability exists, but important validation (especially on physical hardware) is incomplete or was not reproduced in the latest documentation pass. |
| **In development** | Work on the capability is underway, but it is not yet implemented or verified as a usable feature. |
| **Partially implemented** | Some of the intended behavior exists, but the capability is not complete enough to be treated as a finished feature. |
| **Planned** | The project intends to work on the capability, but it is not implemented and no delivery date is promised. |
| **Deferred** | The capability is intentionally not being pursued in the current scope. |
| **Not implemented** | The capability is absent from the documented 0.18 system. Its inclusion here is not a commitment to build it. |
| **Future goal** | A long-term architectural aspiration, not a scheduled feature. |

## Implemented

### Kernel and platform

- **Custom Theia NT Kernel.** A from-scratch kernel with a hybrid architectural
  identity: monolithic performance characteristics combined with
  microkernel-inspired object, handle, and communication boundaries.
- **Independent system architecture.** The kernel, boot path, task model,
  memory model, and system API are designed as one system rather than assembled
  from another operating system's components.
- **Protected-mode execution.** The processor runs in protected mode with
  virtual memory enabled.
- **User/kernel separation.** User-mode code and kernel-mode code have distinct
  privileges and memory access.
- **Preemptive multitasking.** The scheduler can switch between runnable tasks
  without requiring cooperative yielding.
- **Single-processor design.** The current scheduler and kernel are designed for
  single-processor systems.
- **BIOS boot path.** A native two-stage boot path loads the kernel and system
  images without GRUB, UEFI, or another operating system's loader.
- **Bootable disk image.** The system is distributed as a complete `.img` image
  that can be booted in a suitable virtual machine or on compatible hardware.
- **Fault handling.** The kernel handles processor exceptions, user-mode
  faults, and fatal boot or runtime failures with defined behavior.
- **System shutdown and reboot.** The console provides shutdown and reboot
  commands; power-off uses firmware power management when the platform supports
  it.

### Memory

- **Physical memory discovery.** The boot path obtains the platform's memory
  map, and the kernel classifies usable and reserved memory.
- **Physical page management.** Physical memory is tracked at page granularity
  with explicit allocation and release.
- **Virtual memory.** Paging provides virtual-to-physical translation and
  memory protection.
- **Per-task address spaces.** User tasks run with their own address spaces
  rather than sharing a single global user address space.
- **Memory permissions.** User memory can be mapped with controlled read and
  write access.
- **Controlled memory sharing.** Pages can be shared between related tasks
  under explicit rules, with reference tracking so a shared page is not freed
  while still in use.
- **User-buffer validation.** The kernel validates user-supplied buffers before
  reading or writing them, rather than trusting user pointers.
- **Memory system calls.** User-mode code can allocate, free, map, unmap,
  protect, query, share, and unshare memory through the system API.
- **Memory information.** The system can report physical memory, the boot memory
  map, and scheduler-related memory information through diagnostics.

### Tasks and scheduling

- **Task model.** Theia OS uses tasks rather than a POSIX process model. Tasks
  can be kernel tasks or user tasks.
- **Kernel and user tasks.** Kernel tasks run with kernel privileges; user tasks
  run in user mode with their own address spaces.
- **Task states.** Tasks move through explicit states such as ready, running,
  blocked, and terminated.
- **Round-robin scheduling.** Runnable tasks are selected in round-robin order.
- **Blocking and waiting.** Tasks can block on synchronization objects, wait
  queues, sleep timeouts, or completion of another task.
- **Task ownership.** The scheduler records parent/child ownership, and
  operations such as waiting, terminating, querying, and sharing memory respect
  that relationship.
- **Task creation and termination.** Tasks can be created, queried, waited on,
  and terminated through the system API, with orderly cleanup on exit.
- **Task identity.** Public task identifiers are generation-tagged handles, so a
  stale identifier cannot silently refer to a different task after a slot is
  reused.
- **Scheduler statistics.** The system reports task counts by state, the current
  task, context-switch counts, and timer ticks.
- **User-mode exception handling.** User-mode faults are isolated from the
  kernel and reported through the system's diagnostic paths.

### Objects, handles, IPC, and synchronization

- **Kernel objects.** Ports, events, mutexes, and semaphores are represented as
  kernel objects with managed lifetimes.
- **Per-task handle tables.** User-mode code refers to kernel objects through
  opaque handles private to its task. Handles cannot be used across task
  boundaries without an explicit sharing mechanism.
- **Handle generations.** Handle slots carry generation information, so a closed
  or stale handle cannot resolve to a newer object.
- **Deferred object destruction.** Objects are retired through a controlled
  destruction path rather than being freed while references may still exist.
- **Port IPC.** Tasks can create ports and exchange bounded messages through
  send, receive, call, and reply operations.
- **Events.** Binary events support manual-reset and auto-reset behavior, with
  wait and signal operations.
- **Mutexes.** Mutexes provide ownership-based mutual exclusion with waiting,
  ownership transfer, and termination handling.
- **Semaphores.** Counting semaphores provide bounded resource signaling with
  waiting and signal operations.
- **Directed object sharing.** Objects can be offered to a directly owned child
  task through a controlled claim mechanism instead of a global handle space.

### System API and diagnostics

- **System API foundation.** A defined interface covers tasks, memory, handles,
  IPC, synchronization, and system services for the documented 0.18 scope. It is
  not a complete final API, and additional services are expected.
- **System information queries.** The system can report memory information, the
  boot memory map, scheduler statistics, kernel identity, and ABI information.
- **Time and uptime.** The system reports real-time clock time in UTC and
  monotonic uptime.
- **Last-error reporting.** Each task has a record of the most recent failed
  known system call, allowing user-mode code to diagnose failures without kernel
  access.
- **Kernel logging.** The kernel maintains a bounded log with severity,
  category, timestamp, and message information. User-mode code can submit
  application log records subject to validation and rate limits.
- **Console diagnostics.** The console provides commands for system
  information, memory information, uptime, help, screen clearing, reboot,
  shutdown, echo, and a user-mode demonstration.

### User environment

- **Init image.** A separate Init image prepares the transition from the kernel
  to the console environment.
- **Console image.** A separate Console image provides the interactive VGA text
  console and keyboard input.
- **Built-in commands.** The console supports help, detailed help, system
  information, memory information, uptime, reboot, shutdown, screen clearing,
  echo, and a user-mode system-call demonstration.
- **User-mode demonstration.** A user task exercises task creation, memory
  queries, system queries, time, uptime, last-error reporting, and logging
  through the system API.

## Implemented — validation limited

These capabilities exist, but the documentation does not claim more validation
than has been recorded.

| Capability | Status | Limitation |
| --- | --- | --- |
| Interactive console and keyboard | Implemented — validation limited | Builds and has recorded boot use; full interactive behavior has not been revalidated on physical hardware in the latest documentation pass. |
| BIOS/QEMU boot | Implemented — validation limited | Earlier sessions recorded successful boots; the latest documentation pass did not reproduce a QEMU or hardware boot. |
| ACPI power-off | Implemented — validation limited | Implemented; no physical platform accepted a real power-off request during the latest documented validation. |
| Real-time clock | Implemented — validation limited | Time decoding is host-tested; direct hardware clock access was not revalidated in the latest pass. |
| Interrupt and exception paths | Implemented — validation limited | Implemented and compiled; the full interrupt and exception paths are not covered by host-side tests. |
| Physical hardware support | Implemented — validation limited | QEMU is the primary validation environment; broad physical hardware coverage is not claimed. |

## Partially implemented

The current 0.18 line is a foundation, so several areas are intentionally
partial rather than complete:

- **System API coverage.** The 0.18 System API covers the core services
  described above, but it is not the complete long-term API. The separate
  application-facing API, Standway, is planned and not implemented.
- **User environment.** The console and user-mode demonstration exist, but there
  is no general application runtime, application loader, or user desktop.
- **Device support.** The system has keyboard, timer, VGA text output, real-time
  clock, and firmware power-management support, but no broad driver model or
  plug-and-play device ecosystem.
- **Platform coverage.** The documented boot path targets BIOS-class systems.
  It does not provide UEFI boot or support for a wide range of machines.
- **Diagnostics access.** The kernel log is bounded and has no public read,
  drain, or flush interface. It is a diagnostic aid, not a general system log.
- **Time services.** Real-time clock time is reported at whole-second
  resolution; sub-second time comes from the system's monotonic uptime source,
  not from the real-time clock.

These limitations are part of the current design, not hidden defects. They are
documented so readers can judge the system accurately.

## Not implemented

The following capabilities are absent from the documented 0.18 system. Items
marked as long-term goals belong to the project's architectural direction, but
they are still not current features. Other items are not promises that they
will be built.

- A general-purpose file system or persistent storage stack. A dedicated
  persistent filesystem is a long-term goal.
- Networking and network protocols.
- USB support.
- Audio support.
- Accelerated graphics or a graphical desktop environment. A desktop is a
  long-term goal.
- Symmetric multiprocessing.
- UEFI boot.
- POSIX, Unix, Linux, or Windows compatibility layers.
- A general third-party application ecosystem. A native application ecosystem is
  a long-term goal.
- A package manager or software repository.
- A system installer.
- A publicly distributed source tree, SDK, or build system.
- A stable driver development interface for third parties.
- Multi-user accounts or a security model for untrusted local users.
- Virtualization support for running other operating systems.
- Power management beyond the implemented shutdown and reboot paths.

## Planned and deferred

The project itself is in active development. A planned component is not
described as available until its behavior has been implemented and verified.

### Planned and long-term

The following are part of Theia OS's architectural direction. None of them is a
complete feature in the documented 0.18 line.

- **Standway — planned.** A separate application-facing API intended to sit
  above the System API and serve native applications. Standway is not
  implemented, and no interface details or delivery date are published.
- **Dedicated persistent filesystem — planned.** A Theia OS filesystem intended
  to make persistent storage a first-class system component. No filesystem is
  present in 0.18.
- **Native application ecosystem — planned.** A native development model,
  application interfaces, and consistent facilities for Theia OS applications.
  These do not exist yet.
- **Graphical desktop — long-term goal.** A complete desktop experience built on
  the system's own architecture. The current user environment is a VGA text
  console.
- **General-purpose everyday computing — long-term goal.** The project's
  long-term ambition, not a description of the current system.

The near-term planning position remains validation and hardening of the 0.18
foundation. See the [Roadmap](../Development/Roadmap.md).

### Deferred

- **Legacy platform compatibility.** Theia OS does not treat compatibility with
  another operating system's legacy interfaces as a design goal.
- **POSIX compatibility.** A POSIX layer is not part of the current
  architecture and is not scheduled.
- **Compatibility-oriented application environments.** These are not part of the
  documented scope.

### Future goals

The following are long-term architectural aspirations, not dated commitments:

- broader hardware coverage and a more complete device model;
- a richer user environment with native applications, development tools, and a
  complete desktop experience;
- stronger verification of the kernel's isolation and fault-containment
  properties;
- additional platform support where it fits the project's architecture;
- continued evolution of the public system API without breaking existing
  contracts.

## How to read feature claims

Theia OS documentation uses evidence levels rather than marketing language:

- **Implemented** means the behavior is part of the documented system.
- **Implemented — validation limited** means the behavior exists but has not
  been fully validated on the relevant hardware or environment.
- **In development** means work is underway, but the capability is not yet
  implemented or verified as usable.
- **Partially implemented** means the capability is present only in a limited
  form.
- **Planned** means the capability belongs to the approved long-term direction
  but is not implemented, and no delivery date is promised.
- **Not implemented** means it is absent. It is not described as if it were
  available.
- **Long-term goal** means the project is interested in the direction, but no
  delivery is promised.

For the current verification status, see
[Development Status](../Development/Development-Status.md). For recorded
milestones, see [Version History](../Development/Version-History.md).

---

Theia OS is proprietary software. This feature list describes public
capabilities only and does not disclose private implementation details.
