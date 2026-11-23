# Features

This page describes Theia OS as of the documented **0.18 System API
Foundation** line. It separates implemented capabilities from limited
validation, planned work, deferred decisions, and absent features.

Status vocabulary is defined in
[Development Status](../Development/Development-Status.md).

## Status vocabulary

| Status | Meaning |
| --- | --- |
| **Implemented** | Present in the documented system. |
| **Verified** | Exercised in the documented validation of the published image. |
| **Limited validation** | Present, but validation is incomplete at the relevant level. |
| **Planned** | Approved direction; not implemented and not dated. |
| **Deferred** | Intentionally outside the current scope. |
| **Not implemented** | Absent from the documented 0.18 line. |
| **Historical** | Kept for context; not the current name or capability. |

## Implemented kernel foundation

- **Theia NT Kernel.** A from-scratch hybrid kernel with Ring 0 services and
  explicit resource and communication boundaries.
- **Native boot.** A BIOS boot path that loads the system without GRUB or UEFI.
- **Boot coordination.** A mandatory coordinator role documented as `initland`
  governs the kernel-to-userland handoff. The kernel does not start Mashel
  directly.
- **Protected execution.** Privilege separation, memory isolation, and fault
  handling.
- **Memory management.** Physical memory, virtual memory, user regions,
  permissions, controlled sharing, and validated access to user-provided data.
- **Task model.** The system schedules Tasks, not executables. Tasks have
  states, ownership, waiting, and orderly cleanup.
- **Resource references.** User-visible resources are referenced through opaque
  handles rather than private pointers.
- **IPC and synchronization.** Bounded message ports, events, mutexes, and
  semaphores.
- **Critical failure handling.** Defined Kernel Panic and Theia OS BSOD
  presentations; fatal paths always end in a safe halt.
- **Diagnostics.** Bounded logging, system information, time, uptime, and
  controlled reporting for the user environment.

## Implemented userland

- **Init in Ring 3.** Started by the kernel; responsible for starting Mashel.
- **Mashel.** The official Ring 3 console component, with the `theia>` prompt.
  It uses public system services and does not access hardware directly.
- **Text Output.** The public service used by Mashel to display information.
- **Published image.** A complete 0.18 image was validated in the reference
  virtual environment up to the Mashel prompt.

> Historical note: "Theia Console" was an earlier name for the interactive
> component. The current official name is Mashel.

## System API

The API has eight implemented areas:

| Area | Purpose |
| --- | --- |
| System | System identity, state, and core queries |
| Diagnostics | Logging and diagnostic reporting |
| Text Output | Controlled text output to the display |
| Memory | Allocation, mapping, protection, query, and sharing concepts |
| Handle | Handle lifecycle operations |
| Task | Task creation, query, wait, yield, sleep, and termination |
| Synchronization | Events, mutexes, and semaphores |
| IPC | Ports, messages, calls, and replies |

The public boundary is the System API. Internal and privileged interfaces are
not userland APIs. Low-level binary details are private.

Standway is the planned application-facing API and is not part of 0.18.

## Verification position

The documented 0.18 validation includes:

- production of a complete bootable image;
- validation of the published image in the reference virtual environment;
- confirmation that the image reaches the Theia OS Mashel banner and `theia>`
  prompt;
- confirmation that the user-visible system information and text output
  services are available in that environment.

Physical-hardware validation remains limited. A virtual-machine boot is not the
same as hardware validation. See
[Development Status](../Development/Development-Status.md).

## Planned

- **0.19 — Standway, codename Pequita.** Separate application-facing API above
  the System API. Not implemented.
- **0.20 — Filesystem Foundation.** Foundation for Theia NTFS. Not implemented.
- **0.21–0.23 — Filesystem maturation.** Planned follow-on filesystem work.
- **0.24 — Program Foundation.** Planned Loader and Executor.
- **Theia NTRD.** Planned temporary filesystem; not a replacement for Theia
  NTFS.
- **Native application ecosystem.** Planned, but dependent on application
  support that does not yet exist.

## Deferred

- Ring 1 and Ring 2 availability until HAL Foundation.
- Mihriban until application support exists.
- General third-party driver interfaces.
- POSIX, Linux, Unix, and Windows compatibility.

## Not implemented in 0.18

- Persistent filesystem or file-backed memory.
- Networking, USB, audio, or accelerated graphics.
- Graphical desktop.
- Symmetric multiprocessing.
- UEFI boot.
- General application loader, package manager, installer, or SDK.
- Public source tree.

## Related documentation

- [What Is Theia OS?](What-Is-Theia-OS.md)
- [Development Status](../Development/Development-Status.md)
- [Roadmap](../Development/Roadmap.md)
- [System API](../System-API/System-API.md)
- [Build and Validation](../Development/Build-and-Validation.md)

---

Theia OS is proprietary software. This feature list describes public
capabilities only and does not disclose private implementation details.
