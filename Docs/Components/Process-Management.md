# Process Management

Theia OS uses a native **task** model rather than a POSIX process model. A task
is the unit of execution, scheduling, ownership, and resource cleanup. This
document describes the public behavior of task management and scheduling in the
documented 0.18 line.

It does not disclose private data structures, internal function names, or
implementation details.

## Task model

A task represents an independent flow of execution. Theia OS distinguishes two
task types:

- **Kernel tasks** run with kernel privileges and can perform privileged work.
- **User tasks** run in user mode with their own address space and interact with
  the kernel through the system API.

Every task has:

- an identity;
- a state;
- an owner relationship;
- a set of handles to kernel objects;
- a stack;
- optionally, a user address space;
- an exit reason when it terminates.

The current system does not provide POSIX processes, POSIX threads, fork/exec
semantics, or a Unix-style signal model. Tasks are Theia OS's own abstraction.

## Scheduling model

Theia OS uses a preemptive, round-robin scheduler for single-processor systems.

Key properties:

- runnable tasks are selected in round-robin order;
- a running task can be preempted by the system timer;
- a task can voluntarily yield;
- a blocked task does not consume processor time;
- a task can sleep for a bounded duration;
- a task can wait for another task to terminate;
- an idle task runs when no other task is ready.

The scheduler is not a real-time scheduler. Theia OS does not currently claim
hard real-time guarantees, deadline scheduling, priority inheritance, or
symmetric multiprocessing.

### Context switching

A context switch saves the state of the current task and restores the state of
the next task. For user tasks, the scheduler also ensures that the task's
address space is active before it resumes.

The kernel validates the saved user context before returning to user mode. If a
saved context is invalid, the task is terminated rather than resumed with an
inconsistent state.

## Task states

Tasks move through a small set of public states:

| State | Meaning |
| --- | --- |
| **Unused** | The task slot has no active task |
| **Ready** | The task can run but is waiting to be selected |
| **Running** | The task is currently executing |
| **Blocked** | The task is waiting for an event, object, timeout, or another task |
| **Terminated** | The task has stopped and is awaiting cleanup |

A terminated task's slot is retained until the scheduler reclaims its resources.
This keeps the task's identity resolvable while other tasks observe its
termination.

## Task identity

Public task identifiers are generation-tagged handles rather than raw slot
numbers.

- The identifier encodes a task slot and a generation.
- The generation changes when a slot is reused.
- A stale identifier cannot silently resolve to a different task.
- An invalid or stale identifier is rejected with a defined error.

This design prevents a common class of bugs in which an old task identifier is
reused after the original task has exited.

## Ownership

The scheduler records the creator of each task. Ownership governs several
operations:

- waiting for another task;
- terminating another task;
- querying another task;
- sharing memory with another task;
- directing an object to another task.

A task generally cannot operate on an unrelated task. The exact rules depend on
the operation, but the principle is consistent: authority comes from recorded
ownership and explicit authorization, not from knowing another task's
identifier.

Kernel-created tasks have a kernel owner. User tasks record the user task that
created them.

## Task creation

Tasks can be created through the system API or by the kernel.

### User task creation

When a user task creates another user task, the new task receives:

- a private address space;
- a copy of the creator's user mappings, unless the creation path specifies
  otherwise;
- a user stack;
- a handle table;
- a recorded owner relationship.

The new task starts in the **Ready** state. The creator receives the new task's
identifier.

The entry point must lie inside the user address range. The stack size must be
page-aligned and within the supported limits.

### Kernel task creation

Kernel tasks are created by kernel code. They run with kernel privileges and use
a kernel stack. The current kernel task creation path supports a fixed kernel
stack size and does not accept user-controlled flags.

### Image-backed tasks

The kernel can create a user task from a trusted flat system image. This is how
the built-in user-mode demonstration is launched. The image is copied into a
fresh user address space, a stack is allocated, and the task is queued as ready.

Image-backed task creation is a kernel-only path. It is not a general
application loader.

## Blocking and waiting

A task can block when it cannot make progress. Blocking is preferable to
polling because it releases the processor to other tasks.

Theia OS supports:

- waiting on a synchronization object;
- waiting on an IPC operation;
- sleeping for a bounded duration;
- waiting for another task to terminate;
- blocking until explicitly unblocked by kernel code.

### Wait queues

A wait queue is a scheduler-managed list of tasks waiting for a condition.
Objects such as ports, events, mutexes, and semaphores use wait queues to make
blocked tasks runnable when their state changes.

Wait queues are designed to avoid lost wakeups when a condition changes between
the moment a task checks it and the moment the task blocks.

### Timeouts

Timeouts are managed by the scheduler. When a timeout expires, the task becomes
ready with a timeout result. Timeout handling is designed to remain correct over
long-running system operation.

### Completion waits

Each task has a completion wait queue. A parent can wait for a child to
terminate. When the child exits, the completion queue is advanced and waiting
parents are made runnable.

## Yield and sleep

- **Yield** gives other ready tasks a chance to run without blocking the
  caller.
- **Sleep** blocks the caller for a bounded duration. A zero-duration sleep does
  not block.

The public system API currently exposes a deliberately limited form of sleep and
task waiting to user mode. Kernel services use the full blocking mechanism
internally. This is a current interface limitation, not a limitation of the
scheduler's wait implementation.

## Termination and cleanup

Termination is orderly and synchronous. When a task terminates, the scheduler:

1. removes the task from any wait queue;
2. marks it terminated and records the exit reason;
3. clears its entry point, argument, and user context;
4. wakes tasks waiting for its completion;
5. notifies IPC and synchronization services so their resources can be
   released or transferred;
6. releases the task's diagnostic log quota;
7. destroys the task's handle table, releasing its object references;
8. retains the slot until the next scheduling pass reclaims it.

The order matters. Mutex ownership and semaphore units must be able to reach
waiters before the terminating task's handle table is destroyed. IPC calls must
be cancelled or completed. The cleanup path is designed to leave no dangling
references.

### Exit reasons

A task can terminate for reasons including:

- normal exit;
- explicit termination by an authorized task;
- a user-mode exception.

The exit reason is recorded as part of the task's state.

## Address-space activation

Before a user task runs, the scheduler activates that task's address space and
updates the kernel's privilege-transition state.

For a user task, the scheduler verifies that the active address space matches the
task's own address space before saving a user context. A mismatch stops the
transition instead of allowing a task to resume with another task's mappings.

This check is an important part of the isolation model: a context switch must
not silently mix one task's execution state with another task's memory.

## User-mode exceptions

A fault in user mode is handled at the user/kernel boundary.

- The kernel classifies the exception.
- The offending task can be terminated.
- The kernel records diagnostic information.
- The rest of the system remains responsible for cleaning up the task's
  resources.

A user-mode fault does not grant the task kernel privileges and does not
automatically crash the kernel.

## Scheduler statistics

The scheduler maintains public statistics:

- task counts by state;
- the current task;
- the number of context switches;
- the timer tick count.

These statistics are available through the system information interface. They
are diagnostic information, not a scheduling-control interface.

## Current limits and restrictions

The documented 0.18 task model has these limits:

- a fixed maximum number of task slots;
- one reserved kernel context;
- one reserved idle task;
- a fixed number of ordinary task slots;
- a fixed kernel stack size;
- a bounded user stack size;
- a fixed maximum number of user pages per address space;
- a fixed maximum number of address spaces;
- a bounded wait-queue capacity;
- single-processor scheduling only.

The public system API also limits user-mode sleep and task-wait forms. The
scheduler's internal blocking mechanism is more general than the current
user-mode interface.

## Relationship to memory management

Task management and memory management are tightly coupled:

- each user task can have its own address space;
- task creation can clone the creator's user mappings;
- the scheduler activates the correct address space before a user task runs;
- task exit releases the task's memory and handles;
- memory sharing is restricted to directly owned tasks.

The memory side of this relationship is described in
[Memory Management](Memory-Management.md).

## Validation status

The task scheduler and task-related system calls are implemented and covered by
host-side test suites. The user-mode demonstration exercises task creation and
several system calls in a running system.

The current public claims are:

- **Implemented:** task creation, scheduling, blocking, waiting, termination,
  address-space activation, ownership, and statistics.
- **Host-tested:** scheduler capacity, task lifecycle, and related system calls
  are exercised by host tests.
- **Boot-validated with limits:** earlier sessions recorded successful boots and
  demonstration runs. The latest documentation pass did not reproduce a QEMU or
  physical-hardware boot.
- **Not claimed:** real-time scheduling, SMP, POSIX processes, or broad
  physical-hardware coverage.

See [Development Status](../Development/Development-Status.md) for the full
evidence matrix.

## Related documentation

- [Architecture Overview](../Architecture/Overview.md)
- [Kernel Architecture](../Architecture/Kernel-Architecture.md)
- [Memory Management](Memory-Management.md)
- [Features](../Introduction/Features.md)
- [Glossary](../Resources/Glossary.md)

---

Theia OS is proprietary software. This document describes public process
management behavior and does not disclose private implementation details.
