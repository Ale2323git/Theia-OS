# Glossary

This glossary defines terminology used in the Theia OS public documentation.
Terms are listed alphabetically.

## B

**BIOS**
Firmware interface used by the documented Theia OS boot path. UEFI is not
implemented in 0.18.

**Boot path**
Firmware → native Theia boot path → kernel initialization → mandatory boot
coordinator → Init (Ring 3) → Mashel (Ring 3).

**Bootloader**
The native Theia boot component. Theia OS does not use GRUB or another
operating system's loader.

## C

**Critical failure**
A fatal kernel or boot failure. Presentation may be Kernel Panic or the Theia OS
BSOD, but a safe halt is always required.

## E

**Event**
Synchronization object with binary signaling.

**Exception**
Processor-detected fault. A Ring 3 exception can terminate the offending task
without granting kernel privileges.

## F

**Filesystem**
A persistent data organization component. No filesystem is implemented in 0.18;
Theia NTFS and Theia NTRD are planned.

**Firmware**
Low-level platform software that starts the system. The documented platform
uses BIOS.

## H

**HAL Foundation**
Deferred architectural foundation that gates general Ring 1/Ring 2 availability.
It is not implemented in the documented 0.18 line.

**Handle**
Opaque reference to a kernel-managed resource. It is not a pointer and is not
valid in another owner's context without explicit transfer.

**Hybrid kernel**
A kernel combining monolithic performance characteristics with
microkernel-inspired resource and communication boundaries. Theia NT is a
hybrid kernel.

## I

**Init**
The first Ring 3 system component. It is created during the boot handoff and
starts Mashel through public system interfaces.

**initland**
The documented name of the mandatory boot coordinator role. It is established
by kernel initialization and is not a userland component.

**IPC (Inter-process communication)**
Communication between tasks. Theia OS uses bounded message channels.

## K

**Kernel**
Privileged Ring 0 core controlling processor state, interrupts, memory, tasks,
resources, communication, synchronization, and diagnostics.

**Kernel mode**
Privileged execution mode in Ring 0.

**Kernel Panic**
Traditional fatal-error presentation. It reports a fatal failure before a safe
halt.

**Kernel object**
Kernel-managed resource such as a message channel or synchronization object.

## M

**Mashel**
The official Theia OS interactive console component. It runs in Ring 3, uses
public system services, and presents the `theia>` prompt.

**Memory map**
Boot-time description of usable and reserved physical memory regions.

**Mihriban**
Deferred component. It remains deferred while application support does not
exist.

**Mutex**
Synchronization object providing ownership-based mutual exclusion. There is no
priority inheritance.

## P

**Page**
Fixed-size unit of physical and virtual memory.

**Paging**
Mapping virtual pages to physical pages with protection.

**Port**
Kernel-managed message channel for bounded IPC.

**Preemption**
Scheduler ability to interrupt a running task and switch to another.

**Process**
Traditional OS abstraction not used as Theia's native model. Theia OS uses
Tasks.

**Program Foundation**
Planned 0.24 milestone introducing Loader and Executor support.

## R

**Ring 0**
Privileged kernel execution level.

**Ring 1 / Ring 2**
Reserved privilege levels. General availability is deferred until HAL
Foundation; they are not public userland facilities.

**Ring 3**
Restricted userland level used by Init, Mashel, and user tasks.

## S

**Scheduler**
Kernel component that selects Tasks and manages their execution state.

**Semaphore**
Counting synchronization object.

**Standway**
Planned application-facing API for 0.19, codename Pequita. Not implemented in
0.18.

**System API**
Authorized userland interface to kernel services. It has eight areas: System,
Diagnostics, Text Output, Memory, Handle, Task, Synchronization, and IPC.

**System image**
Separately loaded system component such as Init or Mashel.

## T

**Task**
Theia OS's unit of execution and scheduling. The system schedules Tasks, not
executables.

**Theia Console**
Historical name for the interactive component. The current official name is
Mashel.

**Theia NT Kernel**
Theia OS's native hybrid kernel. The name does not imply Windows NT
compatibility.

**Theia NTRD**
Planned temporary filesystem. It is not a replacement for Theia NTFS and is not
implemented in 0.18.

**Theia NTFS**
Planned primary persistent filesystem for Theia OS. It is not implemented in
0.18.

**Text Output**
System API area providing controlled text output. Mashel uses it instead of
direct hardware access.

## U

**User memory**
Memory regions owned by a task, with permissions and controlled sharing.

**User mode**
Restricted Ring 3 execution mode.

**Userland**
The Ring 3 part of the system, including Init, Mashel, and user tasks.

## Related documentation

- [What Is Theia OS?](../Introduction/What-Is-Theia-OS.md)
- [System API](../System-API/System-API.md)
- [System Interface](../ABI/Syscall-ABI.md)
- [Mashel](../Components/Mashel.md)
- [Development Status](../Development/Development-Status.md)

---

Theia OS is proprietary software. This glossary defines public terminology only
and does not disclose private implementation details.
