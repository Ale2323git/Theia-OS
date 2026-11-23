# IPC and Synchronization

Theia OS provides bounded message channels for communication between tasks and
synchronization objects for coordinating activity. Both are exposed through the
public system interface.

## Message channels

A message channel is a kernel-managed communication resource with bounded
capacity. Tasks use it to exchange defined messages and to request and return
results. A task that cannot proceed waits through the scheduler instead of
polling.

Access is based on ownership and explicit authorization. There is no global
userland namespace for these channels.

## Synchronization objects

| Object | Behavior |
| --- | --- |
| Event | Binary signaling |
| Mutex | Ownership-based mutual exclusion |
| Semaphore | Counting resource signaling |

Tasks wait on synchronization objects when they cannot continue. They become
ready when the condition changes or a timeout expires.

There is no priority inheritance. The project documents priority inversion as a
known limitation rather than claiming real-time synchronization.

## Failure behavior

Invalid references, wrong resource types, invalid flags, exhausted capacity, and
ownership failures return defined results. A failed request does not change
system state.

## Documented limits

- bounded message capacity;
- bounded object counts;
- no general userland channel namespace;
- no priority inheritance;
- shared memory is subject to explicit memory-sharing rules.

## Related documentation

- [System Architecture](../Architecture/System-Architecture.md)
- [Kernel Architecture](../Architecture/Kernel-Architecture.md)
- [System API](../System-API/System-API.md)
- [Mashel](Mashel.md)
- [Process Management](Process-Management.md)

---

Theia OS is proprietary software. This document describes public IPC and
synchronization behavior and does not disclose private implementation details.
