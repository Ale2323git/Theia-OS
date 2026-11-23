# What Is Theia OS?

Theia OS is an independent, proprietary operating system built from the ground
up. It has its own kernel, boot path, system architecture, System API, and user
environment. It is not a Linux distribution and is not based on Unix, POSIX,
Windows, or another operating system.

Its long-term ambition is to become a complete, general-purpose operating
system. The documented 0.18 line is recorded as a foundation, not that finished
platform.

## At a glance

| Aspect | Description |
| --- | --- |
| Identity | Independent proprietary operating system |
| Kernel | Theia NT Kernel, hybrid design |
| Boot | Native BIOS boot with a mandatory coordinator role documented as `initland` |
| Privilege levels | Ring 0 kernel and Ring 3 userland are active |
| User environment | Mashel, the official Ring 3 console component |
| System API | Eight areas: System, Diagnostics, Text Output, Memory, Handle, Task, Synchronization, IPC |
| Memory | Isolated user memory, permissions, and validated access |
| Task model | The system schedules Tasks, not executables |
| Filesystem | Theia NTFS planned for persistent storage; Theia NTRD planned for temporary files |
| Documented historical milestone | 0.18 System API Foundation |
| Source | Private; no source tree is published |

## The kernel

The Theia NT Kernel is a hybrid kernel. It combines direct Ring 0 services with
microkernel-inspired boundaries around resources and communication. The name
"Theia NT" is Theia's own kernel identity; it does not imply Windows NT
derivation or compatibility.

The active privilege boundary is Ring 0/Ring 3. Ring 1 and Ring 2 are part of the
long-term architecture but are deferred for general availability until HAL
Foundation. Their future status must not be read as a currently available
application facility.

## Boot and userland

The native boot path starts from BIOS, prepares the system, and loads the kernel
and userland components. A mandatory coordinator role, documented as
`initland`, governs the handoff to userland. The kernel starts Init in Ring 3;
Init then starts Mashel. The kernel never skips Init to start Mashel directly.

Mashel is the official name of the interactive console component. "Theia
Console" is a historical name only and is not used as the current component
name. Mashel consumes public system services and does not access hardware
directly.

## System API

Userland requests kernel services through the System API. The eight public areas
are:

1. System
2. Diagnostics
3. Text Output
4. Memory
5. Handle
6. Task
7. Synchronization
8. IPC

The System API is the authorized userland boundary. Internal and privileged
interfaces are not public APIs. Low-level binary details are private and are not
published as part of the product documentation.

Standway is planned as the application-facing API above the System API. It is
not part of 0.18.

## What Theia OS is not

- Not a Linux distribution, Unix-like system, POSIX implementation, or Windows
  compatibility layer.
- Not a GRUB-based system or a UEFI boot project in the documented line.
- Not a hobbyist boot demonstration or a kernel-only experiment.
- Not a source distribution; the public repository contains documentation and
  release images.
- Not production-ready; the documented 0.18 line is recorded as foundational.

## Documented 0.18 state

The documented 0.18 record includes:

- the native boot path and coordinator role documented as `initland`;
- Ring 3 Init and Ring 3 Mashel;
- the Theia NT Kernel, memory management, Task scheduling, and task lifecycle;
- resource references, IPC, synchronization, and diagnostics;
- the eight System API areas;
- a published image verified in the reference virtual environment.

Standway, Theia NTFS, Theia NTRD, Program Foundation, a native application
ecosystem, a graphical desktop, Ring 1/Ring 2 availability, Mihriban, and HAL
Foundation are planned or deferred, not part of the documented 0.18
capabilities.

See [Features](Features.md) and
[Development Status](../Development/Development-Status.md) for the full status
distinction.

## Where to go next

- Read the [Vision](Vision.md) for long-term direction.
- Review [Features](Features.md) for capability status.
- Continue to the [Architecture Overview](../Architecture/Overview.md).
- Use the [Glossary](../Resources/Glossary.md) for terminology.

---

Theia OS is proprietary software. This document describes public information
only and does not disclose private implementation details.
