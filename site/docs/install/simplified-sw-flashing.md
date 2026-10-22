---
title: Simplified Software Flashing 
x-toc-enable: true
...

*Got here from an device installation page? Good, proceed and don't be a lazy sloth by skipping if you are a *newbie*.
Do so and get yourself bricked just fine.*

**If your device had a Simple tag on the supported devices page or you’re already running Libreboot, and you don’t have flash protection turned on, [internal flashing](../install/) is possible.**

### Disable /dev/mem protection

*Before internal flashing, [Disable /dev/mem protection](devmem.md) first.*

*In short add this kernel parameter: iomem=relaxed(linux distro)/kernel.securelevel=-1(bsd distro)*

**THEN REMOVE THIS KERNEL PARAMETER TO PREVENT UNAUTHORIZED/MALICIOUS ACCESS TO YOUR SYSTEM AFTER FLASHING LIBREBOOT.**

(An important reminder for those who might forget to delete it, even better, replace it with lockdown=integrity / lockdown=confidentiality)


### Backup the previous (*working*) firmware 

Do it twice (to check and compare if reading it gone wrong and backups differs.)

	flashprog -p internal -r Backup1.bin

	flashprog -p internal -r Backup2.bin

### Check the backups

Compare those if reading from the flash were successful:

	diff Backup1.bin Backup2.bin

For more than 2:

	shasum Backup1.bin Backup2.bin Backup3.bin

If those differs, something gone wrong and proceeding further may **(DON'T) leave your device unrecoverable** (without a reliable backup) if flashing fails as well.


### Flash the new firmware

**Replace the libreboot.rom with your device's filename.rom found on the installation page and DO NOT SHUT DOWN or REBOOT it after flashing libreboot, if running on vendor firmware.**

	flashprog -p internal -w libreboot.rom


*Made and checked those backups, aren't you? If you don't, whatever, your responsibility, but don't cry/blame others if you dont have those if anything goes wrong to fall back...*

*In case if it gone wrong, feel free to ask for help on IRC.*

**Use sync in the terminal to avoid data loss/corruption, then turn off the psu (power supply) or pull the cable to avoid the factory one overwriting the fresh (libreboot) installation, especially with dell ones.**


### Enable /dev/mem protection

**REMOVE *iomem=relaxed(linux distro)/kernel.securelevel=-1(bsd distro)* KERNEL PARAMETER TO PREVENT UNAUTHORIZED/MALICIOUS ACCESS TO YOUR SYSTEM AFTER FLASHING LIBREBOOT.**

(An important reminder for those who might forget to delete it, even better, replace it with lockdown=integrity / lockdown=confidentiality)


[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"
