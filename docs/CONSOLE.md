# Theia Console

This document describes Theia Console, the interactive command-line interface included in Theia OS.

## 1. Overview

Theia Console is the interactive text interface used to communicate with the current development build of Theia OS.

It provides a command prompt, accepts user input, and processes supported commands.

The console prompt is:

`theia>`

Theia Console is part of the current development architecture. Its implementation and long-term role may evolve in future versions.

## 2. Development History

### Version 0.5 — First Interactive Console

Version 0.5 introduced the first interactive console.

Initial commands:

- `ECHO`
- `CLS`
- `VERSION`

This milestone established the initial interactive command-line experience of Theia OS.

### Version 0.6 — Name Not Assigned

Version 0.6 continued development of the interactive console. No official milestone name has been assigned.

### Version 0.7 — Kernel Time

Version 0.7 is identified by the Kernel Time milestone. This is a kernel development milestone rather than an official console milestone name.

### Versions 0.8–0.11 — Memory Development

These versions focused on memory-related milestones:

- **0.8 — Memory Foundation**
- **0.9 — Memory Region Manager**
- **0.10–0.11 — Memory Allocator Foundation work**

The console remained part of the broader operating-system development process.

### Version 0.12 — Input Foundation

Version 0.12 established the console input foundation.

### Versions 0.13–0.14 — Console Expansion

These versions continued expanding console functionality.

### Version 0.15 — Basic Closed

Version 0.15 marks the completion of the Basic Functionality development phase.

## 3. Available Commands

The following commands are documented for the current development build.

| Command | Description |
|---|---|
| `ECHO` | Displays text. |
| `HELP` | Displays command help. |
| `VERSION` | Displays version information. |
| `CLS` | Clears the console screen. |
| `SYSINFO` | Displays system information. |
| `MEMINFO` | Displays memory information. |
| `REBOOT` | Requests a system restart. |
| `SHUTDOWN` | Initiates the shutdown request sequence. |

Command availability and behavior must be verified against the exact build being tested.

## 4. Command Behavior

### ECHO

Displays the text provided as input.

Example:

`theia> ECHO Hello`

### HELP

Displays information about supported commands.

Example:

`theia> HELP`

### VERSION

Displays version information for Theia OS.

Example:

`theia> VERSION`

### CLS

Clears the console screen.

Example:

`theia> CLS`

### SYSINFO

Displays system information provided by the current implementation.

Example:

`theia> SYSINFO`

### MEMINFO

Displays memory information provided by the current implementation.

Example:

`theia> MEMINFO`

### REBOOT

Requests a system restart.

The exact reset behavior depends on the current implementation and should be verified in the target environment.

Example:

`theia> REBOOT`

### SHUTDOWN

Initiates the shutdown request sequence.

A shutdown request is not necessarily equivalent to complete hardware power-off. The behavior must be evaluated against the current implementation.

Example:

`theia> SHUTDOWN`

## 5. Input and Command Processing

The console depends on the input-handling facilities implemented in the current build.

Its general interaction model consists of:

1. Displaying the command prompt.
2. Receiving user input.
3. Identifying the requested command.
4. Processing recognized commands.
5. Displaying output or handling unrecognized commands.
6. Returning to the prompt when appropriate.

This sequence describes the general interaction model. Exact behavior depends on the implementation.

## 6. Unknown Commands

Input that does not match a supported command should be handled according to the current command-processing implementation.

Error messages and recovery behavior should be documented after they have been verified in a running build.

## 7. Current Scope

Theia Console is a text-based interface.

This document does not claim the existence of a complete graphical shell, advanced command scripting, or other features that have not been verified.

Console capabilities may change as Theia OS develops.

## 8. Testing

Console testing should verify:

- Prompt display.
- Keyboard input.
- Command recognition.
- Command output.
- Handling of unknown commands.
- Screen clearing.
- System and memory information.
- Restart behavior.
- Shutdown behavior.

Results should be recorded for the specific version and virtual-machine configuration tested.

---

**Document version:** Theia OS 0.15  
**Component:** Theia Console  
**Status:** Technical documentation
