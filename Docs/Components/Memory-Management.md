# Memory Management

Memory management is one of the foundations of Theia OS. It provides physical
page management, virtual memory, per-task address spaces, memory permissions,
and controlled sharing. It also protects the kernel from untrusted user
pointers.

This document describes the public behavior of the memory subsystem in the
documented 0.18 line. It does not disclose private data structures, memory maps,
internal addresses, or implementation details.

## Design goals

The memory subsystem is designed around five goals:

1. **Isolation.** User tasks should not be able to read or modify memory that
   belongs to the kernel or to another task without authorization.
2. **Explicit ownership.** Memory should have a clear owner and a clear
   lifetime.
3. **Controlled sharing.** Sharing should be deliberate, trackable, and
   reversible.
4. **Validation at the boundary.** The kernel should validate every user buffer
   before accessing it.
5. **Bounded behavior.** Memory operations should fail predictably when
   resources are exhausted rather than corrupting system state.

These goals shape both the memory model and the system API that exposes it.

## Memory layers

The memory subsystem is organized in layers:

| Layer | Responsibility |
| --- | --- |
| Firmware memory discovery | Obtain the platform's memory map and distinguish usable from reserved regions |
| Physical memory management | Track physical pages and allocate or release them |
| Virtual memory | Map virtual addresses to physical pages with protection |
| User address spaces | Give user tasks isolated views of memory |
| User memory regions | Track ownership, permissions, and sharing of user mappings |
| User-buffer safety | Validate and copy data between user and kernel memory |

Each layer depends on the guarantees of the layer below it. Virtual memory
cannot operate without physical pages, and user address spaces cannot provide
isolation without virtual memory.

## Firmware memory discovery

The boot path asks the platform firmware for the system memory map. The kernel
then:

- identifies usable memory;
- identifies reserved memory that must not be allocated;
- normalizes overlapping or conflicting regions;
- rejects a map that is inconsistent or too large to handle safely.

The resulting map is used to initialize physical memory management and to
answer system memory queries.

A memory map is a snapshot of the platform at boot time. The system's public
memory-map query reports that boot-time view; it is not a live map of every page
frame in the system.

## Physical memory management

Physical memory is managed at page granularity. Each page can be in one of three
public states:

- **Free:** available for allocation.
- **Used:** currently allocated.
- **Reserved:** never available for general allocation.

The physical memory manager:

- classifies pages based on the firmware map;
- reserves the memory used by the boot path, kernel, system images, processor
  structures, and other critical regions;
- allocates a free page when a component requests one;
- releases a page back to the free set when it is no longer needed;
- maintains consistent counts of total, free, used, and reserved pages.

A page is not allocated twice, and a reserved page is never handed out as
general-purpose memory. The initializer verifies that its accounting is
consistent before the system continues.

## Virtual memory

Theia OS uses paging to provide virtual-to-physical translation and memory
protection. Virtual memory allows the kernel to:

- give each user task its own view of memory;
- map the same physical page at different virtual addresses when appropriate;
- mark pages as readable, writable, or supervisor-only;
- isolate kernel memory from user mode;
- reclaim mappings when a task exits.

The kernel's own mappings are supervisor-only. User mappings are placed in a
dedicated user address range and are subject to the permission rules of the
address space that owns them.

The system does not currently provide demand paging from a file system, a swap
subsystem, or a general file-backed virtual memory model. Memory is mapped
explicitly and backed by physical pages.

## User address spaces

A user task can have its own address space. An address space contains:

- the task's user mappings;
- a copy of the kernel's supervisor mappings needed to service the task;
- page tables that translate the task's virtual addresses;
- reference information for shared physical pages.

Address spaces provide the main isolation boundary between user tasks. A task
cannot see another task's private mappings unless the kernel explicitly shares
them.

The current implementation supports a bounded number of address spaces and a
bounded number of pages per address space. These limits are part of the
system's fixed-capacity resource model.

### Address-space lifecycle

At a high level:

1. A new address space is created with the kernel mappings it needs.
2. User pages are allocated and mapped into the space.
3. The scheduler activates the space when the task runs.
4. The task can map, unmap, protect, query, and share pages through the system
   API.
5. When the task exits, its mappings and owned pages are released through the
   cleanup path.
6. The address space is destroyed only after its active pages have been
   released.

The kernel does not destroy an address space while it still has active user
pages. Cleanup is ordered so that page references are released before the
address-space structures are removed.

## User memory regions

User memory regions provide a higher-level view of user mappings. A region
represents a contiguous range of user pages with:

- a base address;
- a size;
- read and write permissions;
- an ownership record;
- a mapping type that distinguishes private pages from shared pages.

Regions make it possible for the kernel to reason about user memory as owned
ranges rather than as isolated pages. They also support operations such as
protecting a range, freeing a region, and reporting memory information.

### Private and shared mappings

A user mapping can be:

- **Private:** the page belongs to one address space.
- **Shared:** the page is referenced by more than one address space.

Sharing is reference-tracked. A shared page is not released until the last
reference is gone. This prevents a page from being freed while another task can
still access it.

Sharing is not automatic. It requires an explicit operation and is subject to
ownership and permission checks.

## Memory permissions

User memory supports read and write permissions. Read access is required for a
user page to be usable by user mode. Write access is optional and can be changed
through the protection operation.

The permission model is intentionally simple. The current system does not
expose execute-only or no-access user mappings through its public memory API.
The kernel's own mappings remain supervisor-only.

## Memory system calls

The public system API provides memory operations in these categories:

| Operation | Purpose |
| --- | --- |
| Allocate | Reserve a user memory region, optionally at a preferred address |
| Free | Release a user memory region |
| Map | Map pages into the caller's address space |
| Unmap | Remove mappings from the caller's address space |
| Protect | Change the permissions of a user memory range |
| Query | Retrieve information about a user page or region |
| Share | Make a page available to a directly owned target task |
| Unshare | Drop a shared reference without destroying a private page |

Memory operations validate:

- page alignment;
- nonzero and non-overflowing sizes;
- that the range lies inside the user address range;
- that the range does not exceed the per-operation limit;
- that the caller owns the target task or address space where required;
- that the requested permissions are valid for the operation.

Invalid requests return a defined error and do not change kernel state.

### Sharing rules

Memory sharing requires:

- a different, directly owned user task as the target;
- page-aligned addresses inside the user range;
- write access only when the source page is writable;
- a destination that is not already mapped or reserved.

Unsharing requires that the page has more than one reference. Unsharing the last
reference would destroy a private page, so it is rejected. This keeps the
ownership model explicit: a task cannot use unshare to delete another task's
private memory.

## User-buffer safety

The kernel never trusts a user pointer.

Before reading from or writing to a user buffer, the kernel:

1. checks that the buffer lies inside the active user address space;
2. checks that the requested range does not overflow;
3. checks that the memory is mapped;
4. checks that the required read or write permission is present;
5. copies data through a controlled path rather than dereferencing the user
   pointer directly.

If validation fails, the operation returns an error and the kernel does not
touch the user buffer. This rule applies to system calls that accept output
buffers, input buffers, or both.

The same model protects the kernel from malformed pointers, integer overflow in
size calculations, and attempts to access kernel or unmapped memory through a
user-supplied address.

## Memory failures

Memory operations can fail for defined reasons, including:

- invalid parameters;
- invalid or unmapped addresses;
- address overflow;
- access denial;
- an address that is already mapped;
- an address that is not mapped;
- insufficient memory;
- insufficient resources;
- a region that does not exist;
- a system that is not ready.

Failures are reported through the system API's status model. The kernel does not
return a success status for a partially completed operation without accounting
for what changed.

For operations that change multiple pages or multiple records, the kernel is
expected to validate the whole request before making changes and to preserve
consistent state if a later step fails.

## Current limits

The documented 0.18 memory subsystem has these public limits:

- page-granular allocation;
- a fixed maximum number of address spaces;
- a fixed maximum number of pages per address space;
- a fixed maximum number of registered user regions per address space;
- a fixed maximum size for a single memory operation;
- no demand paging from storage;
- no swap;
- no general file-backed memory mapping;
- no execute-only user permission through the public memory API.

These limits are part of the current architecture. They are not presented as
permanent, but they are also not scheduled features.

## Relationship to process management

Memory management and process management are tightly connected:

- each user task can have its own address space;
- the scheduler activates the correct address space before a user task runs;
- task creation can clone the creator's user mappings;
- task exit releases the task's memory and handles;
- memory sharing is restricted to directly owned tasks.

The task side of this relationship is described in
[Process Management](Process-Management.md).

## Validation status

The memory subsystem is implemented and covered by host-side test suites. The
user-mode demonstration exercises memory queries and system information as part
of a real system-call sequence.

The current public claims are:

- **Implemented:** physical memory management, virtual memory, user address
  spaces, region management, permissions, sharing, and user-buffer validation.
- **Host-tested:** the memory-related system API and supporting logic are
  exercised by host tests.
- **Boot-validated with limits:** earlier sessions recorded successful boots and
  demonstration runs. The latest documentation pass did not reproduce a QEMU or
  physical-hardware boot.
- **Not claimed:** broad physical-hardware coverage, file-backed paging, swap,
  or a complete virtual memory manager for general-purpose workloads.

See [Development Status](../Development/Development-Status.md) for the full
evidence matrix.

## Related documentation

- [Architecture Overview](../Architecture/Overview.md)
- [Kernel Architecture](../Architecture/Kernel-Architecture.md)
- [Process Management](Process-Management.md)
- [Features](../Introduction/Features.md)
- [Glossary](../Resources/Glossary.md)

---

Theia OS is proprietary software. This document describes public memory
management behavior and does not disclose private implementation details.
