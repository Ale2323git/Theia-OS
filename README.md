```text
 _____ _   _ _____ _____    ___  _____
|_   _| | | | ____|_   _|  / _ \/  ___|
  | | | |_| |  _|   | |   | | | \ `--.
  | | |  _  | |___  | |   | |_| |`--. \
  |_| |_| |_|_____| |_|    \___/\__/\__/
```

**An independent, proprietary operating system built from the ground up.**

**Version 0.18 · Active development · Proprietary software**

Theia OS is a from-scratch operating system with its own hybrid kernel, system
architecture, and development roadmap. It is not a Linux distribution and is
not based on Unix, POSIX, or Linux. It does not use another operating system's
kernel, compatibility layer, or system architecture.

Theia OS is developed with the long-term ambition of becoming a complete,
general-purpose operating system suitable for everyday computing. That ambition
extends beyond the kernel to persistent storage, an application-facing API, a
native application ecosystem, and a coherent desktop experience. Those are
long-term goals, not current capabilities.

This repository is the public documentation and release home of Theia OS. It
does not contain the operating system's source code. Complete bootable disk
images are distributed through [GitHub Releases](https://github.com/Ale2323git/Theia-OS/releases).

---

## Project identity

- **Independent:** Theia OS is developed as its own operating system, not as a
  derivative of an existing one.
- **Built from the ground up:** its kernel, boot path, system interfaces, and
  user environment are designed as a coherent whole.
- **Proprietary:** the source code and internal implementation are private. No
  open-source license is granted.
- **Hybrid kernel:** the Theia NT Kernel combines monolithic performance
  characteristics with microkernel security principles.
- **Own architecture:** Theia OS has its own system model, task model, memory
  model, object model, and system API. It does not present itself as a
  compatibility environment for another operating system.
- **General-purpose ambition:** Theia OS is developed with the long-term goal of
  becoming a complete, general-purpose operating system suitable for everyday
  computing.
- **A platform, not a demonstration:** the kernel is the first layer of a larger
  system intended to include persistent storage, an application-facing API, a
  native application ecosystem, and a desktop environment.

Theia OS is not a hobbyist mini-OS, a boot demonstration, or a basic educational
operating system. It is an early-stage but deliberately engineered operating
system project with a long-term architectural plan.

## Vision and philosophy

Theia OS exists to explore what an operating system can look like when it is
designed from first principles rather than assembled around inherited
interfaces.

Its guiding ideas are:

- **Clarity over imitation.** The system should have its own coherent design,
  not a collection of compatibility layers.
- **Isolation by construction.** User-mode code and kernel-mode code have
  distinct responsibilities, and the boundary between them is explicit.
- **Performance with discipline.** The kernel is designed to be direct and
  efficient without giving up the structural separation that protects the
  system.
- **Bounded resources.** Core services operate within explicit capacity limits
  instead of relying on unbounded allocation.
- **Long-lived public contracts.** System interfaces are treated as durable
  contracts, not incidental implementation details.
- **Evidence-based progress.** A feature is described according to what has
  actually been implemented and verified, not what is merely intended.

### Inspiration and identity

Windows is one of the sources of inspiration for Theia OS. Mature desktop
operating systems show the value of a coherent desktop experience, clearly
separated system and application interfaces, structured management of processes
and resources, native application development facilities, a consistent
application ecosystem, and practical usability for everyday computing.

Inspiration is not imitation. Theia OS has its own kernel, architecture,
terminology, implementation, and technical identity. It is not a Windows clone,
a Windows reimplementation, or a Windows-compatible operating system, and no
such compatibility is claimed.

Read more in [Vision](Docs/Introduction/Vision.md).

## Architectural direction

Theia OS is designed as a complete system rather than a kernel alone. Its
architectural direction includes:

- a dedicated hybrid kernel — **implemented**;
- a centralized System API for kernel services — **implemented foundation** for
  the documented 0.18 service areas, not a complete final API;
- a separate application-facing API through **Standway** — **planned**;
- structured task and object management — **implemented**;
- handle-based resource management — **implemented**;
- inter-process communication mechanisms — **implemented**;
- memory isolation and virtual memory management — **implemented**;
- a dedicated persistent filesystem — **planned**;
- a future native application ecosystem — **planned**;
- a coherent graphical desktop experience — **long-term goal**.

Only the items marked implemented are present in the documented 0.18 system. The
planned and long-term items describe architectural direction, not current
capability. See [Features](Docs/Introduction/Features.md) for the full status
breakdown.

## Key features

The list below reflects the documented state of the 0.18 development line.
Status terms are defined in the [Development Status](Docs/Development/Development-Status.md)
document.

### Implemented

- A custom Theia NT Kernel with a hybrid monolithic/microkernel-inspired
  architecture.
- A native BIOS boot path that loads the kernel and system images without
  GRUB, UEFI, or another operating system's loader.
- Protected-mode operation with virtual memory and page-level isolation.
- Physical and virtual memory management, including user address spaces,
  memory permissions, and controlled memory sharing.
- A preemptive, round-robin task scheduler for single-processor systems.
- A task model with explicit states, ownership rules, waiting, timeouts, and
  orderly termination.
- A kernel object and handle model that gives user-mode code opaque references
  instead of direct kernel pointers.
- Inter-process communication through bounded message ports.
- Synchronization primitives: events, mutexes, and semaphores.
- A System API foundation covering tasks, memory, handles, IPC, synchronization,
  and system information for the documented 0.18 service areas. The API is not
  complete and is expected to grow.
- System diagnostics: kernel logging, system information queries, time and
  uptime reporting, and per-task last-error reporting.
- A VGA text console with keyboard input and built-in commands, including
  system information, memory information, uptime, reboot, shutdown, and a
  user-mode demonstration.
- System shutdown and reboot support, with firmware power-off where the
  platform provides it.
- Separate Init and Console system images with a defined handoff from the
  kernel.

### Implemented, with validation limits

- The interactive console is implemented and builds into the system image;
  its full behavior has not been revalidated on physical hardware in the most
  recent documentation pass.
- Firmware power management and real-time clock access are implemented, but
  their hardware-specific paths are not fully validated on physical machines.
- Earlier development sessions recorded successful BIOS/QEMU boots. Those runs
  were not reproduced during the latest documentation pass.

### Not implemented in the documented 0.18 line

- A general-purpose file system. A dedicated persistent filesystem is a
  long-term goal.
- Networking.
- USB, audio, or accelerated graphics support.
- A graphical desktop environment. A complete desktop is a long-term goal.
- Symmetric multiprocessing.
- UEFI boot.
- POSIX, Unix, Linux, or Windows compatibility layers.
- A general third-party application ecosystem, package manager, or installer. A
  native application ecosystem is a long-term goal.
- A publicly distributed source tree or build system.

These are not presented as completed features, and no delivery dates are
promised. See the [Roadmap](Docs/Development/Roadmap.md) for the project's
current planning position.

### Planned and long-term direction

The following are part of Theia OS's architectural direction but are **not**
present in the documented 0.18 system:

- **Standway — planned.** A separate application-facing API intended to sit
  above the System API and serve native applications. Standway is not
  implemented, and no interface details or delivery date are published.
- **Dedicated persistent filesystem — planned.** A Theia OS filesystem intended
  to make persistent storage a first-class system component. No filesystem is
  present in 0.18.
- **Native application ecosystem — planned.** A native development model,
  application interfaces, and a consistent ecosystem for Theia OS applications.
  These do not exist yet.
- **Graphical desktop — long-term goal.** A complete desktop experience built on
  the system's own architecture. The current user environment is a VGA text
  console.
- **General-purpose everyday computing — long-term goal.** The project's
  long-term ambition, not a description of the current system.

These items are directional. They are planned or long-term goals rather than
committed milestones, and no delivery dates are promised.

## Architecture overview

Theia OS is organized as a layered system:

1. **Firmware and boot** — the platform firmware starts a native Theia boot
   path, which discovers memory and loads the kernel and system images.
2. **Kernel core** — processor setup, interrupts, timing, memory management,
   scheduling, and fault handling.
3. **Kernel services** — objects, handles, IPC, synchronization, logging, and
   system information.
4. **System API boundary** — a defined interface through which user-mode code
   requests kernel services. The 0.18 API is a foundation, not the final
   complete interface.
5. **System images and user mode** — Init and Console provide the system
   environment, and user tasks can run in their own address spaces.
6. **Diagnostics** — boot-time and runtime reporting that makes system state
   observable without exposing private implementation details.

The kernel is a hybrid design: it keeps core services close to the hardware for
directness and performance, while applying microkernel-inspired boundaries
around objects, handles, and message-based communication. The result is a
single kernel with explicit internal contracts rather than a collection of
unrelated services.

Above the System API, Theia OS intends a separate application-facing API called
**Standway**. Standway is a planned component: it is not part of the 0.18
system, and the current console and demonstration interact with the System API
directly. The long-term intent is to give native applications a stable interface
distinct from the kernel-service boundary.

Continue with the [Architecture Overview](Docs/Architecture/Overview.md).

## Development status

| Area | Status |
| --- | --- |
| Kernel foundation | Implemented |
| Boot path | Implemented; BIOS/QEMU validation recorded, not reproduced in the latest pass |
| Memory management | Implemented and host-tested |
| Task scheduling | Implemented and host-tested |
| Object and handle model | Implemented and host-tested |
| IPC and synchronization | Implemented and host-tested |
| System API | Implemented foundation for the 0.18 service areas; the API is not complete |
| Diagnostics | Implemented and host-tested |
| Standway application API | Planned; not implemented |
| Persistent filesystem | Planned; not implemented |
| Native application ecosystem | Planned |
| Graphical desktop | Long-term goal; not implemented |
| Console and keyboard | Implemented; full interactive validation pending |
| Physical hardware support | Limited; QEMU is the primary validation environment |
| Source availability | Private; no source distribution |
| Production readiness | Not yet; the system is in foundational development |

The current public milestone is **Theia OS 0.18**, which establishes the system
call and diagnostics foundation on top of the scheduler, memory, object, IPC,
and synchronization work from earlier milestones.

See [Development Status](Docs/Development/Development-Status.md) for the full
status matrix and [Version History](Docs/Development/Version-History.md) for
recorded milestones.

## Roadmap

The public roadmap has two horizons. The near-term focus is validation and
hardening of the existing 0.18 foundation: real-hardware boot coverage, console
and input validation, exception and interrupt-path testing, power-management
validation, and documentation of the public contracts.

The long-term direction is a complete, general-purpose platform: a dedicated
persistent filesystem, the Standway application-facing API, a native
application ecosystem, a coherent desktop experience, and broader hardware
support. These are architectural goals without committed milestones. No specific
sequence, release date, or compatibility commitment is approved for public
disclosure.

Storage, networking, additional platform support, and a broader user environment
remain outside the documented 0.18 scope.

Read the [Roadmap](Docs/Development/Roadmap.md) for the full statement of
scope and planning principles.

## Downloads

Complete Theia OS disk images are distributed through
[GitHub Releases](https://github.com/Ale2323git/Theia-OS/releases).

A published release provides a complete bootable `.img` disk image for the
documented platform. Theia OS images are intended for use in a virtual machine
or on compatible hardware. Review the release notes for platform requirements
and known limitations before booting an image.

The repository does not distribute source code, source packages, or buildable
source trees.

## Releases

Release versions and dates are published on the
[GitHub Releases](https://github.com/Ale2323git/Theia-OS/releases) page. This
documentation does not invent or predict them.

- A release image is a complete system image, not a source archive.
- A release does not imply production readiness; check the release notes for the
  validation level and known limitations.
- If no release is currently listed, no public disk image is currently
  available.

## Documentation

| Section | Document | What it covers |
| --- | --- | --- |
| Introduction | [What Is Theia OS?](Docs/Introduction/What-Is-Theia-OS.md) | Identity, scope, and the system at a glance |
| Introduction | [Vision](Docs/Introduction/Vision.md) | Long-term direction and design philosophy |
| Introduction | [Features](Docs/Introduction/Features.md) | Implemented, limited, and unimplemented capabilities |
| Architecture | [Overview](Docs/Architecture/Overview.md) | System layers and component relationships |
| Architecture | [Kernel Architecture](Docs/Architecture/Kernel-Architecture.md) | Theia NT Kernel design and hybrid model |
| Architecture | [System Architecture](Docs/Architecture/System-Architecture.md) | System composition, interfaces, and failure model |
| Architecture | [Boot Architecture](Docs/Architecture/Boot-Architecture.md) | Firmware-to-console boot path |
| Components | [Memory Management](Docs/Components/Memory-Management.md) | Physical memory, virtual memory, and isolation |
| Components | [Process Management](Docs/Components/Process-Management.md) | Tasks, scheduling, waiting, and termination |
| Development | [Development Status](Docs/Development/Development-Status.md) | Current maturity and verification status |
| Development | [Roadmap](Docs/Development/Roadmap.md) | Scope, priorities, and planning position |
| Development | [Version History](Docs/Development/Version-History.md) | Recorded development milestones |
| Resources | [Glossary](Docs/Resources/Glossary.md) | Terminology used across the documentation |
| Resources | [FAQ](Docs/Resources/FAQ.md) | Common questions about Theia OS |

Start with [What Is Theia OS?](Docs/Introduction/What-Is-Theia-OS.md) if you are
new to the project. Use the [Glossary](Docs/Resources/Glossary.md) for
unfamiliar terms.

## Screenshots

The existing screenshot files are available in the repository:

- [Theia-OS-Complete-Window.png](Asserts/Screenshots/Theia-OS-Complete-Window.png)
- [Theia-OS-sysinfo.png](Asserts/Screenshots/Theia-OS-sysinfo.png)
- [Theia-OS.png](Asserts/Screenshots/Theia-OS.png)

The `Asserts/` directory is the repository's existing asset directory. Its name
is preserved as part of the current repository structure.

## Repository structure

| Path | Purpose |
| --- | --- |
| `Docs/Introduction/` | Project identity, vision, and feature overview |
| `Docs/Architecture/` | Kernel, system, and boot architecture |
| `Docs/Components/` | Public documentation for documented components |
| `Docs/Development/` | Status, roadmap, and version history |
| `Docs/Resources/` | Glossary and FAQ |
| `Asserts/Screenshots/` | Existing screenshots of Theia OS |
| `README.md` | Project entry point and documentation navigation |
| `LICENSE` | Existing license file, preserved without modification |

The repository does not contain kernel sources, drivers, build scripts, private
headers, or internal design documents.

## Issue reporting

Public issues are welcome for documentation corrections, broken links, and
reproducible problems with published disk images.

Use the [GitHub issue tracker](https://github.com/Ale2323git/Theia-OS/issues).
When reporting a problem with a disk image, include:

- the release version and image name;
- the virtualization platform or hardware model;
- the firmware mode used to boot the image;
- the exact steps that reproduce the problem;
- the observed behavior and any on-screen message.

Do not include confidential information, private source material, or security
details in a public issue. Report security-sensitive matters privately to the
project owner.

## Intellectual property

Theia OS is **proprietary software**. Copyright © 2026 Ale López. All rights
reserved.

- The source code, internal implementation, build system, and private design
  materials are not published.
- No license, express or implied, is granted to use, copy, modify, adapt,
  publish, distribute, sublicense, sell, or create derivative works of Theia OS
  without prior written permission from the copyright holder.
- The public documentation and disk images do not grant any right to the
  private source code or to the Theia OS name, logo, or trademarks.
- Theia OS is provided "as is", without warranty of any kind. The copyright
  holder is not liable for claims or damages arising from its use.
- The repository includes a `LICENSE` file, preserved as provided. It is not an
  open-source license, and nothing in this repository should be interpreted as
  granting open-source rights.

For licensing inquiries, contact the project owner through the repository.

---

**Theia OS** — an independent operating system, built from the ground up.
