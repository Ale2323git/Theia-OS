# Mashel

**Mashel** is the official name of the Theia OS interactive console component.
It runs in Ring 3 and presents the `theia>` prompt. "Theia Console" is a
historical name only; it is not the current component name.

## Role in the system

The kernel does not start Mashel directly. The mandatory boot coordinator
governs the handoff, kernel initialization creates Init in Ring 3, and Init
creates Mashel through public system interfaces.

Mashel provides the user-visible text environment on top of the public System
API. It is a system component, not a general-purpose Unix shell or a native
application loader.

## Public services used

Mashel consumes public system services for:

- text output;
- system information and uptime;
- authorized communication with system components.

Mashel does not call privileged or private interfaces directly. It does not read
or write display memory, keyboard ports, or other hardware registers. Privileged
work is handled by the system through defined internal channels that are not
published.

## User-visible behavior

The published 0.18 image was verified in the reference virtual environment
showing:

- the Theia OS Mashel banner;
- the message `Type 'HELP' for a list of commands.`;
- the `theia>` prompt on text output.

The command set covers help, system information, memory information, uptime,
text output, screen clearing, reboot, and shutdown. The complete interactive
command matrix has not been exhaustively revalidated command by command in the
published validation pass.

Commands for version, system information, memory information, and uptime use
public system queries. They are not private kernel interfaces.

## Status

| Item | Status |
| --- | --- |
| Ring 3 Mashel component | Implemented |
| Init-to-Mashel handoff | Verified in the reference virtual environment |
| Prompt and text output | Verified in the reference virtual environment |
| Use of public system services | Verified by documented architecture |
| Complete command-by-command validation | Limited |
| Physical-hardware validation | Limited |

## Related documentation

- [Boot Architecture](../Architecture/Boot-Architecture.md)
- [System Architecture](../Architecture/System-Architecture.md)
- [IPC and Synchronization](IPC-and-Synchronization.md)
- [System API](../System-API/System-API.md)
- [Running Theia OS](../Installation/Running-Theia-OS.md)

---

Theia OS is proprietary software. This document describes public Mashel
behavior and does not disclose private protocol details.
