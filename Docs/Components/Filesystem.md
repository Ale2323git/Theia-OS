# Filesystem

## Documented status

The documented 0.18 line has **no persistent filesystem** and no file-backed
memory. The bootable image is a complete system image, not a general-purpose
filesystem volume. No filesystem behavior should be inferred from the published
system.

## Planned architecture

| Component | Role | Status |
| --- | --- | --- |
| Theia NTFS | Intended primary persistent filesystem for Theia OS | Planned |
| Theia NTRD | Intended temporary filesystem for transient files | Planned |
| Filesystem Foundation | 0.20 milestone | Planned |
| Filesystem maturation | 0.21–0.23 milestones | Planned |

Theia NTFS is the intended primary persistent filesystem. Theia NTRD is planned
for temporary files and is **not** a replacement for Theia NTFS. These are
roadmap decisions, not implemented capabilities.

## What this means in 0.18

- no persistent user files;
- no file-backed virtual memory;
- no demand paging or swap;
- no installer, package manager, or application data model;
- no published filesystem services;
- no delivery dates for planned filesystem work.

## Verification status

The absence of a filesystem is consistent with the published 0.18 image and its
validation. The planned 0.20–0.23 work is not verified as implemented in the
documented 0.18 line.

## Related documentation

- [Roadmap](../Development/Roadmap.md)
- [Features](../Introduction/Features.md)
- [Architecture Overview](../Architecture/Overview.md)
- [Memory Management](Memory-Management.md)

---

Theia OS is proprietary software. This document describes public filesystem
status and planned direction; it does not disclose private implementation
details.
