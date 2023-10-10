---
title: Building Libreboot for Emulation
x-toc-enable: true
...

Introduction
============

Libreboot supports building for qemu as a target board.
The resulting rom can then be tested using qemu.

The qemu board is mostly intended to speed up development by removing the need to flash to bare metal during initial tests.
Qemu may also be useful for end users who intend to make changes to their libreboot rom without having to flash and reboot their machine.

Building and Testing
====================

Libreboot can be built for qemu just like any other board.

	./build fw coreboot qemu_x86_12mb

In order to test the resulting roms, you must have qemu installed on the host machine.
Test the roms by pointing qemu to the rom in bios mode.
For example:

`qemu-system-x86_64 -bios bin/qemu_x86_12mb/grub_qemu_x86_12mb_libgfxinit_corebootfb_usqwerty_noblobs.rom`

`qemu-system-x86_64 -bios bin/qemu_x86_12mb/uboot_payload_qemu_x86_12mb_libgfxinit_corebootfb_noblobs.rom -serial stdio`

There is basic support for an arm64 virtual machine as well, although the payloads are not as developed as the x86 one:

	./build fw coreboot qemu_arm64_12mb

```
qemu-system-aarch64 -bios bin/qemu_arm64_12mb/uboot_payload_qemu_arm64_12mb_libgfxinit_corebootfb_noblobs.rom \
	-M virt,secure=on,virtualization=on,acpi=on -cpu cortex-a53 -m 768M -serial stdio -vga none -display none
```

NOTE: After the 20230625 release, U-Boot support was removed from x86 Qemu; it's
also problematic on ARM-based Qemu. Only x86 qemu, with other payloads, is to
be considered reliable for now.

Use Cases
=========

While development is the primary motivation for qemu support, the board makes it easy to test minor changes to release roms.
For example one can use *cbfstool* from coreboot to edit the background image in a libreboot rom as follows:

```
cbfstool /path/to/rom remove -n background.png
cbfstool /path/to/rom add -f mynewbackground.png -n background.png -t raw
```

Using qemu allows the user to verify that the background image renders properly before performing the same operation on their release rom.
