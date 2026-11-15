# Glossary

This glossary defines the terminology used throughout the Theia OS public
documentation. Terms are listed alphabetically.

## A

**ABI (Application Binary Interface)**
The binary-level contract between system components and user-mode code. It
defines how services are requested and how results are reported. The detailed
numeric identifiers and layouts of Theia OS's system ABI are part of the
proprietary interface specification and are not published here.

**Address space**
The set of virtual addresses available to a task, together with the mappings and
permissions that translate those addresses to physical memory. A user task can
have its own address space, which isolates it from other tasks.

**Application-facing API**
The interface intended for native applications, distinct from the kernel-service
boundary. In Theia OS this role is planned under the name Standway; it is not
implemented in the 0.18 line.

**Architecture**
The overall structure of the system: its layers, components, boundaries, and
rules for how they interact. Theia OS has its own kernel, system, and boot
architecture.

## B

**BIOS**
Basic Input/Output System. The firmware interface used by the documented Theia
OS boot path. The current platform boots through BIOS rather than UEFI.

**Boot path**
The sequence that starts the system: firmware, Theia's first-stage loader,
second-stage loader, kernel entry, kernel initialization, Init, and Console.

**Bootloader**
The code that loads the kernel and system images. Theia OS uses its own
two-stage loader and does not use GRUB or another operating system's loader.

## C

**Console**
The interactive Theia OS system image. It provides a VGA text interface,
keyboard input, built-in commands, and the ability to launch the user-mode
demonstration.

**Context switch**
The act of saving one task's execution state and restoring another's so that the
scheduler can run a different task.

## D

**Deferred destruction**
A controlled object-cleanup model in which an object is retired after its last
reference is released, rather than being freed immediately while other
components may still be observing it.

**Desktop environment**
A coherent graphical user environment. Theia OS currently provides a VGA text
console; a desktop experience is a long-term goal, not a current feature.

**Driver**
Software that controls a hardware device. Theia OS has limited device support in
the documented 0.18 line and does not yet provide a general third-party driver
model.

## E

**Event**
A kernel synchronization object with a binary state, used to signal one or more
waiting tasks. Theia OS supports manual-reset and auto-reset events.

**Exception**
A processor-detected fault or exceptional condition. A user-mode exception can
terminate the offending task without giving it kernel privileges.

## F

**File system**
The system component that organizes and stores data persistently. Theia OS does
not include a file system in 0.18; a dedicated persistent filesystem is planned
as a long-term component.

**Firmware**
Low-level platform software that starts the system and provides early hardware
services. The documented Theia OS platform uses BIOS firmware.

## G

**General-purpose operating system**
An operating system intended for broad everyday use rather than a single task or
demonstration. This is Theia OS's long-term ambition, not a description of the
current 0.18 system.

## H

**Handle**
An opaque, per-task reference to a kernel object. A handle is not a pointer and
cannot be used by another task without an explicit sharing mechanism.

**Handle table**
The per-task structure that stores a task's handles. Handles are private to the
task that owns the table.

**Hardware validation**
Testing a behavior on physical hardware. It is distinct from host-side testing
and from booting in a virtual machine.

**Host test**
A test that runs on the development host rather than inside a real Theia OS
boot. Host tests can exercise logic and interfaces but do not replace hardware
validation.

**Hybrid kernel**
A kernel design that combines characteristics of monolithic and microkernel
systems. Theia NT keeps core services in kernel mode while using
microkernel-inspired object, handle, and message boundaries.

## I

**Init**
The first Theia OS system image after the kernel. In the documented 0.18 line,
Init performs initial user-mode preparation and hands control to Console.

**Inter-process communication (IPC)**
Communication between tasks. Theia OS provides bounded message ports for IPC.

**Isolation**
The separation of tasks and privilege levels so that one component cannot
arbitrarily access another's memory or privileges. Theia OS enforces isolation
through privilege levels, address spaces, handles, and validation.

## K

**Kernel**
The privileged core of the operating system. It controls the processor,
interrupts, memory, scheduling, objects, IPC, synchronization, and the system
API boundary.

**Kernel mode**
The privileged execution mode in which the kernel runs. Kernel-mode code can
access hardware and kernel memory.

**Kernel object**
A kernel-managed resource such as a port, event, mutex, or semaphore. User-mode
code refers to kernel objects through handles.

## M

**Memory map**
The platform's description of which physical memory regions are usable and which
are reserved. The boot path obtains it from firmware.

**Microkernel**
A kernel design that keeps a small privileged core and moves services into
user-mode components. Theia OS is not a pure microkernel; it is inspired by
microkernel security principles while retaining a hybrid design.

**Monolithic kernel**
A kernel design in which core services run in kernel mode. Theia NT has
monolithic performance characteristics because its core services run in the
kernel.

**Mutex**
A synchronization object that provides ownership-based mutual exclusion. A mutex
can be locked by one task at a time and can transfer ownership under defined
rules.

## P

**Page**
A fixed-size unit of physical and virtual memory. Theia OS manages memory at
page granularity.

**Paging**
The mechanism that maps virtual pages to physical pages and enforces memory
permissions.

**Port**
A kernel object used for bounded message-based IPC. Ports support send,
receive, call, and reply operations.

**Preemption**
The scheduler's ability to interrupt a running task and switch to another task
without the first task's cooperation.

**Process**
A traditional operating-system abstraction that combines an address space,
execution state, and resources. Theia OS uses **tasks** rather than POSIX
processes. "Process management" in this documentation refers to the task model.

**Proprietary**
Software whose source and implementation are privately owned. Theia OS is
proprietary; no open-source license is granted.

## Q

**QEMU**
A machine emulator and virtualizer used as a validation environment for Theia
OS disk images. A QEMU boot is not the same as physical-hardware validation.

## R

**Round-robin scheduling**
A scheduling policy that selects runnable tasks in rotation. Theia OS uses
preemptive round-robin scheduling for single-processor systems.

## S

**Scheduler**
The kernel component that selects which task runs, performs context switches,
and manages blocking, timeouts, and termination.

**Semaphore**
A counting synchronization object used to signal the availability of a bounded
resource.

**SMP (symmetric multiprocessing)**
The use of multiple processors in one system. The documented Theia OS line is
single-processor and does not implement SMP.

**Standway**
The planned application-facing API for Theia OS. It is intended to sit above the
System API and serve native applications. Standway is not implemented in 0.18,
and no interface details or delivery date are published.

**System API**
The interface through which user-mode code requests kernel services. In 0.18 it
covers tasks, memory, handles, IPC, synchronization, and system services. It is a
foundation, not a complete final API.

**System image**
A separately loaded component of the running system, such as Init or Console.

## T

**Task**
Theia OS's native unit of execution. A task has an identity, state, ownership,
stack, handles, and optionally a user address space. Tasks can be kernel tasks or
user tasks.

**Task state**
The current lifecycle position of a task: unused, ready, running, blocked, or
terminated.

**Theia NT Kernel**
The name of Theia OS's custom kernel. It is a hybrid kernel with monolithic
performance characteristics and microkernel-inspired security boundaries. The
name does not imply Windows NT compatibility.

**Theia OS**
The independent, proprietary operating system documented by this repository.

## U

**UEFI**
Unified Extensible Firmware Interface. A modern firmware interface. The
documented Theia OS boot path uses BIOS and does not provide UEFI boot.

**User mode**
The restricted execution mode in which user tasks run. User-mode code cannot
directly access kernel memory or privileged processor operations.

**Userland**
The user-mode part of the system: user tasks, the console environment, and the
system API clients that run outside the kernel.

## V

**Validation**
Evidence that a behavior works in a particular environment. Theia OS
distinguishes design, implementation, compilation, host testing, BIOS/QEMU
validation, and physical-hardware validation.

**VGA text console**
The text-mode display environment used by the Theia OS console. It is not a
graphical desktop.

**Virtual memory**
The abstraction that gives tasks a virtual address space and maps it to physical
memory with protection. Theia OS uses paging for virtual memory.

## W

**Wait queue**
A scheduler-managed list of tasks waiting for a condition. Objects use wait
queues to wake blocked tasks when their state changes.

## Related documentation

- [What Is Theia OS?](../Introduction/What-Is-Theia-OS.md)
- [Architecture Overview](../Architecture/Overview.md)
- [Features](../Introduction/Features.md)
- [Development Status](../Development/Development-Status.md)

---

Theia OS is proprietary software. This glossary defines public terminology only
and does not disclose private implementation details.
