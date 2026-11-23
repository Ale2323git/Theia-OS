# Version History

This page records public development milestones. It distinguishes development
history from published releases.

- Development milestones are recorded from the project's versioned work.
- Public releases are published on
  [GitHub Releases](https://github.com/Ale2323git/Theia-OS/releases).
- A milestone listed here is not automatically a public release.
- No future release version or date is invented here.

## Versioning

Theia OS's foundational development is recorded in a `0.x` line. The most
recent milestone recorded in this history is **0.18**. Later versions appear
only in the roadmap as planned work.

## Recorded milestones

| Version | Development date | Milestone | Summary |
| --- | --- | --- | --- |
| 0.8 | 2026-10-04 | Memory Foundation | Physical memory foundation |
| 0.9 | 2026-10-07 | Memory Region Manager | Region classification and management |
| 0.12 | 2026-10-10 | Input Foundation | Keyboard input support |
| 0.13 | 2026-10-11 | Console Expansion | Early interactive environment expansion |
| 0.14 | 2026-10-11 | Console Expansion | Continued interactive environment expansion |
| 0.15 | 2026-10-12 | Basic Closed | Recorded "Basic Closed" milestone |
| 0.16 | 2026-10-21 | Allocator Complete & Complete Shutdown | Allocator and shutdown work completed |
| 0.17 | 2026-11-01 | Scheduler Foundation | Task scheduler foundation |
| 0.18 | 2026-11-14 | System API Foundation | System API, diagnostics, Mashel, and published image validation |

The dates are recorded development dates, not public release dates. Earlier
development than 0.8 is not recorded in the available public history.

"Console Expansion" is a historical milestone name. The current official
console component name is Mashel.

## 0.18 — System API Foundation

The 0.18 line established the public system-service layer on top of the earlier
kernel foundation. Its documented scope includes:

- a System API foundation in eight areas: System, Diagnostics, Text Output,
  Memory, Handle, Task, Synchronization, and IPC;
- system information, time, uptime, and bounded logging;
- defined critical-failure presentations with a safe halt on every fatal path;
- native BIOS boot with a coordinator role documented as `initland`;
- Ring 3 Init starting Ring 3 Mashel through public system interfaces;
- Mashel banner, `theia>` prompt, and text output verified in the reference
  virtual environment;
- a published complete image validated as a usable system image.

The internal build and validation process is private and is not described here.

The 0.18 line does not add a persistent filesystem, Standway, native
application loading, Program Foundation, a graphical desktop, POSIX
compatibility, networking, USB, audio, SMP, or UEFI boot.

## 0.17 — Scheduler foundation

The 0.17 line established preemptive Task scheduling, task states, ownership,
waiting, and cleanup. The system schedules Tasks, not executables.

## 0.16 — Allocator completion and shutdown

The 0.16 line completed allocator work and shutdown support, preparing the
system for the scheduler and system-service layers.

## 0.15 — Basic Closed

The 0.15 line records the "Basic Closed" development milestone.

## 0.13 and 0.14 — Interactive environment expansion

These lines expanded the early interactive environment. Their "Console"
milestone wording is historical; the current component is Mashel.

## 0.12 — Input foundation

The 0.12 line established keyboard input support for the interactive
environment.

## 0.9 — Memory region manager

The 0.9 line added memory-region classification and management on top of the
0.8 memory foundation.

## 0.8 — Memory foundation

The 0.8 line established the physical memory foundation. It is the earliest
milestone recorded in the available public history.

## Future direction

The fixed roadmap order is:

1. 0.19 — Standway, codename Pequita.
2. 0.20 — Filesystem Foundation.
3. 0.21–0.23 — Filesystem maturation.
4. 0.24 — Program Foundation: Loader and Executor.

These are planned versions, not completed work. Ring 1 and Ring 2 availability
is deferred until HAL Foundation, and Mihriban is deferred while application
support does not exist.

## Public releases

Complete Theia OS `.img` images are published through
[GitHub Releases](https://github.com/Ale2323git/Theia-OS/releases). Release
versions, dates, and notes are published there; this page does not invent them.
The repository does not distribute source code or buildable source trees.

## Related documentation

- [Development Status](Development-Status.md)
- [Roadmap](Roadmap.md)
- [Features](../Introduction/Features.md)
- [Architecture Overview](../Architecture/Overview.md)

---

Theia OS is proprietary software. This version history describes public
milestones and does not disclose private implementation details.
