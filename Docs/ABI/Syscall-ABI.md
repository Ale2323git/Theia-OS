# System Interface

This page gives a public overview of how userland communicates with the Theia OS
kernel. It does not publish low-level contracts, numeric identifiers, register
usage, or internal mechanisms.

## Public overview

Theia OS defines a system interface for Ring 3 components. Authorized userland
requests kernel services through the System API. The kernel validates the
request, performs the service, and returns a defined result.

The System API is the documented way to consume system services from userland.
Its eight areas are System, Diagnostics, Text Output, Memory, Handle, Task,
Synchronization, and IPC. See [System API](../System-API/System-API.md).

## What is not published

- low-level binary contracts;
- numeric service identifiers;
- register-level conventions;
- privileged dispatch mechanisms;
- internal protocol details;
- implementation of kernel services.

These details are private and are not required to understand the product,
its architecture, or its capabilities.

## Product-level boundary

Ring 0 contains the kernel and privileged services. Ring 3 contains Init, Mashel,
and user tasks. Userland consumes public system services and does not access
private kernel structures. Ring 1 and Ring 2 are deferred until HAL Foundation
and are not public userland facilities.

## Related documentation

- [System API](../System-API/System-API.md)
- [Kernel Architecture](../Architecture/Kernel-Architecture.md)
- [Architecture Overview](../Architecture/Overview.md)
- [Development Status](../Development/Development-Status.md)

---

Theia OS is proprietary software. This document describes public interface
concepts only and does not disclose private implementation details.
