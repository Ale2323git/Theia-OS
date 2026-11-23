# System API

The System API is the authorized interface through which Ring 3 userland
requests kernel services. It is not a POSIX or Linux API and does not expose
private kernel structures.

## Eight service areas

| Area | Purpose |
| --- | --- |
| **System** | System identity, state, and core queries |
| **Diagnostics** | Bounded logging and diagnostic reporting |
| **Text Output** | Controlled text output to the display |
| **Memory** | Allocation, mapping, protection, query, and sharing concepts |
| **Handle** | Handle lifecycle operations |
| **Task** | Task creation, query, wait, yield, sleep, and termination |
| **Synchronization** | Events, mutexes, and semaphores |
| **IPC** | Ports, messages, calls, and replies |

This list is the public 0.18 scope. Internal helpers are not additional public
areas.

## Public boundary

The System API has a public userland surface and a private privileged
implementation. Authorized userland components use the public surface. Internal
interfaces and privileged implementation details are not published as userland
APIs.

## Request model

A userland request is validated, performed by the appropriate kernel service, and
reported through a defined result. Invalid requests do not change system state.
Low-level binary details are private.

## Area details

### System

System identity, version, state, and time-related queries used by components such
as Mashel.

### Diagnostics

Controlled logging, system information, and diagnostic reporting for the user
environment.

### Text Output

Text output to the display through a public service. Mashel uses it instead of
direct hardware access.

### Memory

Physical, virtual, and user memory concepts, permissions, and controlled
sharing. See [Memory Management](../Components/Memory-Management.md).

### Handle

Lifecycle operations for opaque references to kernel-managed resources.

### Task

Task creation and lifecycle operations. The system schedules Tasks, not
executables. See [Process Management](../Components/Process-Management.md).

### Synchronization

Events, mutexes, and semaphores integrated with task waiting. See
[IPC and Synchronization](../Components/IPC-and-Synchronization.md).

### IPC

Bounded port messages with send, receive, call, and reply operations. See
[IPC and Synchronization](../Components/IPC-and-Synchronization.md).

## Relationship to Standway

Standway is the planned application-facing API for 0.19, codename Pequita. It will
sit above the System API for native applications. It is not implemented in 0.18,
and no application should be represented as using it today.

## Related documentation

- [System Interface](../ABI/Syscall-ABI.md)
- [Kernel Architecture](../Architecture/Kernel-Architecture.md)
- [Mashel](../Components/Mashel.md)
- [Features](../Introduction/Features.md)
- [Roadmap](../Development/Roadmap.md)

---

Theia OS is proprietary software. This document describes public System API
behavior and does not disclose private implementation details.
