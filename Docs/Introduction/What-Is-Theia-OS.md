# What Is Theia OS?

Theia OS is an independent, proprietary operating system built from the ground
up. It has its own kernel, boot path, system architecture, and system API. It is
not a Linux distribution, and it is not based on Unix, POSIX, or Linux.

Its long-term ambition is to become a complete, general-purpose operating system
suitable for everyday computing. That ambition extends beyond the kernel to
persistent storage, an application-facing API, a native application ecosystem,
and a coherent desktop experience. Those are long-term goals, not current
capabilities.

Theia OS is developed around a custom kernel called the **Theia NT Kernel**. The
kernel has a hybrid architectural identity: it combines monolithic performance
characteristics with microkernel security principles. This means the system
keeps core services close to the hardware for directness and efficiency, while
using explicit object, handle, and message boundaries to separate user-mode code
from kernel-mode code.

The project is in active foundational development. Version 0.18 establishes the
system call and diagnostics foundation on top of earlier work on memory
management, scheduling, objects, IPC, and synchronization. It is not yet a
production operating system, and it does not claim compatibility with other
operating systems.

## Theia OS at a glance

| Aspect | Description |
| --- | --- |
| Identity | Independent proprietary operating system |
| Kernel | Theia NT Kernel, a custom hybrid kernel |
| Design origin | Built from first principles; not derived from Linux, Unix, or POSIX |
| Architecture | Layered kernel and system services with a defined System API foundation |
| Execution model | Preemptive multitasking on single-processor systems |
| Isolation | Separate user and kernel modes, per-task address spaces |
| Platform | Native BIOS boot path for x86-class systems |
| Distribution | Complete bootable `.img` disk images through GitHub Releases |
| Source availability | Private; no source code is published |
| Current milestone | 0.18, system call and diagnostics foundation |
| Maturity | Foundational development; not production-ready |
| Long-term ambition | Complete, general-purpose operating system for everyday computing |
| Application-facing API | Standway, planned; not implemented |
| Persistent storage | Dedicated filesystem, planned; not implemented |
| User environment | VGA text console today; graphical desktop is a long-term goal |

## What Theia OS is

- **An operating system project with its own architecture.** The kernel, boot
  path, task model, memory model, object model, and system interfaces are
  designed as parts of one system.
- **A hybrid kernel design.** Theia NT keeps performance-sensitive work close
  to the kernel while enforcing structured boundaries around objects, handles,
  and communication.
- **A system built around explicit contracts.** Public interfaces are treated
  as stable contracts rather than incidental details of a particular build.
- **A platform for controlled user-mode execution.** User tasks run in their own
  address spaces and interact with the kernel through a defined system API.
- **A documented, evidence-based project.** Public documentation distinguishes
  what is implemented, what is limited, and what is not yet part of the system.
- **Proprietary software.** The implementation is private, and the public
  repository distributes documentation and complete disk images rather than
  source code.

## Architectural direction

Theia OS is being developed as a complete system rather than a kernel alone. Its
architectural direction combines a low-level kernel with the structure needed for
a general-purpose platform:

- **A dedicated hybrid kernel.** Core services run close to the hardware, while
  objects, handles, and communication are kept behind explicit boundaries.
- **A centralized System API.** User-mode code requests kernel services through
  one defined boundary instead of accessing kernel state directly. The 0.18
  System API is an implemented foundation, not a complete final interface.
- **A separate application-facing API through Standway.** Standway is planned as
  the interface for native applications, distinct from the kernel-service
  boundary. It is not implemented in 0.18.
- **Structured task and object management.** Tasks, objects, handles, and
  ownership relationships are first-class system concepts.
- **Handle-based resource management.** User-mode code refers to kernel resources
  through opaque, validated handles rather than kernel pointers.
- **Inter-process communication.** Bounded message ports provide explicit
  communication between tasks.
- **Memory isolation and virtual memory.** User tasks run in separate address
  spaces, with validated buffers at the kernel boundary.
- **A dedicated persistent filesystem.** A Theia OS filesystem is planned as a
  first-class component for persistent storage. It is not implemented in 0.18.
- **A future native application ecosystem.** Native development facilities and a
  consistent application model are long-term goals, not current features.

These elements are intentionally part of one architecture. The kernel, System
API, application-facing API, storage model, and user environment are meant to
form a coherent platform rather than a collection of unrelated components.

## Inspiration and identity

Windows is one of the sources of inspiration for Theia OS. Mature desktop
operating systems demonstrate the value of a complete desktop experience,
clearly separated system and application interfaces, structured management of
processes and resources, native application development facilities, and a
consistent application ecosystem.

Inspiration is not imitation. Theia OS has its own kernel, architecture,
terminology, implementation, and technical identity. It is not a Windows clone,
a Windows reimplementation, or a Windows-compatible operating system, and no
such compatibility is claimed.

## What Theia OS is not

- **Not a Linux distribution.** It does not use the Linux kernel, a Linux
  userland, or Linux packaging and system conventions.
- **Not Unix or POSIX.** It does not implement a POSIX environment or present
  Unix-style interfaces as its native model.
- **Not a Windows clone or compatible system.** The name "Theia NT" identifies
  Theia's own kernel. It does not imply a Windows NT derivative, a Windows
  reimplementation, or Windows compatibility. Windows is an inspiration for
  certain platform concepts, not a template for the system's identity.
- **Not a hobbyist mini-OS.** Theia OS is an early-stage project, but it is
  engineered as a long-term operating system with a coherent architecture,
  stable interface contracts, and a deliberate development process.
- **Not a source distribution.** The public repository does not contain the
  kernel source, drivers, build scripts, private headers, or internal design
  documents.
- **Not production-ready.** The current system is a foundation. It does not yet
  provide a general application ecosystem, a file system, networking, a
  graphical desktop, or broad hardware support. A dedicated persistent
  filesystem, a native application ecosystem, and a desktop experience are
  long-term goals, not current features.

## How the system is organized

Theia OS is organized in layers:

1. **Firmware and boot.** Platform firmware starts a native Theia boot path.
   The boot path discovers available memory, loads the kernel, and prepares the
   system images that follow.
2. **Kernel core.** The kernel establishes processor state, interrupt handling,
   timing, memory management, scheduling, and fault handling.
3. **Kernel services.** Objects, handles, IPC, synchronization, logging, and
   system information provide the services used by the rest of the system.
4. **System API.** A defined boundary lets user-mode code request kernel
   services without direct access to kernel memory or private structures. The
   0.18 API is an implemented foundation, not a complete final interface.
5. **System images and user mode.** Init prepares the system, Console provides
   the interactive environment, and user tasks run with isolated address
   spaces.
6. **Diagnostics.** The system reports boot progress, runtime state, time,
   uptime, and errors in a controlled way.

The [Architecture Overview](../Architecture/Overview.md) explains these layers
in more detail.

## Current state

The current public milestone is **Theia OS 0.18**. The system includes:

- a native BIOS boot path and a bootable disk image;
- protected-mode execution with virtual memory;
- physical and virtual memory management;
- a preemptive task scheduler;
- kernel objects, handles, IPC ports, events, mutexes, and semaphores;
- a System API foundation for tasks, memory, handles, IPC, synchronization, and
  system information. The API is not complete;
- kernel logging, system information, time, uptime, and last-error reporting;
- a VGA text console with keyboard input and built-in commands;
- separate Init and Console images and a user-mode demonstration.

The System API is implemented as a foundation for these service areas; it is not
complete. **Standway**, a dedicated persistent filesystem, a native application
ecosystem, and a graphical desktop are planned or long-term components and are
not present in 0.18.

The system does not yet include a persistent filesystem, networking, USB, audio,
accelerated graphics, a graphical desktop, symmetric multiprocessing, or UEFI
boot. These are outside the documented 0.18 scope. See
[Development Status](../Development/Development-Status.md) for the complete
status matrix.

## Who this documentation is for

This documentation is written for:

- readers evaluating Theia OS as an operating system project;
- users who want to run a published disk image;
- developers who need to understand the public architecture and system model;
- technical readers who want to follow the project's progress without access to
  its private source code.

It explains **what** Theia OS is, **what** its documented components do, and
**why** its architectural choices matter. It does not disclose how proprietary
internals are implemented.

## Where to go next

- Read the [Vision](Vision.md) for the project's long-term direction.
- Review [Features](Features.md) for a capability-by-capability status list.
- Continue to the [Architecture Overview](../Architecture/Overview.md).
- Use the [Glossary](../Resources/Glossary.md) for unfamiliar terminology.

---

Theia OS is proprietary software. Copyright © 2026 Ale López. All rights
reserved. No source code or open-source license is provided through this
documentation.
