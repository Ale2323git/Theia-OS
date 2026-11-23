# Development Status

The documented history of Theia OS records **0.18 System API Foundation** as a
milestone. This page describes the recorded maturity, evidence, and limits of
that milestone; it is not a release announcement.

## Documented milestone

| Item | Status |
| --- | --- |
| Version line | 0.18 |
| Theme | System API Foundation |
| Kernel | Theia NT Kernel, hybrid |
| Privilege model | Ring 0 kernel, Ring 3 userland |
| Boot | Native BIOS path with coordinator role documented as `initland` |
| Userland | Ring 3 Init and Ring 3 Mashel |
| System API | Eight service areas |
| Distribution | Complete `.img` images through GitHub Releases |
| Source | Private |
| Maturity | Foundational; not production-ready |

## Status vocabulary

| Term | Meaning |
| --- | --- |
| **Implemented** | The behavior exists in the documented system. |
| **Verified** | The behavior was exercised in validation of the published image. |
| **Limited validation** | The behavior exists but relevant validation is incomplete. |
| **Planned** | Approved direction; not implemented and not dated. |
| **Deferred** | Intentionally outside the current scope. |
| **Not implemented** | Absent from 0.18. |
| **Historical** | Prior name or design; not current. |

## Evidence levels

The documented 0.18 validation includes:

- production of a complete bootable image;
- validation of the published image in the reference virtual environment;
- confirmation that the image reaches the Theia OS Mashel banner and `theia>`
  prompt;
- confirmation that the user-visible system services are available in that
  environment.

A virtual-machine boot is not the same as physical-hardware validation.

## Subsystem status

| Subsystem | Status | Notes |
| --- | --- | --- |
| Boot path | Implemented; verified in reference environment | Native BIOS path, no GRUB/UEFI |
| Boot coordination | Implemented | Mandatory coordinator role documented as `initland` |
| Kernel core | Implemented | Ring 0 services, interrupts, timing, faults |
| Memory | Implemented; limited validation | Isolation, permissions, validated access |
| Scheduler | Implemented; limited validation | Schedules Tasks |
| Task lifecycle | Implemented | States, ownership, waiting, cleanup |
| Resources and handles | Implemented | Opaque references and ownership |
| IPC | Implemented; limited validation | Bounded message channels |
| Synchronization | Implemented; limited validation | Events, mutexes, semaphores |
| System API | Implemented foundation | Eight areas; not a complete final API |
| System interface | Implemented | Public concepts documented; low-level details private |
| Diagnostics | Implemented | Bounded logging and system information |
| Init | Implemented; verified in reference environment | Ring 3 component that starts Mashel |
| Mashel | Implemented; verified in reference environment | Banner and prompt verified; full command matrix limited |
| Text Output | Implemented; verified in reference environment | Text output through public service |
| Physical hardware | Limited validation | Not broadly claimed |
| Standway | Planned | 0.19, codename Pequita |
| Filesystem | Planned | Theia NTFS and Theia NTRD not implemented in 0.18 |
| Ring 1/Ring 2 availability | Deferred | Until HAL Foundation |
| Mihriban | Deferred | No application support yet |
| Program Foundation | Planned | Loader and Executor in 0.24 |

## What is verified

- a complete 0.18 image exists as a published artifact;
- the published image boots to Mashel in the reference virtual environment;
- the user-visible banner, prompt, and text output are present;
- critical failures retain safe termination regardless of presentation.

## What is not verified

- broad physical-hardware compatibility;
- every interrupt and exception path on real hardware;
- exhaustive command-by-command Mashel interaction on the final image;
- sustained long-duration behavior;
- security hardening against a determined local attacker;
- planned 0.19+ features, filesystems, HAL Foundation, Program Foundation, or
  Mihriban as implemented capabilities.

## Known limitations

- single-processor design;
- BIOS boot only;
- bounded kernel resources;
- no persistent filesystem in 0.18;
- no demand paging, swap, or file-backed memory;
- no Standway, native application loader, package manager, installer, or SDK;
- no general third-party driver model;
- no POSIX, Linux, Unix, or Windows compatibility;
- no public source or buildable source tree;
- not production-ready.

## Quality gates

1. Architectural fit.
2. Implementation.
3. Validation of a complete image.
4. Validation of a booted system where the claim depends on startup.
5. Public documentation without private implementation details.

## Documentation priorities

- keep public documentation aligned with the verified 0.18 system;
- extend hardware validation where possible;
- preserve the public System API boundary;
- maintain clear distinction between implemented, planned, deferred, and
  historical items;
- prepare documentation for planned 0.19+ work without presenting it as
  implemented.

## Related documentation

- [Features](../Introduction/Features.md)
- [Build and Validation](Build-and-Validation.md)
- [Roadmap](Roadmap.md)
- [Version History](Version-History.md)
- [Architecture Overview](../Architecture/Overview.md)

---

Theia OS is proprietary software. This status page describes public maturity
and does not disclose private implementation details.
