# Theia OS

**An independent operating system built from the ground up.**

Theia OS is an independent, closed-source operating system developed using freestanding C and NASM assembly. It features its own BIOS bootloader, kernel, and interactive command-line interface, establishing the foundation for a future operating system with its own architecture and design.

## Overview

Theia OS is developed as an independent operating system project, with an emphasis on core functionality, system architecture, and gradual development.

Rather than relying on an existing operating system kernel or an external bootloader, Theia OS is designed around its own boot process and kernel.

## Current Development

Theia OS is currently in its early development phase. Development focuses on establishing the fundamental components required for a functional operating system.

Current development areas include:

- Custom BIOS bootloader.
- Freestanding C kernel.
- NASM assembly routines.
- Memory region detection and management.
- Interactive Theia Console.
- Basic system information and command handling.

Features are introduced incrementally and documented according to their actual implementation status.

## Theia Console

Theia Console provides a command-line interface for interacting with the operating system.

The current command set includes:

| Command | Description |
|---|---|
| `HELP` | Displays available commands. |
| `VERSION` | Displays version information. |
| `ECHO` | Outputs text. |
| `CLS` | Clears the console screen. |
| `SYSINFO` | Displays system information. |
| `MEMINFO` | Displays memory information. |
| `REBOOT` | Requests a system restart. |
| `SHUTDOWN` | Requests system shutdown; complete power-off functionality is still under development. |

Command availability and behavior may change between development versions.

## Screenshots

Screenshots of Theia OS will be published here as the project progresses.

<!-- Add verified screenshots here after capturing them from a running build. -->

## Compatibility

The current development target is a BIOS-based, 32-bit x86 environment.

Virtual machines are used for development and testing. Compatibility with additional environments and hardware will depend on future development and testing.

## Download

Official release images will be published through the repository's **GitHub Releases** section when available.

Released disk images are intended to let users try Theia OS without requiring access to its source code.

**Note:** The project is under active development. Releases may have limited functionality and should not be considered production-ready unless explicitly stated.

## Project Status

Theia OS is a work in progress. Its architecture, features, and interfaces may evolve as development continues.

The public repository provides project information, documentation, screenshots, and official release downloads. It does not distribute the private source code.

## License and Usage

Theia OS is closed-source software. All rights are reserved unless explicitly granted by the project's author.

Downloading or receiving a release does not automatically grant permission to modify, redistribute, reverse engineer, or commercially exploit the software. Any permitted use will be governed by the applicable license or distribution terms published with the release.

Documentation and screenshots may be subject to separate usage conditions.

## Acknowledgments

Thank you to everyone who takes an interest in Theia OS and follows its development.
