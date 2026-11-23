# HAL and Drivers

## Documented scope

The documented 0.18 scope covers the platform support required to boot and
provide its user environment:

- keyboard input;
- clock access;
- firmware power-state information;
- text output support;
- interrupt and timing support;
- basic memory information from firmware.

There is no general third-party driver model in 0.18.

## Hardware access boundary

Ring 3 components, including Init and Mashel, do not access hardware directly.
Text output, keyboard input, reboot, shutdown, and display operations are
performed by privileged system code through defined interfaces.

This boundary is part of the public architecture. It is not an invitation to use
private privileged interfaces from userland.

## HAL Foundation

HAL Foundation is a deferred architectural decision. Public documentation ties
the general availability of Ring 1 and Ring 2 to that foundation. The verified
0.18 user environment uses Ring 0 and Ring 3; Ring 1 and Ring 2 are not public
userland facilities.

No implementation status or delivery date is claimed for HAL Foundation.

## Status

| Capability | Status |
| --- | --- |
| Basic keyboard, clock, power-state, and text output support | Implemented with limited hardware validation |
| USB, audio, networking, accelerated graphics | Not implemented |
| General third-party driver model | Not implemented |
| HAL Foundation | Deferred decision |

## Related documentation

- [Kernel Architecture](../Architecture/Kernel-Architecture.md)
- [System Architecture](../Architecture/System-Architecture.md)
- [Boot Architecture](../Architecture/Boot-Architecture.md)
- [Roadmap](../Development/Roadmap.md)
- [Features](../Introduction/Features.md)

---

Theia OS is proprietary software. This document describes public HAL and
driver status and does not disclose private implementation details.
