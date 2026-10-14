# Theia OS Memory Management

This document describes the memory-management foundation of Theia OS 0.15, including physical memory discovery, memory-region tracking, and allocator development milestones.

## 1. Overview

Memory management is a foundational part of Theia OS.

The kernel needs to identify available physical memory, distinguish it from reserved regions, and manage memory usage without overwriting critical system components.

Theia OS develops these capabilities incrementally. The presence of a milestone in the development history does not imply that every advanced memory-management feature is implemented.

## 2. Development History

### Version 0.8 — Memory Foundation

Version 0.8 established the initial memory-management foundation.

This milestone provided the basis for further work on memory discovery and management.

### Version 0.9 — Memory Region Manager

Version 0.9 introduced the Memory Region Manager milestone.

The memory-region management work builds upon information about the physical memory layout and helps distinguish different types of memory regions.

### Versions 0.10–0.11 — Memory Allocator Foundation

The development history records memory allocator foundation work in October 2026.

This stage builds upon the earlier memory foundation and memory-region management work.

The exact allocator capabilities should be documented according to the implementation included in the tested build.

## 3. Physical Memory Discovery

The boot process uses the BIOS E820 interface to obtain a map of physical memory.

The memory map describes regions reported by the firmware, including regions that may be available and regions reserved for other purposes.

The kernel must not assume that all detected physical memory is safe to allocate.

### Memory Map Buffer

The documented development configuration uses the following buffer:

| Property | Value |
|---|---|
| Physical address | `0x8D000` |
| Maximum entries | 32 |

These values describe the current documented configuration and must be verified against the actual build.

## 4. Memory Region Types

The implementation recognizes the following E820 memory type values:

| Type | Meaning |
|---:|---|
| `1` | Available memory |
| `2` | Reserved memory |
| `3` | ACPI reclaimable memory |
| `4` | ACPI NVS memory |
| `5` | Bad memory |

These types describe firmware-reported memory regions. They do not, by themselves, determine whether a region can be allocated immediately.

In particular, memory occupied by the bootloader, kernel, buffers, or other active components must be protected from accidental reuse.

## 5. Reserved Memory

Reserved-memory tracking helps protect physical memory ranges required by the system.

Examples of relevant areas in the documented configuration include:

| Component or structure | Address |
|---|---:|
| Stage 1 | `0x7C00` |
| Stage 2 | `0x8000` |
| Kernel | `0x9000` |
| Init | `0x11000` |
| Theia Console | `0x12000` |
| IDT | `0x8C000` |
| E820 memory map | `0x8D000` |
| Kernel interface area | `0x8E000` |
| Keyboard buffer | `0x8F000` |
| VGA text memory | `0xB8000` |

These addresses are specific to the documented development layout. Actual reserved ranges must account for each component's size and any overlapping or shared memory requirements.

## 6. Memory Allocation

A memory allocator provides a way to manage memory requests made by the kernel and other eligible components.

The allocator foundation is part of the project's development history.

Its safety depends on several requirements:

- Avoiding reserved physical ranges.
- Respecting the boundaries of usable memory.
- Maintaining consistent allocation metadata.
- Handling allocation failures.
- Preventing invalid or overlapping allocations.

This document does not claim support for a particular allocation strategy unless it has been verified in the implementation.

## 7. Memory Safety Considerations

Memory-management code must account for the fact that low-level operating-system components operate directly on physical addresses and memory layouts.

Important considerations include:

- Never treating every E820 entry as allocatable memory.
- Protecting memory occupied by active system components.
- Validating region sizes and address arithmetic.
- Avoiding buffer overflow when processing memory-map entries.
- Keeping allocation metadata consistent.
- Handling missing, invalid, or truncated memory-map information.

These are engineering requirements for reliable memory management, not a claim that every possible validation mechanism is already implemented.

## 8. Current Scope

This document covers the memory-management foundation of Theia OS 0.15.

It does not claim the existence of advanced features such as virtual-memory paging, a complete process memory model, or a fully isolated user-space allocator unless these are implemented and verified separately.

## 9. Testing

Memory-management testing should verify:

- Correct interpretation of E820 entries.
- Handling of different memory-region types.
- Protection of reserved memory ranges.
- Correct behavior at memory-region boundaries.
- Allocator behavior under successful and unsuccessful requests.
- Preservation of kernel and component memory.
- Consistent behavior across repeated tests.

Testing results should identify the exact build and virtual-machine configuration used.

---

**Document version:** Theia OS 0.15  
**Component:** Memory Management  
**Status:** Technical documentation
