# Kernel Architecture

The privileged core of Theia OS is the **Theia NT Kernel**. It is Theia's own
kernel, not a Linux kernel, Unix kernel, Windows NT derivative, or compatibility
layer. "Theia NT" is a native name and does not imply Windows compatibility.

## Hybrid identity

Theia NT combines:

- **Monolithic performance characteristics:** Ring 0 services operate directly
  on processor state, memory, interrupts, and managed platform resources.
- **Microkernel-inspired security principles:** user-visible resources are
  referenced opaquely; communication uses bounded channels; the user/kernel
  boundary is enforced by privilege and validation.

The result is one coherent kernel with deliberate boundaries, not a collection
of separate user-mode servers.

## Responsibilities

| Responsibility | Public description |
| --- | --- |
| Processor control | Execution state, privilege levels, and processor setup |
| Interrupts and exceptions | Hardware events, processor faults, and timer |
| Timing | System time, timeouts, and uptime |
| Memory | Physical, virtual, and user memory with validated access |
| Scheduling | Task selection and task state management |
| Resources | Opaque references, lifetimes, and ownership |
| IPC | Bounded message channels |
| Synchronization | Events, mutexes, and semaphores |
| System API | Validation and dispatch of authorized userland requests |
| Diagnostics | Bounded logging and controlled system information |
| Critical failures | Defined presentation with a safe halt |
| Platform support | Keyboard, clock, power-state information, and text output support |

The kernel does not provide a filesystem, networking, USB, audio, accelerated
graphics, or a general third-party driver model in 0.18.

## Privilege model

### Ring 0

Ring 0 runs the kernel and privileged kernel services. It manages memory,
interrupts, kernel-managed resources, and the boot handoff.

### Ring 3

Ring 3 runs Init, Mashel, and user tasks. Userland cannot access private kernel
memory or privileged interfaces directly. It uses the System API.

### Ring 1 and Ring 2

Ring 1 and Ring 2 are reserved by the architecture and deferred for general
availability until HAL Foundation. They are not public userland facilities and
are not used to launch the normal user environment.

## Scheduler and Task model

The scheduler is preemptive and schedules **Tasks**, not executables. Tasks move
through general states such as ready, running, waiting, and terminated. They have
ownership, waiting, and cleanup semantics.

There is no POSIX process model, fork/exec semantics, real-time scheduling, or
SMP. See [Process Management](../Components/Process-Management.md).

## Memory architecture

The kernel manages physical memory, virtual mappings, user memory regions, and
validation of user-provided data. Memory is isolated between user tasks and
between userland and the kernel.

No demand paging, swap, or file-backed mapping is provided in 0.18. See
[Memory Management](../Components/Memory-Management.md).

## Resources, IPC, and synchronization

Kernel-managed resources are referenced through opaque handles. Message ports
carry bounded communication. Synchronization objects integrate with task
waiting. The internal organization of these services is private.

See [IPC and Synchronization](../Components/IPC-and-Synchronization.md).

## System API boundary

The kernel validates requests, checks authority and references, performs the
operation, and returns a defined result. The eight public areas are System,
Diagnostics, Text Output, Memory, Handle, Task, Synchronization, and IPC.
Internal and privileged interfaces are not public userland APIs.

See [System API](../System-API/System-API.md) and
[System Interface](../ABI/Syscall-ABI.md).

## Interrupts, exceptions, and critical failures

Hardware interrupts and processor exceptions are owned by the kernel. User-mode
faults are contained at the boundary and can terminate the offending task.

Fatal kernel failures follow a defined presentation:

- traditional Kernel Panic, or
- the Theia OS BSOD presentation.

In every fatal path the kernel ends with a safe halt. Presentation choices do
not change the severity of a critical failure.

## Diagnostics

The kernel provides bounded logging, system information, time, uptime, and
last-error reporting for the user environment. Development diagnostics are not
part of the published public interface.

## Boot role

The kernel establishes Ring 0 services, creates Init as a Ring 3 task, and relies
on Init to start Mashel. It does not skip Init. See
[Boot Architecture](Boot-Architecture.md).

## Design trade-offs

| Choice | Benefit | Cost |
| --- | --- | --- |
| Ring 0 core services | Direct and predictable performance | Kernel faults affect the whole system |
| Opaque resource references | Clear boundaries and safer access | Validation and bookkeeping on each operation |
| Bounded resources | Predictable behavior under exhaustion | Fixed limits |
| Single-processor scheduling | Simpler model | No multi-core scalability |
| BIOS boot | Direct control and virtual-machine compatibility | No UEFI in 0.18 |
| Native System API | Coherent Theia OS interface | No POSIX or Linux compatibility |

## Related documentation

- [Architecture Overview](Overview.md)
- [System Architecture](System-Architecture.md)
- [Boot Architecture](Boot-Architecture.md)
- [Memory Management](../Components/Memory-Management.md)
- [Process Management](../Components/Process-Management.md)
- [Development Status](../Development/Development-Status.md)

---

Theia OS is proprietary software. This document describes public architecture
only and does not disclose private implementation details.
