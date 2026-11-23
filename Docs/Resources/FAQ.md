# Frequently Asked Questions

This FAQ answers common questions using the public Theia OS documentation. It
does not disclose private implementation details.

## Identity

### What is Theia OS?

An independent, proprietary operating system with its own kernel, boot path,
System API, and user environment. It is not a Linux distribution and is not
based on Unix, POSIX, or Windows.

### How is Theia OS built?

Theia OS has a proprietary internal build and validation process. Its commands,
scripts, parameters, and procedures are not published. The public repository
contains documentation and release images, not source code.

### Is Theia OS production-ready?

No. The documented 0.18 line is recorded as a foundation, not as a general
daily-use operating system.

## Boot and Mashel

### Does Theia OS use GRUB?

No. It uses a native BIOS boot path.

### What is `initland`?

The documented name of the mandatory boot coordinator role. The documented 0.18
path established the coordinator from kernel initialization and handed off to
Ring 3 Init. It is not a userland component.

### Does the kernel start Mashel directly?

No. The kernel creates Init in Ring 3; Init then creates Mashel through public
system interfaces.

### Is Mashel the same as Theia Console?

"Theia Console" is a historical name. The current official component is
**Mashel**.

### Does Mashel access hardware directly?

No. It uses public system services. Privileged operations are handled by the
system through defined internal channels that are not published.

### What does a successful boot show?

The Theia OS Mashel banner, `Type 'HELP' for a list of commands.`, and the
`theia>` prompt on text output. This was verified in the reference virtual
environment on the published 0.18 image.

## System API

### How many System API areas are there?

Eight: System, Diagnostics, Text Output, Memory, Handle, Task, Synchronization,
and IPC. Internal and privileged interfaces are not public userland APIs.

### How do userland components request kernel services?

Through the System API. Low-level binary details are private and are not part of
the public product documentation.

### Is Standway available?

No. Standway is planned for 0.19, codename Pequita, as the application-facing
API above the System API.

### Are low-level interface details public?

No. The public documentation describes the purpose and capabilities of the
system interface. Numeric identifiers, binary layouts, and privileged mechanisms
remain private.

## Features

### Does 0.18 have a filesystem?

No. Theia NTFS is planned as the primary persistent filesystem and Theia NTRD
is planned for temporary files. NTRD is not a replacement for NTFS.

### Does 0.18 have a native application loader?

No. Program Foundation, Loader and Executor are planned for 0.24.

### Are Ring 1 and Ring 2 available?

Not as public userland facilities. General availability is deferred until HAL
Foundation. The normal system uses Ring 0 and Ring 3.

### Does the system support SMP, UEFI, networking, USB, audio, or a desktop?

No. These are not implemented in the documented 0.18 line.

### Can tasks share memory?

Yes, under explicit memory-sharing rules involving ownership, permissions, and
authorization.

### What is scheduled?

The system schedules Tasks, not executables.

## Roadmap and releases

### What does the roadmap list after 0.18?

0.19 Standway, codename Pequita; then 0.20 Filesystem Foundation, 0.21–0.23
filesystem maturation, and 0.24 Program Foundation with Loader and Executor.
No dates are published.

### Where are images published?

On [GitHub Releases](https://github.com/Ale2323git/Theia-OS/releases). Release
notes provide platform requirements and known limitations.

### How can I report an issue?

Use the [issue tracker](https://github.com/Ale2323git/Theia-OS/issues). Include
the release, execution environment, steps, and observed behavior. Do not include
private source or security-sensitive details.

## Documentation

### Where should I start?

Start with [What Is Theia OS?](../Introduction/What-Is-Theia-OS.md), then read
the [Architecture Overview](../Architecture/Overview.md).

### Does the documentation include source code?

No. It describes public architecture, interfaces, behavior, status, and system
usage without exposing private implementation.

---

Theia OS is proprietary software. This FAQ describes public information only
and does not disclose private implementation details.
