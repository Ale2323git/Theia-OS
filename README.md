```text
 _____ _   _ _____ _____    ___  _____
|_   _| | | | ____|_   _|  / _ \/  ___|
  | | | |_| |  _|   | |   | | | \ `--.
  | | |  _  | |___  | |   | |_| |`--. \
  |_| |_| |_|_____| |_|    \___/\__/\__/
```

**An independent operating system built from the ground up.**

**Version 0.18 · System API Foundation · Historical milestone · Proprietary software**

Theia OS is an independent operating system with its own kernel, native boot
path, system architecture, System API, and user environment. It is not a Linux
distribution and is not based on Unix, POSIX, Windows, or another operating
system. It does not use GRUB or another operating system's loader.

Its long-term ambition is a complete, general-purpose operating system. The
documented 0.18 line is recorded as a foundation, not a finished platform.

Theia OS has its own internal build and validation process. Its commands,
scripts, parameters, and procedures are not published. This repository is the
public documentation and release home; it does not contain the operating system
source tree. Bootable images are distributed through
[GitHub Releases](https://github.com/Ale2323git/Theia-OS/releases).

---

## At a glance

| Aspect | Position |
| --- | --- |
| Kernel | Theia NT Kernel, hybrid design |
| Boot | Native BIOS path with a mandatory boot coordinator role documented as `initland` |
| Privilege model | Ring 0 kernel, Ring 3 userland |
| User environment | Mashel, the official Ring 3 console component |
| System API | Eight service areas for authorized userland requests |
| Memory | Physical, virtual, and user memory with isolation and access validation |
| Tasks | The system schedules Tasks, not executables |
| Communication | Bounded message ports and synchronization objects |
| Storage | Theia NTFS planned as the primary persistent filesystem |
| Documented historical milestone | 0.18 System API Foundation |
| Maturity | Foundational; not production-ready |
| Source | Private; no source distribution |

## Identity and philosophy

Theia OS is designed from first principles as a coherent platform rather than a
kernel demonstration or a compatibility layer.

- **Independence.** The system has its own architecture, interfaces, task model,
  memory model, and terminology.
- **Hybrid kernel design.** Core services run in Ring 0 while user-visible
  resources are represented through opaque references and explicit boundaries.
- **Explicit privilege levels.** Ring 0 and Ring 3 are the active levels. Ring 1
  and Ring 2 are deferred for general availability until HAL Foundation.
- **Stable public contracts.** The System API has defined behavior; internal
  interfaces are not public userland APIs.
- **Evidence-based documentation.** Implemented, verified, planned, deferred,
  historical, and unverified items are described separately.

Windows is one source of inspiration for the long-term platform ambition, but
Theia OS is not a Windows clone, reimplementation, or compatible system.

Read more in [What Is Theia OS?](Docs/Introduction/What-Is-Theia-OS.md) and
[Vision](Docs/Introduction/Vision.md).

## What 0.18 implemented

- a native BIOS boot path with a mandatory coordinator role documented as
  `initland`;
- Init in Ring 3, started by the kernel and responsible for starting Mashel;
- Mashel as the Ring 3 user environment, using public system services and not
  accessing hardware directly;
- Theia NT Kernel services for processor state, interrupts, timing, memory,
  tasks, communication, synchronization, and diagnostics;
- memory isolation, permissions, and validated access to user-provided data;
- Task scheduling, waiting, and orderly cleanup;
- bounded message ports and synchronization objects;
- System API coverage in eight areas;
- a published bootable image verified in the reference virtual environment,
  showing the Mashel prompt.

The 0.18 line does **not** include a persistent filesystem, Standway, native
application loading, a graphical desktop, networking, USB, audio, SMP, UEFI,
POSIX, Unix, Linux, or Windows compatibility.

See [Features](Docs/Introduction/Features.md) and
[Development Status](Docs/Development/Development-Status.md) for status details.

## Architecture overview

1. **Firmware and native boot.** The firmware starts the Theia boot path, which
   prepares the system and loads the kernel and system components.
2. **Kernel initialization.** The kernel establishes Ring 0 services.
3. **Coordinated userland start.** The mandatory boot coordinator governs the
   transition to userland. The kernel does not skip Init to start Mashel.
4. **Ring 3 userland.** Init starts, then starts Mashel. Mashel presents the
   `theia>` prompt.
5. **System API boundary.** Userland consumes kernel services through the eight
   documented areas. Internal and privileged implementation details remain
   private.
6. **Diagnostics and failure handling.** Fatal kernel failures use a defined
   presentation and always end in a safe halt.

Continue with the [Architecture Overview](Docs/Architecture/Overview.md),
[Boot Architecture](Docs/Architecture/Boot-Architecture.md), and
[System Architecture](Docs/Architecture/System-Architecture.md).

## System API

The System API is the authorized way for Ring 3 components to request kernel
services. Its eight public areas are System, Diagnostics, Text Output, Memory,
Handle, Task, Synchronization, and IPC. Standway is planned as the separate
application-facing API above it.

Low-level binary details and internal interfaces are not published. See
[System API](Docs/System-API/System-API.md) and the
[System Interface overview](Docs/ABI/Syscall-ABI.md).

## Build and validation

Theia OS has a proprietary internal process for producing and validating its
published images. Public documentation describes the result of that process,
not its commands, scripts, parameters, or procedures.

See [Build and Validation](Docs/Development/Build-and-Validation.md).

## Roadmap

| Version | Theme | Status |
| --- | --- | --- |
| 0.18 | System API Foundation | Historical milestone |
| 0.19 | Standway, codename Pequita | Planned |
| 0.20 | Filesystem Foundation | Planned |
| 0.21–0.23 | Filesystem maturation | Planned |
| 0.24 | Program Foundation, Loader and Executor | Planned |

This order is a project decision. No delivery dates are published. Ring 1 and
Ring 2 remain deferred until HAL Foundation, and Mihriban remains deferred while
application support does not exist. See [Roadmap](Docs/Development/Roadmap.md).

## Downloads and releases

Complete bootable `.img` images are distributed through
[GitHub Releases](https://github.com/Ale2323git/Theia-OS/releases). A release
provides a complete system image, not a source archive. Check release notes for
platform requirements, validation level, and known limitations.

For a practical overview, see
[Running Theia OS](Docs/Installation/Running-Theia-OS.md).

## Documentation index

| Section | Document | What it covers |
| --- | --- | --- |
| Introduction | [What Is Theia OS?](Docs/Introduction/What-Is-Theia-OS.md) | Identity, scope, and the system at a glance |
| Introduction | [Vision](Docs/Introduction/Vision.md) | Long-term direction and design philosophy |
| Introduction | [Features](Docs/Introduction/Features.md) | Implemented, limited, planned, and absent capabilities |
| Architecture | [Overview](Docs/Architecture/Overview.md) | System layers and component relationships |
| Architecture | [Kernel Architecture](Docs/Architecture/Kernel-Architecture.md) | Theia NT Kernel and privilege model |
| Architecture | [System Architecture](Docs/Architecture/System-Architecture.md) | System composition, boundaries, and failure model |
| Architecture | [Boot Architecture](Docs/Architecture/Boot-Architecture.md) | Firmware-to-Mashel boot path |
| Components | [Memory Management](Docs/Components/Memory-Management.md) | Memory isolation, permissions, and validation |
| Components | [Process Management](Docs/Components/Process-Management.md) | Tasks, scheduling, waiting, and termination |
| Components | [IPC and Synchronization](Docs/Components/IPC-and-Synchronization.md) | Message ports and synchronization objects |
| Components | [Mashel](Docs/Components/Mashel.md) | Official console component and public services |
| Components | [Filesystem](Docs/Components/Filesystem.md) | Theia NTFS, Theia NTRD, and documented status |
| Components | [HAL and Drivers](Docs/Components/HAL-and-Drivers.md) | HAL Foundation decision and platform scope |
| System API | [System API](Docs/System-API/System-API.md) | Eight service areas and public boundary |
| System interface | [System Interface](Docs/ABI/Syscall-ABI.md) | Public overview; low-level details are private |
| Installation | [Running Theia OS](Docs/Installation/Running-Theia-OS.md) | Booting a published image |
| Development | [Development Status](Docs/Development/Development-Status.md) | Maturity, evidence, and limits |
| Development | [Build and Validation](Docs/Development/Build-and-Validation.md) | General statement about the internal build process |
| Development | [Roadmap](Docs/Development/Roadmap.md) | Version order and planning principles |
| Development | [Version History](Docs/Development/Version-History.md) | Recorded milestones |
| Resources | [Glossary](Docs/Resources/Glossary.md) | Technical terminology |
| Resources | [FAQ](Docs/Resources/FAQ.md) | Common questions |

Start with [What Is Theia OS?](Docs/Introduction/What-Is-Theia-OS.md) if you
are new to the project. Use the [Glossary](Docs/Resources/Glossary.md) for
unfamiliar terms.

## Screenshots

- [Theia-OS-Complete-Window.png](Asserts/Screenshots/Theia-OS-Complete-Window.png)
- [Theia-OS-sysinfo.png](Asserts/Screenshots/Theia-OS-sysinfo.png)
- [Theia-OS.png](Asserts/Screenshots/Theia-OS.png)

The existing `Asserts/` directory name is preserved as part of the repository
structure.

## Repository structure

| Path | Purpose |
| --- | --- |
| `Docs/Introduction/` | Identity, vision, and features |
| `Docs/Architecture/` | Kernel, system, and boot architecture |
| `Docs/Components/` | Public component documentation |
| `Docs/System-API/` | System API areas and public boundary |
| `Docs/ABI/` | Public system-interface overview |
| `Docs/Installation/` | Running published images |
| `Docs/Development/` | Status, roadmap, version history, and build overview |
| `Docs/Resources/` | Glossary and FAQ |
| `Asserts/Screenshots/` | Existing screenshots |
| `README.md` | Entry point and documentation navigation |
| `LICENSE` | Existing license file, preserved without modification |

The repository does not contain kernel sources, drivers, build scripts, private
headers, or internal design documents.

## Issue reporting

Use the [GitHub issue tracker](https://github.com/Ale2323git/Theia-OS/issues)
for documentation corrections, broken links, and reproducible problems with
published disk images. Include the release, execution environment, steps, and
observed behavior. Do not include confidential implementation details.

## Intellectual property

Theia OS is **proprietary software**. Copyright © 2026 Ale López. All rights
reserved.

The source code, internal implementation, build system, and private design
materials are not published. No open-source license is granted. Public
documentation and disk images do not grant rights to the private implementation
or to Theia OS trademarks. Theia OS is provided "as is", without warranty.

The repository includes a `LICENSE` file, preserved as provided. It is not an
open-source license.

---

**Theia OS** — an independent operating system, built from the ground up.
