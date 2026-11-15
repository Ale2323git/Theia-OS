# Version History

This page records Theia OS development milestones. It distinguishes development
history from public release artifacts.

- **Development milestones** are recorded from the project's versioned work.
- **Public releases** are published on
  [GitHub Releases](https://github.com/Ale2323git/Theia-OS/releases) with their
  own versions, dates, notes, and disk images.
- A milestone listed here is not automatically a public release.
- No release version or release date is invented on this page.

## Versioning

Theia OS uses a simple `0.x` version line during its foundational development.
The current documented milestone is **0.18**. Later version numbers are not
assigned here.

The version line describes development progress, not production maturity. A
higher number does not mean the system is ready for general use.

## Recorded milestones

| Version | Development date | Milestone | Summary |
| --- | --- | --- | --- |
| 0.8 | 2026-10-04 | Memory Foundation | Established the physical memory foundation |
| 0.9 | 2026-10-07 | Memory Region Manager | Added memory-region classification and management |
| 0.12 | 2026-10-10 | Input Foundation | Established keyboard input support |
| 0.13 | 2026-10-11 | Console Expansion | Expanded the console environment |
| 0.14 | 2026-10-11 | Console Expansion | Continued console expansion |
| 0.15 | 2026-10-12 | Basic Closed | Recorded the "Basic Closed" development milestone |
| 0.16 | 2026-10-21 | Allocator Complete & Complete Shutdown | Completed allocator work and shutdown support |
| 0.17 | 2026-11-01 | Scheduler Foundation | Established the task scheduler foundation |
| 0.18 | 2026-11-14 | Syscalls Foundation | Established the system call and diagnostics foundation |

The dates above are development dates recorded in the project's history, not
public release dates.

The available history begins at 0.8. Earlier development is not recorded in the
version history available for this documentation.

## 0.18 — System calls and diagnostics foundation

The 0.18 line establishes the public system-service layer on top of the earlier
kernel foundation.

Documented scope includes:

- a System API foundation covering tasks, memory, handles, IPC,
  synchronization, and system services. It is not a complete final API;
- system information queries for memory, the boot memory map, scheduler
  statistics, kernel identity, and interface information;
- real-time clock time and monotonic uptime reporting;
- per-task last-error reporting;
- a bounded kernel logging service with application log submission;
- integration of diagnostics with the scheduler, memory subsystem, IPC,
  synchronization, and console;
- a user-mode demonstration that exercises system calls end to end.

The 0.18 line does not add a persistent filesystem, networking, the Standway
application-facing API, a native application ecosystem, a graphical desktop, or
POSIX compatibility.

## 0.17 — Scheduler foundation

The 0.17 line establishes the task scheduler and task model:

- preemptive, round-robin scheduling for single-processor systems;
- kernel and user tasks;
- explicit task states;
- task creation, waiting, and termination;
- blocking and wait queues;
- task ownership;
- address-space activation for user tasks;
- scheduler statistics.

## 0.16 — Allocator completion and shutdown

The 0.16 line completes allocator work and adds complete shutdown support. It
builds on the memory work from earlier milestones and prepares the system for
the scheduler and system-service layers that follow.

## 0.15 — Basic Closed

The 0.15 line records the "Basic Closed" development milestone. It is part of
the sequence leading to the later scheduler and system-service foundation.

## 0.13 and 0.14 — Console expansion

The 0.13 and 0.14 lines expand the console environment. They build on the input
foundation from 0.12 and prepare the interactive environment used by later
milestones.

## 0.12 — Input foundation

The 0.12 line establishes keyboard input support. Input is a prerequisite for
the interactive console and its built-in commands.

## 0.9 — Memory region manager

The 0.9 line adds memory-region classification and management on top of the 0.8
memory foundation. This work supports later physical and virtual memory
management.

## 0.8 — Memory foundation

The 0.8 line establishes the physical memory foundation. It is the earliest
milestone recorded in the available version history.

## Long-term direction

The project's long-term architectural direction includes a dedicated persistent
filesystem, the Standway application-facing API, a native application ecosystem,
and a graphical desktop experience. These are goals, not features of 0.18, and
they are not assigned a version or date on this page.

## Public releases

The public repository distributes complete Theia OS `.img` disk images through
[GitHub Releases](https://github.com/Ale2323git/Theia-OS/releases).

- Release versions, dates, and notes are published on the Releases page.
- This page does not assert a release date or a release version that is not
  published there.
- The repository does not distribute source code, source packages, or buildable
  source trees.
- A release image is a complete system image, not a source archive.

## Stability and compatibility

Theia OS is in foundational development. The public documentation distinguishes:

- interfaces and behaviors that are part of the documented architecture;
- capabilities that are implemented but have limited validation;
- capabilities that are not implemented.

A version milestone does not imply backward compatibility with another operating
system, nor does it imply that every interface is permanently frozen. Public
contracts are treated as long-lived, but the system as a whole is still evolving.

## Related documentation

- [Development Status](Development-Status.md)
- [Roadmap](Roadmap.md)
- [Features](../Introduction/Features.md)
- [Architecture Overview](../Architecture/Overview.md)

---

Theia OS is proprietary software. This version history describes public
milestones and does not disclose private implementation details.
