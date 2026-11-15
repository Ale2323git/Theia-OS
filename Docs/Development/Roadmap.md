# Roadmap

This roadmap describes the project's public planning position. It separates the
completed 0.18 foundation, near-term validation priorities, the long-term
architectural direction, and areas that are **not** approved commitments.

Theia OS does not publish delivery dates, release cadences, or feature
guarantees beyond what is explicitly stated here. A capability appearing in
this document is not a promise that it will be delivered.

## Planning principles

The roadmap follows the same principles as the architecture:

1. **Finish the foundation before expanding it.** A broad feature list is less
   valuable than a reliable core.
2. **Validate before claiming.** Implemented behavior must be tested at the
   appropriate level before it is described as complete.
3. **Protect the architecture.** New work must fit the kernel's object, memory,
   task, and security model rather than bypassing it.
4. **Keep public contracts stable.** Interface changes require deliberate
   design, not accidental drift.
5. **Do not invent compatibility.** Theia OS does not adopt another operating
   system's interfaces as a shortcut to an ecosystem.
6. **Publish only approved scope.** Unapproved features and dates are not
   presented as plans.
7. **Separate direction from capability.** Long-term architectural direction is
   stated as direction, never as an implemented feature.

## Completed foundation

The 0.18 line completes a foundational sequence:

| Area | State |
| --- | --- |
| Boot path | Native BIOS boot path and complete disk image |
| Kernel core | Protected mode, interrupts, timing, memory, scheduling, faults |
| Physical memory | Page tracking, allocation, release, and accounting |
| Virtual memory | Paging, user address spaces, permissions, and sharing |
| Task model | Preemptive scheduling, states, waiting, termination, ownership |
| Object model | Objects, handles, generations, reference tracking, destruction |
| IPC | Bounded message ports with call and reply operations |
| Synchronization | Events, mutexes, and semaphores |
| System API | Foundation for task, memory, handle, IPC, synchronization, and system services; not a complete final API |
| Diagnostics | Logging, system information, time, uptime, and last error |
| User environment | Init, Console, keyboard input, built-in commands, demonstration |

These are documented in [Features](../Introduction/Features.md) and
[Version History](Version-History.md).

## Near-term priorities

The near-term focus is validation and hardening of the existing foundation.
These priorities are directly derived from the known limitations of the current
system.

### Boot and platform validation

- Reproduce and extend BIOS/QEMU boot validation.
- Validate the complete boot path on physical hardware where possible.
- Exercise the console, keyboard, timer, and interrupt paths in real boots.
- Validate firmware power management and real-time clock access on real
  platforms.
- Record the results separately from host-side tests.

### Kernel hardening

- Exercise interrupt and exception paths beyond host-side coverage.
- Strengthen fault containment and recovery behavior for user-mode faults.
- Review resource-exhaustion behavior across bounded kernel services.
- Improve the consistency of validation and cleanup on failure paths.
- Continue reviewing the user/kernel boundary for unsafe assumptions.

### Memory and scheduling evidence

- Extend host-side coverage of memory sharing, protection, and address-space
  cleanup.
- Exercise scheduler behavior under sustained task creation and termination.
- Validate blocking, timeout, and wakeup behavior under load.
- Improve documentation of the public memory and task contracts.

### Documentation and release quality

- Keep public documentation aligned with the implemented system.
- Ensure release notes accurately describe the image and its limitations.
- Maintain the distinction between implemented, limited, and unimplemented
  capabilities.
- Define the public boundary between the System API and the planned Standway
  application-facing API without publishing private interface details.
- Keep the public repository free of private implementation material.

## Long-term architectural direction

Theia OS is developed with the long-term ambition of becoming a complete,
general-purpose operating system for everyday computing. The following
components belong to that direction. They are **not implemented** in the
documented 0.18 system, and no delivery dates are committed.

| Component | Status | Position |
| --- | --- | --- |
| Standway application-facing API | Planned | Separate API above the System API for native applications |
| Dedicated persistent filesystem | Planned | First-class Theia OS storage component |
| Native application ecosystem | Planned | Native development model, application interfaces, and development facilities |
| Graphical desktop experience | Long-term goal | Complete desktop environment on the system's own architecture |
| Broader hardware and device coverage | Long-term goal | Beyond the current BIOS/QEMU-focused validation |

These are architectural goals. Their presence here does not mean that work on
every item has started, and it does not establish a sequence.

Windows is one of the sources of inspiration for the project's platform
ambitions, particularly in the separation of system and application interfaces
and the idea of a coherent desktop and application ecosystem. Inspiration does
not imply imitation or compatibility: Theia OS is not a Windows clone, a Windows
reimplementation, or a Windows-compatible operating system.

## Candidate areas — not scheduled

The following areas are plausible for a general-purpose operating system but are
**not approved, scheduled, or promised** for Theia OS:

| Area | Status |
| --- | --- |
| Networking | Not implemented; not scheduled |
| USB support | Not implemented; not scheduled |
| Audio support | Not implemented; not scheduled |
| Symmetric multiprocessing | Not implemented; not scheduled |
| UEFI boot | Not implemented; not scheduled |
| Package manager and software repository | Not implemented; not scheduled |
| System installer | Not implemented; not scheduled |
| Multi-user security model | Not implemented; not scheduled |
| POSIX, Unix, Linux, or Windows compatibility | Not a current goal; not scheduled |

A future roadmap may promote some of these areas after the foundation has been
validated and the architecture has been reviewed. Until that happens, they
should not be read as planned features.

## Explicitly deferred or excluded

Some decisions are not waiting for a future milestone:

- **Compatibility with another operating system's native interfaces.** Theia OS
  is not a compatibility layer and does not define its identity around another
  platform.
- **POSIX compatibility.** A POSIX layer is not part of the current
  architecture.
- **Windows cloning or compatibility.** Windows informs selected platform
  concepts, but Theia OS is not a Windows reimplementation and does not target
  Windows compatibility.
- **Legacy boot conventions.** The documented boot path is native to Theia OS;
  it does not use GRUB or another operating system's loader.
- **Open-source distribution of the implementation.** Theia OS is proprietary;
  the public repository distributes documentation and complete images, not
  source code.

## Decision gates for future work

Before a candidate area or a long-term direction becomes a committed, scheduled
milestone, it should pass these gates:

1. **Architectural fit.** Does it preserve the kernel's security and resource
   model?
2. **Public contract impact.** Can it be added without destabilizing existing
   interfaces?
3. **Validation path.** Can its behavior be tested at the appropriate level?
4. **Maintenance cost.** Can the project support it without fragmenting the
   architecture?
5. **Public disclosure review.** Can it be documented without exposing private
   implementation details?

## Release policy

- Complete disk images are distributed through
  [GitHub Releases](https://github.com/Ale2323git/Theia-OS/releases).
- Release versions and dates are published on the Releases page. This roadmap
  does not predict them.
- The long-term architectural direction does not imply a release schedule or a
  committed implementation sequence.
- No source code, source package, or buildable source tree is distributed.
- A release does not imply production readiness. Release notes should state the
  validation level and known limitations.

## How this roadmap changes

The roadmap is updated when the project has enough evidence to make a public
commitment. Changes should:

- preserve the project's identity and architectural decisions;
- distinguish committed work, long-term direction, and candidate areas;
- avoid invented dates or compatibility claims;
- keep long-term goals separate from implemented capability;
- keep the public documentation consistent with the actual system.

If a capability is not listed as committed, it should be treated as outside the
current public plan.

## Related documentation

- [Vision](../Introduction/Vision.md)
- [Features](../Introduction/Features.md)
- [Development Status](Development-Status.md)
- [Version History](Version-History.md)
- [Architecture Overview](../Architecture/Overview.md)

---

Theia OS is proprietary software. This roadmap describes public planning
position only and does not disclose private implementation details.
