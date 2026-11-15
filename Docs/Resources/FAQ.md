# Frequently Asked Questions

This FAQ answers common questions about Theia OS using the project's public
documentation. It does not disclose private implementation details.

## Identity

### What is Theia OS?

Theia OS is an independent, proprietary operating system built from the ground
up. It has its own kernel, the Theia NT Kernel, along with its own boot path,
system architecture, task model, memory model, object model, and system API.

See [What Is Theia OS?](../Introduction/What-Is-Theia-OS.md).

### Is Theia OS a Linux distribution?

No. Theia OS does not use the Linux kernel, a Linux userland, or Linux system
conventions. It is not a Linux distribution and is not based on Linux.

### Is Theia OS Unix or POSIX?

No. Theia OS is not based on Unix or POSIX, and it does not present POSIX
interfaces as its native system model. It has its own system architecture and
system API.

### Is Theia OS based on Windows NT?

No. "Theia NT" is the name of Theia's own kernel. It is not a Windows NT
derivative, and Theia OS does not provide Windows compatibility.

### Why is the kernel called "Theia NT"?

"Theia NT" is the project's name for its custom kernel. The name distinguishes
the kernel's identity within Theia OS. It should not be read as a claim of
compatibility with, or derivation from, Windows NT.

### Is Windows an inspiration for Theia OS?

Yes, as one source of inspiration. The project aims to adopt concepts associated
with mature desktop operating systems: a coherent desktop experience, clearly
separated system and application interfaces, structured process and resource
management, native application development facilities, a consistent application
ecosystem, and practical usability for everyday computing.

Inspiration is not imitation. Theia OS has its own kernel, architecture,
terminology, implementation, and technical identity. It is not a Windows clone,
a Windows reimplementation, or a Windows-compatible operating system, and no
such compatibility is claimed.

### Is Theia OS a hobbyist mini-OS?

No. Theia OS is an early-stage project, but it is engineered as a long-term
operating system with a coherent architecture, explicit boundaries, public
contracts, and a deliberate development process. Its long-term ambition is a
complete, general-purpose operating system suitable for everyday computing. It
is not a toy demonstration, a boot-only project, or a basic educational kernel.

### Is Theia OS production-ready?

No. The current 0.18 line is a foundation. It does not yet provide a general
application platform, persistent storage, networking, a graphical desktop, broad
hardware support, or a multi-user security model.

See [Development Status](../Development/Development-Status.md).

### Can I use Theia OS for everyday computing?

Not yet. Everyday general-purpose computing is the project's long-term ambition,
not the current state. The 0.18 line is a foundation without a persistent
filesystem, a native application ecosystem, or a graphical desktop.

## Availability and licensing

### Is the source code available?

No. Theia OS is proprietary. The public repository distributes documentation and
complete disk images, not source code, source packages, or buildable source
trees.

### Can I download Theia OS?

Complete bootable `.img` disk images are distributed through
[GitHub Releases](https://github.com/Ale2323git/Theia-OS/releases). If no release
is listed, no public image is currently available.

### Can I build Theia OS from source?

No public source tree or build system is distributed, so there is no public
build procedure.

### Is Theia OS free?

The public documentation and any published disk images may be downloaded without
a fee, but Theia OS is proprietary. Downloading an image does not grant
open-source rights or permission to modify, redistribute, or reuse the software.
Licensing terms are set by the copyright holder.

### Is Theia OS open source?

No. Theia OS is proprietary software. No open-source license is granted.

### Can I modify or redistribute a Theia OS image?

Not without prior written permission from the copyright holder. The public
availability of an image does not grant redistribution or modification rights.

### Can I use Theia OS commercially?

Commercial use requires permission from the copyright holder. No commercial
license is granted by the public documentation or by downloading an image.

### Can I contribute code?

There is no public source contribution process. Documentation corrections,
broken-link reports, and reproducible problems with published images can be
reported through the [issue tracker](https://github.com/Ale2323git/Theia-OS/issues).

### How do I report a security issue?

Do not post security-sensitive details in a public issue. Report them privately
to the project owner.

### Where is the license?

The repository includes a `LICENSE` file. It is not an open-source license.
Theia OS is proprietary, and all rights are reserved by the copyright holder.
For licensing questions, contact the project owner.

## Platform and hardware

### What platform does Theia OS target?

The documented 0.18 line targets BIOS-class x86 systems and boots from a complete
`.img` disk image. It does not provide UEFI boot in the documented line.

### Is there a 64-bit version?

The documented platform is the current 32-bit x86 protected-mode system. No
64-bit version is documented as available.

### Does Theia OS support ARM or RISC-V?

No ARM or RISC-V platform support is documented for the 0.18 line.

### Does Theia OS support multiple processors?

No. The documented 0.18 line is designed for single-processor systems and does
not implement symmetric multiprocessing.

### Does Theia OS support UEFI?

No. The documented boot path uses BIOS. UEFI boot is not implemented in the 0.18
line.

### Can I run Theia OS in a virtual machine?

The disk image is intended to be bootable in a suitable virtual machine or on
compatible hardware. QEMU is the primary documented validation environment.
Review the release notes for platform details.

### Can I run Theia OS on my physical PC?

Possibly, but broad physical-hardware compatibility is not claimed. The safest
way to evaluate the system is a virtual machine. Physical-hardware validation is
limited.

### Does Theia OS support Secure Boot or TPM?

No Secure Boot or TPM support is documented for the 0.18 line.

### Can Theia OS run other operating systems in a virtual machine?

No. Theia OS is not a hypervisor and does not document virtual-machine hosting
for other operating systems.

## Features and system behavior

### Does Theia OS have a graphical desktop?

No. The current user environment is a VGA text console. There is no graphical
desktop environment in the documented 0.18 line. A complete desktop experience
is a long-term goal, not a current feature.

### Does Theia OS have a file system?

Not in the documented 0.18 line. No general-purpose file system or persistent
storage stack is implemented. A dedicated persistent Theia OS filesystem is
planned as a long-term component, but it does not exist yet.

### Does Theia OS support networking?

No networking stack is implemented in the documented 0.18 line.

### Does Theia OS support USB?

No USB support is implemented in the documented 0.18 line.

### Does Theia OS support audio?

No audio support is implemented in the documented 0.18 line.

### Does Theia OS support third-party applications?

No general application runtime, application loader, SDK, package manager, or
third-party application ecosystem is documented for the 0.18 line. The console
and user-mode demonstration are the current user environment. A native
application ecosystem, built around the planned Standway application-facing API,
is a long-term goal.

### Does Theia OS run Linux or Windows programs?

No. Theia OS does not provide Linux, Unix, POSIX, or Windows compatibility
layers.

### Does Theia OS have a shell?

It has an interactive console with built-in commands. It is not a general-purpose
Unix shell.

### What is Standway?

Standway is the planned application-facing API for Theia OS. The System API is
the kernel-service boundary; Standway is intended to be the interface that native
applications use. Standway is not implemented in the 0.18 line, and no interface
details or delivery date are published.

### Is the System API complete?

No. The 0.18 System API covers tasks, memory, handles, IPC, synchronization, and
system services for the documented scope, but it is a foundation rather than the
complete final interface. Additional services are expected. Standway is a
separate, planned API and is not part of the 0.18 System API.

### What can the console do?

The console provides commands for help, system information, memory information,
uptime, reboot, shutdown, screen clearing, echo, and the user-mode
demonstration. See [Features](../Introduction/Features.md).

### Does Theia OS support multiple users?

No multi-user account model is documented for the 0.18 line.

### Is Theia OS secure?

Theia OS has a real user/kernel boundary, per-task address spaces, opaque
handles, and validated system-call inputs. It is designed with security
boundaries in mind. It has not been presented as audited, hardened, or
production-grade, and it does not yet provide a multi-user security model or a
general sandbox for untrusted third-party applications.

### Does Theia OS have a package manager?

No. There is no package manager, software repository, or system installer in the
documented 0.18 line.

### Does Theia OS support demand paging or swap?

No. The documented memory model does not provide demand paging from storage or a
swap subsystem.

### Does Theia OS have a real-time scheduler?

No. The scheduler is preemptive and round-robin, but Theia OS does not claim hard
real-time guarantees or deadline scheduling.

### Can tasks share memory?

Yes, under controlled rules. Memory sharing requires a directly owned target
task, valid permissions, and explicit system calls. Shared pages are
reference-tracked. See [Memory Management](../Components/Memory-Management.md).

### How are tasks identified?

Each task has a generation-tagged identity. When a task slot is reused, the
generation changes, so a stale identifier cannot silently refer to a different
task.

### What happens when a user task crashes?

A user-mode fault is handled at the user/kernel boundary. The kernel can
terminate the offending task and clean up its resources without granting it
kernel privileges. See [Process Management](../Components/Process-Management.md).

## Development and releases

### What is the current version?

The current public milestone is **Theia OS 0.18**. See
[Version History](../Development/Version-History.md).

### What does version 0.18 include?

The 0.18 line establishes the system call and diagnostics foundation on top of
the memory, scheduler, object, IPC, and synchronization work from earlier
milestones. See [Version History](../Development/Version-History.md).

### What is next?

Near-term work focuses on validation and hardening of the existing foundation.
The long-term architectural direction includes a dedicated persistent filesystem,
the Standway application-facing API, a native application ecosystem, and a
graphical desktop experience. No implementation sequence or delivery date is
committed. See the [Roadmap](../Development/Roadmap.md).

### When will the next release be available?

No public release date is promised. Check
[GitHub Releases](https://github.com/Ale2323git/Theia-OS/releases) for published
versions and notes.

### Why is the public repository documentation-only?

Theia OS is proprietary. The public repository provides documentation and
complete disk images while protecting the private source code and internal
implementation.

### How can I verify the claims in the documentation?

The documentation distinguishes implemented, host-tested, BIOS/QEMU-validated,
and hardware-validated behavior. See
[Development Status](../Development/Development-Status.md). Independent
verification of the private implementation is not possible from the public
repository.

### How do I report a documentation error?

Open an issue in the [GitHub issue tracker](https://github.com/Ale2323git/Theia-OS/issues)
with the document name, the incorrect statement, and the correction you believe
is needed.

## Documentation

### Where should I start?

Start with [What Is Theia OS?](../Introduction/What-Is-Theia-OS.md), then read
the [Architecture Overview](../Architecture/Overview.md). Use the
[Glossary](Glossary.md) for unfamiliar terms.

### Where are the screenshots?

The existing screenshots are linked from the repository
[README](../../README.md#screenshots).

### Does the documentation include source code?

No. The public documentation does not include source code, internal function
names, private data structures, memory maps, or implementation examples.

### Does the documentation describe every internal component?

No. It describes the public architecture and the documented components only.
Some internal subsystems are summarized at a high level and are not documented
in detail.

---

Theia OS is proprietary software. This FAQ describes public information only and
does not disclose private implementation details.
