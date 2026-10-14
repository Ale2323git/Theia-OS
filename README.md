# Theia OS

**An independent operating system built from the ground up.**

Theia OS is an independent, closed-source operating system developed using freestanding C and NASM assembly. It features its own BIOS bootloader, kernel, and interactive command-line interface, establishing the foundation for a future operating system with its own architecture and design.

## Overview

Theia OS is a personal operating system project focused on building its fundamental components independently.

The project follows a development approach centered on core functionality first, followed by security and visual interface development. Its architecture and features are developed incrementally, with each release establishing a new foundation for future development.

Theia OS does not rely on an existing operating system kernel or an external bootloader. It has its own boot process and kernel implementation.

## Theia OS 0.15

**Version 0.15 marks the completion of the Basic Functionality development phase.**

This milestone represents the completion of the initial development stage and establishes the foundation for subsequent phases of Theia OS.

The project is developed using:

- Freestanding C for low-level system components.
- NASM assembly for boot and processor-level routines.
- A custom BIOS bootloader.
- An independently developed kernel.
- An interactive command-line environment.

The completion of this milestone does not imply that every planned operating system feature is already implemented. Development will continue through subsequent phases.

## Features

### Boot Process

Theia OS uses its own BIOS bootloader to begin the system startup process and load its operating system components.

The boot process is designed as part of Theia OS rather than relying on an external bootloader.

### Kernel

Theia OS includes its own kernel, written using freestanding C and assembly language.

The kernel provides the foundation for low-level functionality and the future expansion of system services.

### Memory Management

Memory-related development establishes the foundations for identifying and managing system memory.

Additional memory management capabilities will be developed as the operating system evolves.

### Theia Console

Theia Console provides a command-line interface for interacting with the running system.

It allows users to execute supported commands and access basic system information.

## Commands

The current command interface includes the following commands:

| Command | Description |
|---|---|
| `HELP` | Displays available commands. |
| `VERSION` | Displays version information. |
| `ECHO` | Outputs text to the console. |
| `CLS` | Clears the console screen. |
| `SYSINFO` | Displays system information. |
| `MEMINFO` | Displays memory information. |
| `REBOOT` | Requests a system restart. |
| `SHUTDOWN` | Requests system shutdown. Complete power-off functionality may depend on the current implementation. |

Command behavior and availability are determined by the released version.

## Screenshots

Screenshots will document Theia OS as it appears during actual execution.

Planned screenshot subjects include:

- Theia Console.
- Available commands.
- System information.
- Memory information.
- The operating system running in a virtual machine.

Screenshots should be captured from a genuine running build of Theia OS.

<!-- Add verified screenshots here when available. -->

## Compatibility

The current development target is a BIOS-based, 32-bit x86 environment.

Virtual machines are used to support development and testing. Compatibility with additional virtual machine platforms and physical hardware depends on testing and future development.

Refer to the release notes for the specific requirements and limitations of each published version.

## Download

Official release downloads will be made available through the repository's **GitHub Releases** section.

The planned distribution format is a floppy disk image:

`TheiaOS-0.15.img`

The image is intended to allow users to run Theia OS without access to its private source code.

When available, a release may also include a SHA-256 checksum to help verify the integrity of the downloaded image.

Only files explicitly published as official releases should be considered official distribution builds.

## Project Status

**Theia OS 0.15 — Basic Functionality: Completed.**

The initial development phase is complete. The project will continue evolving through future development phases, with additional functionality introduced incrementally.

Theia OS is an independent project under active development. Its architecture, interfaces, and capabilities may change in subsequent versions.

Completion of version 0.15 does not constitute a claim of production readiness, comprehensive security validation, or compatibility with every computer.

## Development Philosophy

Theia OS follows three main development priorities:

1. **Core functionality** — establish the fundamental capabilities of the operating system.
2. **Security** — develop and strengthen the system's security architecture.
3. **Visual interface** — expand the user experience beyond the initial command-line environment.

These priorities guide the project's ongoing development.

## Source Code

Theia OS is closed-source software.

Its source code, internal implementation, and development history are maintained privately. This public repository is intended to provide project information, documentation, screenshots, and official release downloads.

Publishing a compiled release does not mean that the source code is publicly available.

## License and Usage

Theia OS is distributed under rights reserved by its author, except where explicit permissions are granted.

The applicable terms for downloading, executing, copying, modifying, and redistributing Theia OS should be specified in the official release documentation or a separate license agreement.

No open-source license is granted by this README.

## Acknowledgments

Thank you to everyone interested in Theia OS and its development.

Every milestone contributes to building an independent operating system from the ground up.

---

**Theia OS — Built independently. Developed incrementally.**
