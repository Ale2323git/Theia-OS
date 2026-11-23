# Vision

Theia OS is guided by a long-term vision: an independent operating system that
is coherent from the firmware boundary to the user environment, without
inheriting another platform's assumptions. The ambition is a complete,
general-purpose operating system, not a kernel demonstration.

This is a vision document, not a delivery plan. Concrete version order belongs
in the [Roadmap](../Development/Roadmap.md), and verified capability status is
described in [Features](Features.md).

## Long-term goal

The intended platform has:

- its own kernel, boot path, and system architecture;
- a clear Ring 0/Ring 3 boundary and strong task and memory isolation;
- a centralized System API for kernel services;
- a separate application-facing API for native applications;
- a persistent filesystem and coherent storage model;
- a native application ecosystem and development facilities;
- a coherent desktop experience;
- diagnostics and documentation that make the system understandable.

These are goals. The documented 0.18 line is recorded as a foundation, not this
finished platform.

## Identity, not imitation

Windows is one source of inspiration for the platform ambition: coherent system
and application interfaces, structured resource management, native development
facilities, and a desktop experience. Theia OS is not a Windows clone,
reimplementation, or compatible system. It uses its own kernel, terminology,
interfaces, and design.

## Design philosophy

### Build from first principles

The boot path, memory model, task model, resource model, System API, and user
environment are designed together. Good ideas from other systems may inform
decisions, but they are not copied as compatibility shortcuts.

### Keep boundaries explicit

Ring 0 and Ring 3 have distinct responsibilities. Userland consumes kernel
services through the System API; private kernel details are not exposed. Ring 1
and Ring 2 remain deferred until HAL Foundation.

### Prefer bounded, predictable resources

Kernel services operate within explicit limits. Resource exhaustion is reported
through defined results instead of producing undefined behavior.

### Treat interfaces as contracts

Public behavior is stable and deliberate. Internal interfaces are not promoted
to public APIs. Critical failures never ignore their severity: the configured
presentation may change, but the system ends in a safe halt.

### Verify before claiming

Implementation, validation of a complete image, and validation of a booted
system are different levels of evidence. Documentation must name the level
actually reached.

## Version direction

The fixed roadmap order is:

1. **0.18 — System API Foundation.** Historical milestone.
2. **0.19 — Standway, codename Pequita.** Planned application-facing API.
3. **0.20 — Filesystem Foundation.** Planned persistent filesystem foundation.
4. **0.21–0.23 — Filesystem maturation.** Planned follow-on work.
5. **0.24 — Program Foundation.** Planned Loader and Executor.

No delivery dates are published, and future plans are not presented as current
features. See the [Roadmap](../Development/Roadmap.md).

## Deferred decisions

- Ring 1 and Ring 2 remain deferred until HAL Foundation.
- Mihriban remains deferred while application support does not exist.
- Theia NTRD is planned for temporary files and is not a replacement for Theia
  NTFS, the intended primary persistent filesystem.
- POSIX, Linux, Unix, and Windows compatibility are not defining goals.

## What success looks like

Success is a system that is coherent, understandable, trustworthy, extensible,
and recognizably Theia OS. It is not defined by matching another operating
system feature for feature or by claiming maturity before validation.

---

Theia OS is proprietary software. This document describes direction and does
not grant any license to the implementation.
