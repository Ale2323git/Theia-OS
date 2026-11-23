# Running Theia OS

This guide describes how to run a published Theia OS disk image. It does not
provide build instructions; the source tree and build process are private.

## Requirements

- a published Theia OS `.img` image from
  [GitHub Releases](https://github.com/Ale2323git/Theia-OS/releases);
- a BIOS-class x86 virtual machine or compatible hardware;
- no GRUB or UEFI installation is required.

## Running a published image

Attach the published image as a boot disk to a compatible virtual machine or
compatible hardware, then start the system. Do not convert the image to another
format unless the release notes explicitly say so.

## Expected result

A successful boot reaches:

- the Theia OS Mashel banner;
- `Type 'HELP' for a list of commands.`;
- the `theia>` prompt on text output.

The published image presents the user environment without development
diagnostics. Critical failures still use a defined presentation and end safely.

## Validation limits

- The published 0.18 image was validated in the reference virtual environment.
- Physical-hardware compatibility is limited and not broadly claimed.
- The complete command matrix has not been exhaustively revalidated command by
  command in the published validation pass.
- No persistent filesystem, native application loader, networking, USB, audio,
  or graphical desktop is available in 0.18.

## Troubleshooting

| Symptom | Check |
| --- | --- |
| No boot activity | Confirm BIOS boot mode and that the image is attached as a disk |
| Boot stops with a diagnostic | Record the exact visible message and check the release notes |
| Prompt does not appear | Confirm that the environment supports text-mode output |

Report reproducible issues through the
[issue tracker](https://github.com/Ale2323git/Theia-OS/issues) with the release,
execution environment, steps, and observed output. Do not include private
implementation material.

## Related documentation

- [Boot Architecture](../Architecture/Boot-Architecture.md)
- [Mashel](../Components/Mashel.md)
- [Development Status](../Development/Development-Status.md)
- [Build and Validation](../Development/Build-and-Validation.md)

---

Theia OS is proprietary software. This guide describes public image usage only.
