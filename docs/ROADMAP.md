# Theia OS — Roadmap

## 1. Purpose

This roadmap describes the development direction of Theia OS.

It distinguishes completed milestones, ongoing development, planned features, and long-term concepts. A feature listed as planned is not necessarily implemented or available in a released version.

The roadmap may change as development progresses.

## 2. Project Principles

Theia OS follows three main development priorities:

1. **Core functionality:** establish reliable operating system fundamentals.
2. **Security:** strengthen system protection as the architecture matures.
3. **Visual interface:** introduce graphical capabilities after the underlying foundations are sufficiently developed.

Theia OS is an independent operating system project with its own bootloader and kernel. Its architecture and roadmap are developed specifically for Theia OS.

## 3. Development Phases

The project is organized into successive development phases.

### Phase I — Basic Functionality

**Milestone: 0.15 — Basic Closed**

The 0.15 milestone marks the planned closure of the Basic Functionality phase.

This phase establishes the initial boot process, kernel foundations, memory-management work, and an interactive text console.

The completion of this phase does not mean that every operating system feature is complete. It marks the end of a defined development stage.

### Phase II — Theia OS Core

The next phase focuses on developing the operating system's internal foundations beyond the initial functionality milestone.

The specific implementation sequence and release contents will be documented as they are finalized.

### Phase III — Identity

This phase is intended to further establish Theia OS's own system identity, interfaces, and architectural conventions.

Individual features remain subject to design decisions and implementation.

### Phase IV — User

This phase represents a later stage of development focused on the user-facing system experience.

Its eventual scope will depend on the maturity of the core architecture and the results of earlier phases.

## 4. Completed Milestones

The following milestones have been identified in the project's development history.

| Version | Milestone | Summary |
|---|---|---|
| 0.1 | Proof of Boot | Initial boot proof |
| 0.5 | First Interactive Console | Interactive console with `ECHO`, `CLS`, and `VERSION` |
| 0.7 | Kernel Time | Kernel timing milestone |
| 0.8 | Memory Foundation | Initial memory-management foundation |
| 0.9 | Memory Region Manager | Memory-region management |
| 0.15 | Basic Closed | Closure milestone for the Basic Functionality phase |

Other intermediate versions introduced additional development work. Consult `CHANGELOG.md` for the recorded release history.

## 5. Planned Development Areas

The following areas describe development directions rather than a guarantee of availability in a particular release.

### Kernel and Memory Management

- Continue developing kernel functionality.
- Improve memory allocation and memory-region handling.
- Strengthen the separation between implemented functionality and future interfaces.
- Expand system foundations as required by later phases.

### Console and Command Interface

- Continue developing the text-based system interface.
- Expand command functionality as appropriate.
- Evolve the command interface in accordance with the project's architecture.

### System Architecture

- Develop Theia OS's internal system interfaces.
- Continue defining the responsibilities of essential system components.
- Introduce additional system services when their foundations are ready.

### Filesystem and Applications

- Develop the foundations for TNTFS.
- Establish the interfaces needed by future applications.
- Document file and application formats as they become sufficiently defined and implemented.

### User Experience

- Evaluate future graphical and user-facing capabilities after the core foundations mature.
- Preserve a clear distinction between the current text-based environment and planned interface improvements.

## 6. Compatibility Roadmap

Theia OS 0.15 currently targets legacy BIOS and 32-bit x86.

Potential future compatibility work includes:

- UEFI support.
- Native x86-64 support.
- Expanded virtual-machine testing.
- Testing on physical hardware.

These are future development areas, not claims of existing support. Each requires implementation and verification before being described as available.

## 7. Long-Term Architectural Concepts

The project has longer-term architectural ideas that extend beyond the immediate release sequence.

These concepts may include:

- A more comprehensive system architecture and service model.
- Stronger security boundaries.
- Additional application and system interfaces.
- Exploration of ternary computing on suitable physical hardware.

These ideas are exploratory or long-term unless a specific implementation is documented elsewhere. They should not be interpreted as features of Theia OS 0.15.

## 8. Release Policy

Each release should document its actual functionality and known limitations.

A release number or roadmap entry does not, by itself, establish that a feature is implemented. Features should be described as available only when the corresponding implementation has been completed and verified.

Release plans may change to accommodate technical findings, testing results, and architectural decisions.

## 9. Scope of This Roadmap

This roadmap describes the intended direction of Theia OS without exposing its private source code or internal build procedures.

For current capabilities and compatibility information, consult:

- `CHANGELOG.md`
- `docs/ARCHITECTURE.md`
- `docs/BOOT-PROCESS.md`
- `docs/CONSOLE.md`
- `docs/MEMORY-MANAGEMENT.md`
- `docs/COMPATIBILITY.md`
- `docs/BUILD-AND-TEST.md`

---

**Document version:** Theia OS 0.15  
**Status:** Development roadmap  
**Source code:** Closed-source
