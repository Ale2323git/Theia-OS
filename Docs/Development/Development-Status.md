# Development Status

Theia OS is in **active foundational development**. The current public milestone
is **0.18**, which establishes the system call and diagnostics foundation on top
of earlier work on memory, scheduling, objects, IPC, and synchronization.

This page describes maturity, verification, and known limits. It is not a
release announcement, and it does not promise dates.

## Current milestone

| Item | Status |
| --- | --- |
| Version line | 0.18 |
| Milestone theme | System calls and diagnostics foundation |
| Architecture | Hybrid Theia NT Kernel, single-processor, BIOS boot |
| Distribution | Complete `.img` disk images through GitHub Releases |
| Source availability | Private; no source distribution |
| Maturity | Foundational; not production-ready |
| Project state | In development |
| Primary validation environment | QEMU and host-side test suites |
| Physical-hardware coverage | Limited |
| Long-term direction | General-purpose platform: persistent filesystem, Standway, native application ecosystem, desktop (not implemented) |

The 0.18 line is a foundation, not a finished platform. It demonstrates that
the kernel, memory manager, scheduler, object model, IPC, synchronization,
system API, diagnostics, boot path, and console can operate as one system.

## Status vocabulary

The documentation uses the following terms consistently:

| Term | Meaning |
| --- | --- |
| **Designed** | The behavior or constraint is part of the architecture. |
| **Implemented** | The behavior exists in the documented system. |
| **Compiled** | The system builds the component into a complete image. |
| **Host-tested** | A host-side test suite exercises the behavior. |
| **BIOS/QEMU-validated** | A real boot in QEMU or on BIOS hardware exercised the behavior. |
| **Hardware-validated** | The behavior was exercised on physical hardware. |
| **Implemented — validation limited** | The behavior exists, but the relevant validation is incomplete or was not reproduced in the latest pass. |
| **In development** | Work is underway, but the capability is not yet implemented or verified as usable. |
| **Partially implemented** | Only a limited form of the capability exists. |
| **Planned** | The capability belongs to the approved long-term direction but is not implemented, and no delivery date is promised. |
| **Deferred** | The capability is intentionally outside the current scope. |
| **Not implemented** | The capability is absent from the documented 0.18 system. |

A higher evidence level is not implied by a lower one. A component that is
implemented and compiled is not automatically described as hardware-validated.

## Evidence levels

The project distinguishes between design intent and observed behavior.

| Level | What it means |
| --- | --- |
| Designed | The constraint or interface is part of the architecture. |
| Implemented | The behavior is present in the documented system. |
| Compiled | The build produces the component and the complete disk image. |
| Host-tested | A host-side test exercises the behavior outside a real boot. |
| BIOS/QEMU-validated | A boot in QEMU or on BIOS hardware exercised the behavior. |
| Hardware-validated | A physical machine exercised the behavior. |

The latest documentation pass confirmed that the working tree builds and that
the host-side test suites pass. It did **not** reproduce a QEMU or physical-
hardware boot.

## Subsystem status

| Subsystem | Status | Notes |
| --- | --- | --- |
| Boot path | Implemented — validation limited | Builds a complete image; earlier BIOS/QEMU boots recorded, not reproduced in the latest pass |
| Kernel core | Implemented | Processor, interrupt, timing, fault, and privilege foundations |
| Physical memory | Implemented and host-tested | Page tracking, allocation, release, and accounting |
| Virtual memory | Implemented and host-tested | Paging, address spaces, permissions, and isolation |
| User memory | Implemented and host-tested | Regions, sharing, protection, and user-buffer validation |
| Scheduler | Implemented and host-tested | Preemptive round-robin task scheduling |
| Task lifecycle | Implemented and host-tested | Creation, states, waiting, termination, and cleanup |
| Object and handle model | Implemented and host-tested | Object lifetimes, handles, generations, and destruction |
| Port IPC | Implemented and host-tested | Bounded messages, calls, replies, and ownership rules |
| Events | Implemented and host-tested | Manual-reset and auto-reset signaling |
| Mutexes | Implemented and host-tested | Ownership, waiting, transfer, and close rules |
| Semaphores | Implemented and host-tested | Counting, waiting, signaling, and cleanup |
| System API | Implemented foundation for 0.18; not complete | Task, memory, handle, IPC, synchronization, and system services in the documented scope |
| Diagnostics | Implemented and host-tested | Logging, system information, time, uptime, and last error |
| Standway application API | Planned | Application-facing API; not implemented |
| Persistent filesystem | Planned | Dedicated Theia OS filesystem; not implemented |
| Native application ecosystem | Planned | Application model and development facilities; long-term |
| Graphical desktop | Long-term goal | Not implemented; the console is the current environment |
| Console | Implemented — validation limited | Builds; full interactive behavior not revalidated on hardware in the latest pass |
| Keyboard input | Implemented — validation limited | Implemented; hardware validation limited |
| ACPI power-off | Implemented — validation limited | No physical platform accepted a power-off request in the latest pass |
| Real-time clock | Implemented — validation limited | Decoding host-tested; hardware path not revalidated in the latest pass |
| Init image | Implemented | Handoff stage between kernel and console |
| User-mode demonstration | Implemented | Exercises task, memory, system, time, uptime, error, and log paths |

## What is verified

The current public verification position is:

- the working tree builds a complete Theia OS disk image;
- the host-side test suites for the ABI, objects, IPC, scheduler,
  synchronization, and system services pass;
- the System API covers the core 0.18 service areas but is not a complete final
  API;
- the user-mode demonstration is designed to exercise system calls end to end;
- earlier development sessions recorded successful BIOS/QEMU boots and a clean
  console demonstration;
- the latest documentation pass did not re-run QEMU or physical-hardware
  validation, so those earlier runs are not presented as fresh evidence.

This distinction is deliberate. The documentation does not upgrade an earlier
observation into a current validation claim.

## What is not verified

The following are not claimed as fully validated:

- broad physical-hardware compatibility;
- every interrupt and exception path on real hardware;
- full interactive console behavior on physical hardware;
- ACPI power-off on a range of real platforms;
- direct real-time clock access on a range of real platforms;
- long-duration stability under sustained load;
- security hardening against a determined local attacker;
- production-level performance characteristics.

These are not hidden defects. They are areas where the current evidence is
limited.

## Known limitations

The documented 0.18 system has these limitations:

- **Single-processor design.** No symmetric multiprocessing.
- **BIOS boot only.** No UEFI boot path.
- **Fixed-capacity kernel resources.** Task slots, address spaces, object pools,
  and IPC queues are bounded.
- **No file system in 0.18.** No persistent storage stack or file-backed memory.
  A dedicated persistent filesystem is planned as a long-term component.
- **No networking.** No network protocols or device support.
- **No USB, audio, or accelerated graphics.**
- **System API not complete.** The 0.18 API covers core services but is not the
  final interface.
- **No Standway application API.** The System API is the only public service
  boundary in 0.18. Standway is planned.
- **No graphical desktop.** The user environment is a VGA text console. A
  desktop experience is a long-term goal.
- **No POSIX or Linux compatibility.**
- **No general application ecosystem.** The console and demonstration are the
  current user-mode environment. A native ecosystem is planned long-term.
- **Limited hardware coverage.** QEMU is the primary validation environment.
- **No public source or build system.** The public repository distributes
  documentation and complete disk images only.

## Long-term architectural direction

The project's long-term ambition is a complete, general-purpose operating system
for everyday computing. The stated architectural direction includes:

| Component | Status | Position |
| --- | --- | --- |
| Standway application-facing API | Planned | Separate API above the System API; not implemented |
| Dedicated persistent filesystem | Planned | First-class storage component; no filesystem in 0.18 |
| Native application ecosystem | Planned | Application model and development facilities; long-term |
| Graphical desktop experience | Long-term goal | Not implemented; the console is the current environment |
| Broader hardware and device coverage | Long-term goal | QEMU is the primary validation environment today |

These items are directional. No specific sequence, release date, or
compatibility commitment is approved for public disclosure. The near-term focus
remains validation and hardening of the 0.18 foundation.

## Quality gates

A capability is not treated as complete merely because code exists. The project
uses these gates:

1. **Architectural fit.** The capability must follow the kernel's object,
   memory, task, and security model.
2. **Implementation.** The behavior must exist in the system.
3. **Build.** The system must compile into a complete image.
4. **Host testing.** The behavior must be exercised by the appropriate test
   suite where possible.
5. **Boot validation.** The behavior should be exercised in a real boot when the
   capability depends on hardware or boot-time state.
6. **Documentation.** Public behavior must be documented without exposing
   private implementation.

A capability can be implemented before all gates are satisfied. Its status must
say so.

## Production readiness

Theia OS is **not production-ready**.

It does not yet provide:

- a stable general-purpose application platform;
- persistent storage;
- networking;
- a graphical environment;
- broad hardware support;
- a multi-user security model;
- a complete driver model;
- long-term stability guarantees.

Treating the current system as a production operating system would misrepresent
its maturity.

## Current priorities

The project's near-term priorities are validation and hardening, not rapid
feature expansion:

- reproduce and extend boot validation;
- validate console and keyboard behavior;
- exercise interrupt and exception paths;
- validate firmware power management and real-time clock access on real
  platforms;
- strengthen the evidence behind memory, scheduling, IPC, and synchronization;
- clarify the public boundary between the System API and the planned Standway
  application-facing API;
- keep the public documentation aligned with the actual system, and keep
  long-term direction clearly separated from current capability.

See the [Roadmap](Roadmap.md) for the planning position and
[Version History](Version-History.md) for recorded milestones.

## Related documentation

- [Features](../Introduction/Features.md)
- [Roadmap](Roadmap.md)
- [Version History](Version-History.md)
- [Architecture Overview](../Architecture/Overview.md)
- [Memory Management](../Components/Memory-Management.md)
- [Process Management](../Components/Process-Management.md)

---

Theia OS is proprietary software. This status page describes public maturity and
does not disclose private implementation details.
