# Process Management

Theia OS uses a native **Task** model, not a POSIX process or thread model. The
system schedules Tasks, not executables. This document describes the public task
behavior in 0.18.

## Task model

A task is the unit of execution, ownership, scheduling, and cleanup.

- **Kernel tasks** run in Ring 0.
- **User tasks** run in Ring 3 with isolated address spaces and use the System
  API.

Every task has an identity, a state, an owner, and a lifecycle. There is no POSIX
process model, fork/exec, or Unix signal model.

## Scheduling

The scheduler is preemptive and schedules Tasks. Ready tasks are selected to run,
a running task can be preempted, and waiting tasks do not consume processor
time. An idle task runs when no other task is ready.

There is no real-time scheduling, priority inheritance, or SMP.

## Task states

| State | Meaning |
| --- | --- |
| **Ready** | Runnable but not currently executing |
| **Running** | Currently executing |
| **Waiting** | Blocked on a condition, timeout, or activity |
| **Terminated** | Stopped and cleaned up |

## Ownership

Ownership governs waiting, termination, querying, and sharing. Knowing a task
identity is not authority; authority comes from recorded ownership and explicit
authorization.

## Task creation

The kernel creates Init as a Ring 3 component during the boot handoff. Init then
creates Mashel through public system interfaces.

User task creation provides an isolated address space and a task lifecycle. It
is not a general application loader.

## Waiting and termination

Tasks can wait for conditions, timeouts, or activity completion. When a task
terminates, the system resolves its waits, notifies interested components,
releases its resources, and completes cleanup through defined paths.

A Ring 3 fault is handled at the user/kernel boundary and can terminate the
offending task without granting kernel privileges.

## Documented limits

- fixed task capacity;
- bounded stacks and resources;
- single-processor scheduling only;
- limited user-facing wait and sleep forms;
- no general application loader.

## Status

Implemented as part of the 0.18 foundation. The published image was validated in
the reference virtual environment, including the Ring 3 Init-to-Mashel path.
Physical hardware and long-duration load behavior remain limited. See
[Development Status](../Development/Development-Status.md).

## Related documentation

- [Architecture Overview](../Architecture/Overview.md)
- [Kernel Architecture](../Architecture/Kernel-Architecture.md)
- [Memory Management](Memory-Management.md)
- [System API](../System-API/System-API.md)
- [Boot Architecture](../Architecture/Boot-Architecture.md)

---

Theia OS is proprietary software. This document describes public task behavior
and does not disclose private implementation details.
