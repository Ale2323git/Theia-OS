# Roadmap

This roadmap records the public version order and planning position. It
separates the documented 0.18 foundation from planned versions and deferred
decisions. No delivery dates are published.

## Planning principles

1. Finish the foundation before expanding it.
2. Validate before claiming.
3. Protect the kernel, memory, task, and security model.
4. Keep public contracts stable.
5. Do not invent compatibility with another operating system.
6. Publish only approved scope.
7. Separate direction from capability.

## Fixed version order

| Version | Theme | Status |
| --- | --- | --- |
| 0.18 | System API Foundation | Historical milestone |
| 0.19 | Standway, codename Pequita | Planned |
| 0.20 | Filesystem Foundation | Planned |
| 0.21–0.23 | Filesystem maturation | Planned |
| 0.24 | Program Foundation: Loader and Executor | Planned |

This order is a project decision and is not changed here. Planned versions are
not implemented capabilities and have no published dates.

## 0.18 — recorded foundation

The 0.18 line established:

- native BIOS boot with a coordinator role documented as `initland`;
- Ring 3 Init and Ring 3 Mashel;
- Theia NT Kernel, memory management, Task scheduling, and task lifecycle;
- resources, IPC, synchronization, and diagnostics;
- eight System API areas;
- a published image verified in the reference virtual environment.

See [Features](../Introduction/Features.md) and
[Version History](Version-History.md).

## 0.19 — Standway, Pequita

Standway is planned as the application-facing API above the System API. Its
codename is **Pequita**. It is not implemented in 0.18, and no interface
details or delivery date are published.

## 0.20–0.23 — filesystem work

- **0.20 Filesystem Foundation.** Planned foundation for persistent storage.
- **0.21–0.23 Filesystem maturation.** Planned follow-on work.

Theia NTFS is the intended primary persistent filesystem. Theia NTRD is planned
for temporary files and is not a replacement for Theia NTFS. Neither is
implemented in 0.18.

## 0.24 — Program Foundation

Program Foundation is planned to introduce the Loader and Executor for native
program support. It is not a current capability and has no published date.

## Deferred decisions

- Ring 1 and Ring 2 general availability until HAL Foundation.
- Mihriban until application support exists.
- General third-party driver model.
- POSIX, Linux, Unix, and Windows compatibility.
- Compatibility layers as a shortcut to an application ecosystem.

## Candidate areas — not scheduled

Networking, USB, audio, accelerated graphics, UEFI boot, SMP, package manager,
installer, and multi-user accounts are not implemented and are not committed
milestones.

## Decision gates

Before a planned area becomes a committed milestone, it must pass:

1. architectural fit;
2. public contract impact review;
3. validation path;
4. maintenance cost review;
5. public disclosure review.

## Release policy

- Complete images are published through
  [GitHub Releases](https://github.com/Ale2323git/Theia-OS/releases).
- Releases provide images, not source.
- Release versions and dates are published on the Releases page; this roadmap
  does not predict them.
- A release does not imply production readiness.

## Related documentation

- [Vision](../Introduction/Vision.md)
- [Features](../Introduction/Features.md)
- [Development Status](Development-Status.md)
- [Version History](Version-History.md)
- [Filesystem](../Components/Filesystem.md)
- [HAL and Drivers](../Components/HAL-and-Drivers.md)

---

Theia OS is proprietary software. This roadmap describes public planning
position only and does not disclose private implementation details.
