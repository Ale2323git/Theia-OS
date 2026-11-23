# System Architecture

Theia OS is more than a kernel. This document explains how firmware, boot
coordination, Ring 0 services, the System API, Ring 3 Init, and Mashel fit
together.

## System composition

| Component | Role | Runs in |
| --- | --- | --- |
| Platform firmware | Starts the system and provides early platform services | Firmware |
| Native Theia boot path | Prepares the system and loads the kernel and userland components | Boot environment |
| Kernel | Processor, memory, tasks, resources, communication, synchronization, diagnostics | Ring 0 |
| Boot coordinator | Mandatory handoff role documented as `initland` | Kernel-controlled boot path |
| System API | Authorized kernel-service boundary | Public userland surface / private privileged implementation |
| Init | First Ring 3 system component; starts Mashel | Ring 3 |
| Mashel | Official interactive console component | Ring 3 |
| Diagnostics | Logging, state, time, uptime, and results | Ring 0 and Ring 3 |

There is no active kernel interface table in the public architecture. Userland
uses the System API, not private kernel structures.

## End-to-end flow

1. Firmware starts the native Theia boot path.
2. The boot path prepares the system and loads kernel and userland components.
3. Kernel initialization establishes Ring 0 services.
4. The mandatory boot coordinator governs the userland handoff.
5. Init is created as a Ring 3 component.
6. Init starts Mashel through the public system interfaces.
7. Mashel presents the command prompt and uses public system services.
8. User tasks, where present, use the same System API boundary.

The kernel never starts Mashel directly.

## System API boundary

The System API is the authorized interface for userland kernel services. It has
eight areas:

| Area | Purpose |
| --- | --- |
| System | System identity and core state |
| Diagnostics | Controlled logging and diagnostic reporting |
| Text Output | Text output to the display |
| Memory | Allocation, mapping, protection, query, and sharing concepts |
| Handle | Handle lifecycle operations |
| Task | Task creation, query, wait, yield, sleep, and termination |
| Synchronization | Events, mutexes, and semaphores |
| IPC | Ports, messages, calls, and replies |

Its public surface is available to authorized userland. The privileged
implementation and internal interfaces are private. See
[System API](../System-API/System-API.md).

## System interface and Standway

Userland components request kernel services through the defined System API.
Low-level binary details are not published. Standway is planned as the
application-facing API for 0.19 and is not part of the 0.18 system.

See [System Interface](../ABI/Syscall-ABI.md).

## Resources and handles

Kernel-managed resources are referenced through opaque handles. Handles are
private to their owner and are not kernel pointers. Sharing and transfer are
explicit and authorized.

## Tasks and address spaces

The scheduler schedules Tasks. Kernel tasks run in Ring 0; user tasks run in
Ring 3 with isolated address spaces. Ownership governs waiting, termination,
querying, and sharing. Termination releases resources through defined cleanup.

See [Process Management](../Components/Process-Management.md).

## Memory and isolation

The boot path supplies basic memory information. The kernel manages physical
memory, virtual mappings, user regions, permissions, and validated access.
There is no demand paging or swap in 0.18.

See [Memory Management](../Components/Memory-Management.md).

## Init and Mashel

### Init

Init is the first Ring 3 component. It confirms the handoff, prepares the user
environment, and creates Mashel through the public system interfaces. It is not
a general service manager.

### Mashel

Mashel is the official Ring 3 console component. Its banner identifies Theia OS
Mashel and its prompt is `theia>`. It uses public system services for text
output, system information, and authorized communication. It does not access
VGA memory, keyboard ports, or other hardware directly.

Commands for version, system information, memory information, and uptime are
system queries. They are not private kernel interfaces.

See [Mashel](../Components/Mashel.md).

## Diagnostics and observability

Diagnostics include bounded kernel logging, system information, time, uptime,
and results for the user environment. Development diagnostics are not part of
the published public interface.

## Failure model

- Request failures return defined results.
- User faults are contained at the user/kernel boundary.
- Resource exhaustion is explicit.
- Fatal failures use a defined presentation and end in a safe halt.

## Security boundaries

- Ring 0/Ring 3 privilege separation;
- task isolation and private references;
- validation of user-provided data;
- ownership and authorization checks;
- no kernel pointers or private structures in public userland APIs;
- no multi-user account model or general untrusted-application sandbox.

## Documented constraints

- single-processor;
- BIOS boot;
- bounded kernel resources;
- no persistent filesystem in 0.18;
- no general third-party driver model;
- no Standway, native application loader, or desktop;
- limited physical-hardware validation.

## Related documentation

- [Architecture Overview](Overview.md)
- [Kernel Architecture](Kernel-Architecture.md)
- [Boot Architecture](Boot-Architecture.md)
- [System API](../System-API/System-API.md)
- [Mashel](../Components/Mashel.md)
- [Development Status](../Development/Development-Status.md)

---

Theia OS is proprietary software. This document describes public architecture
only and does not disclose private implementation details.
