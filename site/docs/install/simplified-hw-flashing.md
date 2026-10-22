---
title: Simplified Software Flashing 
x-toc-enable: true
...

**If your device needs to be flashed with an external SPI programmer, [25xx NOR flash
instructions](../install/spi.md) gives information on how to set it up.**

**If needed, partially or fully disassembly your device before proceeding further accoding to it's guide page**

**Some flash chips might need it's motherboard to be powered (but turned off) from an external power source, in that case, disconnect the vcc/power cable between the clip and the programmer, or it might be damaged and fried.**


### Backup the previous (*working*) firmware

*In case you may need to specify the chip with -C "XXX/XXX" from the listed options, make sure you select the option containing your model, read from the flash chip itself on the motherboard.*

Do it twice (to check and compare if reading it gone wrong and backups differs.)

	flashprog -p serprog:dev=/dev/ttyACM0,spispeed=16M -r Backup1.bin

	flashprog -p serprog:dev=/dev/ttyACM0,spispeed=16M -r Backup2.bin


### Check the backups

Compare those if reading from the flash were successful:

	diff Backup1.bin Backup2.bin

For more than 2:

	shasum Backup1.bin Backup2.bin Backup3.bin

If those differs, something gone wrong and proceeding further may **(DON'T) leave your device unrecoverable** (without a reliable backup) if flashing fails as well.

*If something gone wrong, feel free to ask for help on IRC.*


### Flash the new firmware

Instructions continues on the device's page.


[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"
